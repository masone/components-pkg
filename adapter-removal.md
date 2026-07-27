# Adapter removal — findings & precondition

> Investigation of `src/components/adapter/*` (27 folders / ~28 exports) and the decision
> that removing them gates everything else.

---

## Decision

- **Adapter removal is a precondition.** We start cleaning up components-pkg **only when
  (a) the adapters are gone and (b) everything still in use exposes a Chakra v3 interface.**
- **Per-adapter call:** decide whether it can *reasonably migrate to the v3 API*.
  - **Yes → remove the adapter, expose the v3-native component** (Chakra primitive or
    design-system component with a v3 interface).
  - **No → initially treat it as a shared component, *not* a Chakra primitive.** This
    reduces the risk of anyone expecting Chakra APIs on something that isn't cleanly Chakra.
- This is consistent with "lean into Chakra": removal strips a v2-flavoured layer and
  exposes v3-native props — it does **not** push us toward abstracting Chakra away (see below).

---

## What the adapters actually are (four kinds, not one)

They are **not** uniformly "prop renames." Reading across the set, they fall into four
distinct kinds with very different removal costs.

### ① Pure v2→v3 renames — trivial (the majority)
Mechanical prop/name mapping; removal = a codemod on call sites.
`Divider` is 9 lines — it just re-exports `Separator`:
```tsx
export const Divider: FC<SeparatorProps> = (props) => <Separator {...props} />;
```
Also (13 total): `Divider`, `Stack` (`spacing→gap`), `Text` (`isTruncated→truncate`,
`noOfLines→lineClamp`, `textColor→color`, `align→textAlign`), `Button`
(`isDisabled→disabled`, `isLoading→loading`), `Input`
(`isDisabled/isReadOnly/isInvalid/textColor` → v3), `Skeleton`, `Chip`, `FormControl`,
`FormLabel`, `SimpleGrid`, `Select`, `List`, `Tab`.

> **Codemod caveat — `Skeleton` negates:** it maps `loading={!isLoaded}`, so the codemod must
> *invert* the flag, not just rename it. A naive rename silently flips skeleton states.

### ② Event-model bridges — high effort, the real crux
Not renames — they **rebuild a fake synthetic `ChangeEvent`** from Chakra v3's detail
callbacks so old `onChange(event)` code (and **react-hook-form**) keeps working. `Switch`:
```tsx
const syntheticEvent = { target, currentTarget: target } as ChangeEvent<HTMLInputElement>;
onChange(syntheticEvent);   // bridges v3 onCheckedChange({checked}) → v2 onChange(event)
```
`Checkbox` does the same. (Verified from source: `Input` does **not** — it only renames
flags; and `Tabs` unwraps `{value}` to a plain string without a synthetic event. So the true
event-bridges are **just `Checkbox` and `Switch`** — two adapters.) **This is the RHF /
synthetic-event tension** flagged early: Chakra v3 moved off synthetic events, RHF still
expects them. Removing these forces every form + RHF integration onto the v3 event model.
*This is the hardest part of the whole refactor — a real decision, not a codemod.*

### ③ Component renames / API reshapes — medium
`Modal` maps the v2 Modal API onto v3 `Dialog`:
```tsx
<Dialog {...rest} open={isOpen} onOpenChange={(e) => { if (!e.open) onClose?.(); }} />
```
`Table` maps the flat v2 API (`Thead/Tbody/Th/Td`, `isNumeric`) onto the v3 compound
`Table.Header/Body/…`. `Tabs` maps `onChange(value)` from v3 `onValueChange({value})`
(unwraps to a plain string, so — unlike `Checkbox`/`Switch` — it does **not** collide with
RHF; low effort). Removal = migrating call sites to the v3 component shape.

### ④ Genuine behavior mislabeled as "adapter" — do NOT remove; reclassify & keep
These aren't compat shims at all — they carry real behavior. `Popover`:
```tsx
const [supportsHover] = useMediaQuery(['(hover: hover) and (pointer: fine)']);
if (usesHoverCard) return <HoverCard {...rest} />;   // desktop→hover, touch→click fallback
```
Also: `Hide`/`Show` (reimplement components v3 removed), `Box` (polymorphism `as`/`asChild`
+ link handling), and the custom-named `Link`, `SelectMenu`, `PopoverFilter`,
`RangeFilterInputWithSlider`, `InsertionLayout`. **Deleting these deletes functionality** —
they were dumped in the wrong folder. Reclassify into `components`/`features` and keep.

---

## Why removal leans *into* Chakra, not away

- **Removing ①②③ exposes v3-native props/components** — it strips a v2-flavoured abstraction
  layer. That's leaning in.
- **The "abstract Chakra away" trap is elsewhere:** (a) *keeping* the renamed dialect forever
  (the dialect itself is a soft abstraction over Chakra), or (b) treating a ④ behavior
  component like `Popover` as a shim and rebuilding it from scratch. Neither is required.

---

## Effort & risk

| Kind | Effort | Migrate to v3, or reclassify? |
|---|---|---|
| ① renames | **Low** — mechanical codemod | Migrate → v3-native |
| ② event bridges (Switch/Checkbox) | **High** — RHF decision | Migrate *if* RHF integration resolved; else shared component |
| ③ reshapes (Modal→Dialog, Table) | **Medium** — call-site migration | Migrate → v3-native |
| ④ behavior (Popover, Box, Hide/Show, Link, filters…) | **Not a removal** | Reclassify as shared component; keep behavior |

**The "reasonably migratable?" gate maps onto this:** ①③ yes; ② yes-with-a-decision; ④ no →
shared component (custom API, no Chakra expectation).

---

## Risks / crux

1. **Form controls + react-hook-form (②).** The synthetic-event bridge is the one genuinely
   hard sub-problem. "Lean into Chakra" here means adopting v3's event model, which needs an
   RHF-integration decision (e.g. RHF `Controller` wrapping `onCheckedChange`) — not a
   codemod.
2. **Don't mistake ④ for shims.** Reclassify behavior components; deleting them loses real
   functionality (`Popover`'s hover/touch fallback, etc.).

---

## Verified catalogue

The full per-adapter catalogue (all 27 folders / 28 exports, read from source) lives in
`component-inventory-adapters.md`. Counts: **① 13 · ② 2 · ③ 3 · ④ 10.** Only **`Checkbox` and
`Switch` (②)** need a *decision* (react-hook-form); everything else is a mechanical codemod
(16) or a reclassify-and-keep (10, no deletion).

Corrections that catalogue made to this doc (from reading actual code): `Input` is ① not ②
(rename-only, no synthetic event); `Skeleton` negates its flag (`loading={!isLoaded}`); `Tabs`
is ③ not ② (unwraps `{value}`, no RHF collision).
