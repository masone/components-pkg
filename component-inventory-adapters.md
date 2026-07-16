# Adapter catalogue — `src/components/adapter/*`

> Removal plan for the legacy Chakra-v2→v3 compatibility layer. Reading of the **actual
> code** of every folder under `src/components/adapter/` (27 folders, 28 exports — `Tab/`
> ships both `Tab` and `Tabs`). Framework: `adapter-removal.md` (four kinds ①②③④ +
> migrate-or-reclassify rule). Analysis only; no source modified.

Each adapter is a thin wrapper over an already-v3-native component under
`@/src/components/<name>` (or a raw Chakra import). The adapter's only job is to accept the
legacy v2 prop/callback names and forward the v3 ones — so the "Prop / API map" column is
the exact codemod spec.

## Catalogue

| Adapter | Kind | Prop / API map (legacy → v3) | Reasonably migratable to v3? | Action | Effort |
|---|---|---|---|---|---|
| **Box** | ④ | `textColor`→`color`; `spacing`→`gap`; keeps `color`; polymorphism `as` (string vs component) with `asChild` branch; NextLink compat (`href`, `prefetch`) — passes `href` on string `as`, uses `asChild` + `<AsComp href prefetch>` for component `as` | **no** (real polymorphism + link-injection logic, not a rename) | reclassify → shared component (no Chakra API expected) | high |
| **Button** | ① | `isDisabled`→`disabled` (`disabled: isDisabled ?? disabled`); `isLoading`→`loading` | **yes** | remove → v3-native | low |
| **Checkbox** | ② | `isChecked`→`checked`; `isDisabled`→`disabled`; `isInvalid`→`invalid`; `onChange(event)` rebuilt from v3 `onCheckedChange(details)` — synthetic `ChangeEvent` with `target={type:'checkbox',name,checked,value:''}`; requires `name` | **yes-with-decision** (RHF / synthetic-event) | **decision outstanding** (see ② options) | high |
| **Chip** | ① | `isActive`→`selected`; `isDisabled`→`disabled` (default `false`) | **yes** | remove → v3-native | low |
| **Divider** | ① | `Divider`→`Separator` (props passed straight through as `SeparatorProps`) | **yes** | remove → v3-native | low |
| **FormControl** | ① (component rename) | `FormControl`→`Field`; `isRequired`→`required`; `isDisabled`→`disabled`; `id`/`label`/`errorMessage`/`size`/`tooltip`/`hint`/`labelButtonText`/`labelButtonOnClick` pass through unchanged | **yes** | remove → v3-native | low |
| **FormLabel** | ① (component rename) | `FormLabel`→`Field.Label` wrapped in `Field.Root`; children only, no props mapped | **yes** | remove → v3-native | low |
| **Hide** | ④ (B1) | `above`→`hideFrom`; `below`→`hideBelow` (on raw Chakra `Box`) — v3 removed `Hide` but ships `hideFrom`/`hideBelow` as **native style props** | **removable** | **migrate to native `hideFrom`/`hideBelow`; delete** (not kept) | low |
| **Input** | ① | `isDisabled`→`disabled`; `isReadOnly`→`readOnly`; `isInvalid`→`invalid`; `textColor`→`color`. **NOTE:** current code is a pure rename — it does **NOT** rebuild a synthetic event (no `onChange` handling), contrary to `adapter-removal.md`'s claim that "Checkbox and Input do the same." | **yes** | remove → v3-native | low |
| **InsertionLayout** | ④ | custom-named re-export of `SingleColumnCenteredLayout`; only `maxContentWidth?: keyof sizes.container` — not a Chakra shim | **no** | reclassify → shared component (no Chakra API expected) | low |
| **Link** | ④ | `noOfLines`→`lineClamp`; `isTruncated`→`truncate`; `textColor`→`color`; `onClick`/`href` null-coalesced to `undefined`; injects `leftIcon`/`rightIcon` around children; NextLink compat (`as` component + `prefetch` ⇒ `asChild` + `<AsComp href prefetch replace>`) | **no** (icon injection + NextLink asChild branch is real behavior) | reclassify → shared component (no Chakra API expected) | high |
| **LinkOverlay** | ④ | NextLink compat: `as` component + `prefetch` ⇒ `asChild` + `<AsComp href prefetch>`; else pass `href` through; re-exports `LinkBox` | **no** (link-injection logic) | reclassify → shared component (no Chakra API expected) | medium |
| **List** | ① | `spacing`→`gap` on `List.Root` (forces `as="ul"`); also exports `UnorderedList` (`listStyleType:"initial"`, `variant:'icon-outside'\|'icon-inside'`), `OrderedList` (`listStyleType:"decimal"`), `ListItem` = `List.Item` | **yes** | remove → v3-native | low–medium |
| **Modal** | ③ | `Modal`→`Dialog`; `isOpen`→`open`; `onClose`→`onOpenChange((e)=>{ if(!e.open) onClose?.() })`; `ModalCloseButton`→`ChakraDialog.CloseTrigger asChild` + `DialogCloseButton` | **yes** | remove → v3-native | medium |
| **Popover** | ④ | `trigger:'hover'\|'click'` (default `hover`); on hover-capable devices (`useMediaQuery('(hover: hover) and (pointer: fine)')`) returns **`HoverCard`**, else click `Popover` owning its own open state; `isOpen`→controlled `open`; `onOpen`/`onClose`→`onOpenChange`; `closeOnBlur`→`closeOnInteractOutside` fallback | **no** (device-based hover/touch fallback is real behavior) | reclassify → shared component (no Chakra API expected) | high |
| **PopoverFilter** | ④ | `isDisabled`→`disabled` (default `false`) over the custom `filterPatterns/popover` component | **no** (domain filter component, not a Chakra primitive) | reclassify → shared component (no Chakra API expected) | low |
| **RangeFilterInputWithSlider** | ④ | `isDisabled`→`disabled`; `onFocus` passthrough; generic over `<NFrom, NTo extends string>`; wraps custom `rangeFilterInputWithSlider` | **no** (domain filter component) | reclassify → shared component (no Chakra API expected) | low |
| **Select** | ① | `isDisabled`→`disabled`; `isInvalid`→`invalid` | **yes** | remove → v3-native | low |
| **SelectMenu** | ④ (really a ①-style dialect) | reshapes `options→items`, `withIndicator→showOptionsCheckmark` (default flip), `leftIcon→icon` — **pure rename over the SMG `Menu`** (two layers off Chakra), no new behavior | **n/a — redundant** | **merge into `Menu`; delete** (migrate `options→items` etc.) | low |
| **Show** | ④ (B1) | `above`→`hideBelow`; `below`→`hideFrom`; `showDisplay`→`display` (on raw Chakra `Box`) — v3 removed `Show`; use native `hideFrom`/`hideBelow` | **removable** | **migrate to native props; delete** (not kept) | low |
| **SimpleGrid** | ① | `spacing`→`gap`; `spacingX`→`gapX`; `spacingY`→`gapY`; `columns` passthrough | **yes** | remove → v3-native | low |
| **Skeleton** | ① | `isLoaded`→`loading` **inverted**: `loading={!isLoaded}` | **yes** (mind the inversion in the codemod) | remove → v3-native | low |
| **Stack** | ① | `spacing`→`gap` (over raw Chakra `Stack`) | **yes** | remove → v3-native | low |
| **Switch** | ② | `isChecked`→`checked`; `isDisabled`→`disabled`; `onChange(event)` rebuilt from v3 `onCheckedChange(details)` — synthetic `ChangeEvent` with `target={checked}` only (no `name`/`value`) | **yes-with-decision** (RHF / synthetic-event) | **decision outstanding** (see ② options) | high |
| **Tab** (`Tab/Tab.tsx`) | ① | `isDisabled`→`disabled`; `marginX`, `value` passthrough | **yes** | remove → v3-native | low |
| **Tabs** (`Tab/Tabs.tsx`) | ③ | `onChange(value:string)` reshaped from v3 `onValueChange({value})` — unwraps the detail object; **not** a synthetic `ChangeEvent` (plain string, no RHF collision) | **yes** | remove → v3-native | low |
| **Table** | ③ | flat→compound: `Table`=`Table.Root`, `Thead`=`Table.Header`, `Tbody`=`Table.Body`, `Tr`=`Table.Row`, `Tfoot`=`Table.Footer`, `Th`=`Table.ColumnHeader`, `Td`=`Table.Cell`; `isNumeric`→`textAlign:'end'` (on `Th`/`Td`) | **yes** | remove → v3-native | medium |
| **Text** | ① | `noOfLines`→`lineClamp`; `isTruncated`→`truncate`; `textColor`→`color`; `align`→`textAlign` | **yes** | remove → v3-native | low |

## Summary

**Counts per kind (28 exports / 27 folders):**
- **① pure v2→v3 rename — 13:** Button, Chip, Divider, FormControl, FormLabel, Input, List, Select, SimpleGrid, Skeleton, Stack, Tab, Text.
- **② event-model bridge (synthetic `ChangeEvent`) — 2:** Checkbox, Switch.
- **③ component rename / API reshape — 3:** Modal (→Dialog), Table (flat→compound), Tabs (`onChange`←`onValueChange`).
- **④ genuine behavior mislabeled as adapter — 10:** Box, Hide, Show, Link, LinkOverlay, InsertionLayout, Popover, PopoverFilter, RangeFilterInputWithSlider, SelectMenu.

**Trivial codemods (① + the light ③ Tabs) — remove → v3-native, low effort:** Button
(`isDisabled/isLoading`), Chip (`isActive→selected`), Divider (→`Separator`), FormControl
(→`Field`, `isRequired/isDisabled`), FormLabel (→`Field.Label`), Input
(`isDisabled/isReadOnly/isInvalid/textColor` — pure rename), Select (`isDisabled/isInvalid`),
SimpleGrid (`spacing*→gap*`), Skeleton (`isLoaded→loading`, **inverted**), Stack
(`spacing→gap`), Text (`noOfLines/isTruncated/textColor/align`), Tab (`isDisabled`), Tabs
(`onChange←onValueChange`). List is low–medium (also ships `UnorderedList`/`OrderedList`
helpers, not just a rename).

**③ reshapes — see the detailed migration section below.**

**② — the RHF / synthetic-event crux (Checkbox, Switch): final decision OUTSTANDING.** Both
fabricate a fake `React.ChangeEvent<HTMLInputElement>` from v3's `onCheckedChange(details)` so
legacy `onChange(event)` handlers and **react-hook-form** keep working. Two options; the call
is **not yet made** (part of the separate RHF workstream):

- **Option A — remove → v3-native.** Migrate every form + RHF onto v3's
  `onCheckedChange({checked})` (e.g. RHF `Controller`). Fully idiomatic, but high effort and
  touches all forms — needs the RHF-integration decision first.
- **Option B — keep as legitimate custom `components`, retain the bridge.** The synthetic-
  event `onChange` (+ `ref` forwarding) is a justified reason to wrap, kept for now so RHF is
  untouched. Make everything *else* Chakra-esque: rename the v2 dialect (`isChecked→checked`,
  `isDisabled→disabled`, `isInvalid→invalid`) and expose native Chakra props (optionally also
  expose `onCheckedChange` for non-RHF consumers). Fold the bridge into
  `components/checkbox` / `components/switch`, drop the adapter layer. This **de-risks** ②:
  the prop renames proceed **now** (like ①, since RHF doesn't touch those props); only the
  eventual *bridge removal* waits on the RHF decision. Watch: the component must keep
  forwarding `ref` and building a faithful `target` while the rest is cleaned up.

**Final decision: outstanding.**

**④ — not a flat "reclassify & keep" (see "④ disposition — refined" below):** it splits into
Group A (custom behavior on a Chakra primitive — `Box`, `Link`, `LinkOverlay`, `Popover`),
Group B1 (removable via native v3 props — `Hide`, `Show`), and Group B2 (collapse the
redundant adapter into one component — `InsertionLayout`, `PopoverFilter`,
`RangeFilterInputWithSlider`, and `SelectMenu`, which is really a redundant dialect over the
SMG `Menu`).

**Notable discrepancies vs `adapter-removal.md` (from reading the real code):**
1. **Input is ①, not ②.** The current `Input/index.tsx` only renames
   `isDisabled/isReadOnly/isInvalid/textColor` and does **not** build a synthetic
   `ChangeEvent` — so the only genuine ② event-bridges are **Checkbox** and **Switch**.
2. **Skeleton inverts** the flag (`loading={!isLoaded}`) — the codemod must negate, not just
   rename.
3. **Tabs** reshapes the change callback (`onValueChange({value})` → `onChange(value)`) but
   passes a plain string, so — unlike Checkbox/Switch — it does **not** collide with RHF;
   it's a low-effort ③, not a ② bridge.

---

## ③ Reshapes — migration detail (Modal / Table / Tabs)

**The test for a ③:** does an SMG target component already exist (wrapping the renamed
Chakra v3 primitive) to migrate onto? For all three it does — `Dialog`, `Table`, `Tabs` — so
we **follow the rename** onto the SMG component. (If there were *no* Chakra equivalent, it
would be a custom component and we'd bake the wrapper into the API instead — but that's the
④ case, not these three.)

### Table — ✅ easy, pure search-replace. Go for it now.

Rename the flat elements to the `Table` compound; one prop transform:

- root `Table` → `Table.Root`
- `Thead` → `Table.Header` · `Tbody` → `Table.Body` · `Tfoot` → `Table.Footer`
- `Tr` → `Table.Row`
- `Th` → `Table.ColumnHeader` · `Td` → `Table.Cell`
- `isNumeric` (on `Th`/`Td`) → `textAlign="end"`

All mechanical.

### Tabs — ✅ easy, two small renames. Go for it now.

- `<Tab isDisabled>` → `<Tab disabled>`
- `<Tabs onChange={fn}>` → `<Tabs onValueChange={({ value }) => fn(value)}>`

Only wrinkle: the handler signature changes `(value)` → `({ value })` — trivial, but not a
blind string-swap.

### Modal — ⚠️ careful codemod, not a blind find-replace.

- `<Modal>` → `<Dialog>`
- `isOpen={x}` → `open={x}`  *(pure rename)*
- `onClose={fn}` → `onOpenChange={(e) => { if (!e.open) fn() }}`  *(semantic wrap, not a rename)*
- `ModalCloseButton` → reconcile with SMG `Dialog`'s own close handling (`Dialog` renders its
  own close when given a `title`), so it often **disappears** rather than mapping 1:1.

### Recommendation

Knock out **Table + Tabs** now via the ① flow (codemod call sites → repoint export → delete
adapter). **Modal** takes the same flow but needs a **careful/human** codemod for the
`onClose` wrap and the close-button reconciliation.

After removal, all three resolve to the SMG `components` `Dialog` / `Table` / `Tabs`, which
are then classified (**`components`**, generic design-system) and conformance-cleaned in the
next phase — note they carry their own debt (e.g. `components/tab` does
`Omit<ChakraTabsRootProps, 'variant'>`, a style-prop leak).

### ③ concerns (codemod safety)

- **Table — no real concern.** 1:1 structural map, no behavioral traps. Only note: it's an
  **import + JSX-element rewrite** (7 flat names → the compound `Table.*`), so a bigger — but
  still mechanical — change than an attribute swap.
- **Tabs — one footgun.** The change callback is a **signature change**, not a rename:
  `onChange(value)` ← `onValueChange({ value })`. A blind prop rename compiles but silently
  breaks (the handler receives an object, not a string) — the handler body must become
  `({ value }) => …`. Same class of trap as Skeleton's inversion.
- **Modal — real concerns; hand-migrate, don't automate:**
  1. `onClose → onOpenChange` is a **semantic wrap** (fires on open *and* close, passes an
     event): `onOpenChange={(e) => { if (!e.open) onClose() }}`. A naive rename breaks close.
  2. **`ModalCloseButton` is per-usage.** SMG `Dialog` renders its own close **only when given
     a `title`**. So a *titled* modal + an explicit `ModalCloseButton` → **duplicate** close
     buttons; a *titleless* modal that relied on `ModalCloseButton` → the close **disappears**.
     Each call site needs a look.

**Cross-cutting (low, not a blocker):** all three land consumers on the current SMG
`Dialog`/`Table`/`Tabs`, whose remaining debt is **mostly internal** (e.g. the
`Omit<ChakraTabsRootProps,'variant'>` style leak) rather than public-API — so these props
won't need a second migration. Double-migration risk is low.

---

## ④ disposition — refined (Group A / B1 / B2)

Checking each ④ against "does a same-named Chakra v3 component exist, and does the adapter
wrap one?" splits the flat "reclassify & keep" into three dispositions. **None of the ④ is a
plain primitive.**

### Group A — same-named Chakra component exists; the adapter adds real behavior on top
`Box`, `Link`, `LinkOverlay`, `Popover`. The base is a Chakra primitive/component; the
adapter's value is the behavior — polymorphism + NextLink (`Box`/`Link`/`LinkOverlay`),
device hover↔touch → `HoverCard` (`Popover`). **Custom components on a primitive — not
primitives.** Per-component decision: keep the behavior as a real `components` component, or
shed it back to the primitive where it's now native (Chakra `asChild` covers most of Box's
polymorphism; the NextLink glue is the genuinely-custom part).

### Group B1 — no Chakra component, but replaceable by native v3 style props → REMOVE
`Hide`, `Show`. Chakra v3 removed these and ships `hideFrom`/`hideBelow` as **style props**.
So they are *not* "keep" — migrate consumers to the native props (`<Hide above="md">` →
`hideFrom="md"`) and **delete** the wrappers.

### Group B2 — no Chakra underneath → collapse the redundant adapter into ONE component
`InsertionLayout`, `PopoverFilter`, `RangeFilterInputWithSlider`, `SelectMenu`. The
"adapter" isn't bridging Chakra — it's aliasing/reshaping our own code, so it's pointless
indirection.
- **InsertionLayout / PopoverFilter / RangeFilterInputWithSlider** — an underlying custom
  component already exists; fold the adapter's API into it (bake `disabled` etc. in) and
  delete the adapter. One component remains, in `components`/`features`.
- **SelectMenu — special: a redundant *dialect*, not a distinct component.** It wraps the SMG
  `Menu` (two layers off Chakra) and only renames `options→items`,
  `withIndicator→showOptionsCheckmark` (default flip), `leftIcon→icon` — no new behavior. It
  collapses **into `Menu`**: migrate consumers to `Menu`, delete `SelectMenu`. **End state:
  one `Menu` exposed, no `SelectMenu`, Chakra's raw Menu stays internal.** (Corrects its ④
  "custom, keep" call — it's really a ①-style rename over our own component.)

**Net for ④:** A = 4 keep-or-shed decisions; B1 = 2 removals (native props); B2 = 4 collapses
into single components (one of which — `SelectMenu` — disappears into `Menu`). So of the 10
④ items, only ~4 (Group A) are genuine keep-as-components; the rest either delete or collapse.
