# API conventions — the shared ruleset

> How we get consistency without pretending every component is the same. The core
> move: **unify conventions, not APIs.** Applies to design-system (②) components;
> primitives (①) are transparent Chakra, features (③) are domain-only.

---

## The principle: consistency ≠ sameness

Every component's API splits in two:

- **Essential difference** — what the component uniquely does (`Dialog` has `title`/actions,
  `Badge` has text, `Carousel` has slides). **Legitimately varies. Leave it alone.**
- **Incidental difference** — how it expresses the *shared* concepts every component has
  (disabled, loading, size, variant, icon, controlled value, events). **This is the enemy,
  and it's fully fixable.**

Goal: **no surprises in the incidental parts.** Learn the conventions once → every
component is predictable, even though its essential props differ. We don't unify APIs; we
unify the *vocabulary* and the *sourcing*, and let essential differences stand — now
legible.

---

## Rule 1 — Prop sourcing (kills leaks *and* the whitelist treadmill)

> A design-system component's public props come from **(1) the native element's
> attributes, (2) the shared SMG prop vocabulary, (3) its own semantic/domain props** —
> **never from Chakra's prop types.**

**Do:**
```tsx
type ButtonProps =
  React.ComponentPropsWithoutRef<'button'>      // onClick, disabled, type, aria-* — free
  & RecipeVariantProps<typeof buttonRecipe>     // variant, size
  & Loadable & { fullWidth?: ResponsiveValue<boolean> };  // shared vocab + semantic
```

**Don't:**
```tsx
type BadgeProps = ChakraBadgeProps & { text: string };   // ← spreads Chakra internals: LEAK
type InputProps = ChakraInputProps & { … };              // ← same leak
```

- **Leak → gone.** Native element types carry `onClick`/`disabled`/`aria-*` but **not**
  `bg`/`margin`/Chakra internals. The leak *was* the Chakra prop type.
- **Whitelist treadmill → gone.** Native attributes come free — no hand-listing
  `maxLength`/`aria-*` (the Textarea pain).
- Strip the styling escape hatches from the native set (`className`, `style`) — the escape
  hatch is "wrap in a primitive," per the category rules.

## Rule 2 — Shared prop vocabulary (one name per concept, everywhere)

Every component uses the **same** name + type for a shared concept; none reinvents it.

| Concept | Canonical prop | Ship as reusable type |
|---|---|---|
| Disabled state | `disabled` | (native) |
| Loading state | `loading`, `loadingText` | `Loadable` |
| Size | `size` | via `RecipeVariantProps` |
| Visual variant | `variant` | via `RecipeVariantProps` |
| Full-width self-sizing | `fullWidth?: ResponsiveValue<boolean>` | `FullWidth` |
| Icons | `leftIcon`, `rightIcon` | `WithIcons` |
| Controlled value (form) | `value`, `onChange` | `Controlled<T>` |
| Open state (overlays) | `open`, `onOpenChange` | `Openable` |
| Click | `onClick` | (native) |

**Banned dialects** (the adapter/v2 legacy): `isDisabled`, `isLoading`, `isTruncated`,
`noOfLines`, `spacing`, `textColor`, `align`. Enforced by lint.

Shared vocabulary ships as small **TS mixin types** (`Loadable`, `Openable`,
`Controlled<T>`, `WithIcons`, `FullWidth`) that components compose — so the vocabulary is
literally reused, not re-typed.

## Rule 3 — No Chakra style props (recap of the category rule)

Appearance → recipe/variant. External layout → wrap in a primitive. Unavoidable
self-sizing → a named semantic prop (`fullWidth`). Native HTML attributes always allowed.
Enforced by **lint**, not by removing props from types.

## Rule 4 — Declared API archetype (the "what to expect" signal)

Each component is **exactly one** archetype; declared in the manifest and visible in the
type shape:

| Archetype | Type shape | Examples |
|---|---|---|
| **Transparent** (①) | `export { X } from '@chakra-ui/react'` | Box, Flex, Stack |
| **Branded element** | native element + vocab + variants | Badge, Spinner, Button, Text, Heading |
| **Compound** | `X.Root/X.Part` namespace | Table, Card |
| **Curated** | closed semantic props, no native spread | Dialog, Drawer, Pagination, Carousel |

The archetype tells a consumer the *contract* once found; it doesn't replace category
(which governs *rules*). Normalize outliers so each component actually fits one archetype
(e.g. `Button`'s bespoke union → branded element; `Badge`'s `text` → children).

---

## Enforcement summary

| Mechanism | Enforces |
|---|---|
| Lint — `no-restricted-imports` on `@chakra-ui/react` (branded components), with a rationale message | Discovery: agents/humans reach for our component, not Chakra's |
| Lint — no Chakra prop-type extension / spread in ② components | Rule 1 (no leaks) |
| Lint — no raw style props on ②/③ | Rule 3 |
| Lint — banned prop dialects | Rule 2 |
| Shared TS mixin types (`Loadable`, `Openable`, `Controlled<T>`, …) | Rule 2 (one vocabulary) |
| Manifest: `component → category → archetype → one-line contract` | Rules 4 + discovery (human + agent index) |

## What this delivers

Components become **predictably different, not uniform.** `Dialog`, `Badge`, and `Button`
share one vocabulary for the incidental concepts and diverge only in the essential ones —
which are now visible and expected. `Button`'s "custom shape" stops hiding because its
incidental props match everything else; only its genuine behavior is bespoke.

---

## Reconciliation note

This refines the earlier rulebook (`components-rules.md`): the design-system **principle**
(no raw style props, recipe owns the look) stands, but the **enforcement mechanism** moves
from "type-level removal in a curated wrapper" to **lint + the prop-sourcing rule**, and we
add **branding via registered recipe** and the **archetype declaration**. `components-rules.md`
R2/R3 should be updated to point here.
