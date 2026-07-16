# Components classification (first pass)

> A working map of every export into the three-category model, plus the open
> questions it surfaces. Name-based + spot-checked; items marked ⚠️ need a quick
> verify in the detailed pass. Altitude is deliberately high-level — this is the
> backlog map, not a migration order.

## How to read this

- **Category** = which contract governs the component (see the structure RFC).
- **Future work** = the *kind* of cleanup the bucket implies, and its **breakage risk**.
- The point: **breaking work is concentrated in buckets ② + ④.** ① and ③ are low-risk.

| Category | Contract | Future work | Breaking? |
|---|---|---|---|
| ① Primitives | Transparent Chakra re-export | Remove `Pick`/`Omit`/adapters | **No — additive** |
| ② Design-system | Curated API, recipe owns look, no style props | Curate API, recipe-ize, strip style props | **Yes → major** |
| ③ Features | Domain props only, Chakra internal | Ensure no Chakra leakage | Mostly internal |
| ④ Adapters | (legacy v2 compat shim) | Codemod prop renames → delete | **Yes → major** |
| ⑤ Infra | not components | n/a | n/a |

---

## ① Primitives (~8) — non-breaking cleanup, safe to do early

`box, flex, stack, grid, simpleGrid, center, aspectRatio, text`

- **Inconsistency:** `Flex` uses `Omit`, `Stack` uses `Pick`, several have duplicate
  `adapter/*` versions that rename props.
- **Future work:** collapse to transparent re-exports. Removing a whitelist only *adds*
  allowed props → **additive, no coordinated major needed.**
- ⚠️ `heading`, `text` carry `textStyle` typography identity. Still primitive
  re-exports, but the design-system typography lives in `textStyles` — don't split
  hairs now, just flag.

## ② Design-system (~40) — breaking cleanup, batch into one major

- **Form:** `button, input, textarea, checkbox, checkboxGroup, radio, select, switch,
  field, colorPicker, datePicker, timePicker, discreteSlider, rangeSlider`
- **Feedback:** `alert, badge, chip, count, progress, skeleton, spinner, rating, toast,
  closeButton`
- **Overlays:** `dialog, drawer, popover, hoverCard, tooltip, menu`
- **Structure / nav:** `accordion, mobileOnlyAccordion, breadcrumbs, pagination, tab,
  table, list, link, linkOverlay, card, avatar, collapse, separator`

- **Inconsistency:** leaked raw style props (blocklist-style `Omit`), v2 prop dialects
  (via adapters), whitelists (`textarea`).
- **Future work:** curate the public API, move the look into a recipe, strip raw style
  props. Per-component, batched into the coordinated major.

## ③ Features (~18) — mostly internal, low consumer impact

`articleTeaser, carousel, energyLabel, errorPage, galleryHeader, focusedHeader,
simpleHeader, navigation, tenantSelection, vehicleReference, topListingBadge,
topVehicleSharedBadge, missingImage, devOverlay, filterPatterns, checkboxFilter,
rangeFilterInput, rangeFilterInputWithSlider`

- **Inconsistency:** possible Chakra-prop leakage, `as` workarounds.
- **Future work:** ensure a domain-only API with Chakra kept internal.

## ④ Adapters (28) — breaking, same major as ②

`adapter/*` — the Chakra v2 compat dialect (`isDisabled`, `isLoading`, `isTruncated`,
`noOfLines`, `spacing`, `textColor`, `align`, `sx`).

- **Future work:** ship codemods for the prop renames, repoint the public export to the
  clean component, delete the adapter. **Freeze new adapters now** (lint); removal is a
  later epic.

## ⑤ Infra (not components)

`themeProvider, translationProvider, icons` — providers/assets, out of the category
model.

---

## Misfits & borderline calls (open questions)

These are the interesting classification tensions. They need a decision, but not now —
the RFC carries them as explicit "to confirm" items.

1. **`carousel`** — generic and semantic-only, but owns no brand visual identity →
   classified **feature**, matching the source notes even though it isn't data-bound.
   **Precedent this sets:** *semantic-only API + composition → feature, even without a
   data object.* Confirm we're happy with that rule.

2. **`checkboxFilter` / `rangeFilterInput` / `rangeFilterInputWithSlider` /
   `filterPatterns`** — domain filter patterns → **feature**, while their generic
   siblings (`checkbox`, `rangeSlider`) are **design-system**. Same concept, two
   categories — the clean illustration of the tie-break. Confirm the split.

3. **`layout` / `section` / `fullHeight`** — SMG-custom layout helpers (not Chakra
   primitives) → not ① despite feeling like it. Design-system-layout or feature?
   **Genuinely ambiguous → holding-bucket candidate.**

4. **`link` / `linkOverlay`** — design-system, but the Next.js routing integration
   (`adapter/Link` + the `as`/`asChild` dance) is a cross-cutting concern to schedule
   separately.

5. **`heading` / `text`** (⚠️ from ①) — primitive re-export vs. design-system typography
   (`textStyle`). Decide whether typography identity makes them ②.

6. **`field` / `formControlSection`** — is `field` generic enough for ② and
   `formControlSection` a ③ composition? Verify.

## Cross-cutting open questions (from earlier)

- **IconButton vs Button** — currently keep unified.
- **react-hook-form + Chakra synthetic events** — v3 moves away; RHF still expects them.
- **Recipe co-location vs centralized** — currently centralized (`themes/shared/recipes/`).
- **Monorepo split timing** — the eventual `primitives`/`features` package split *will*
  be a breaking path change; this RFC only sequences toward it.

---

## Method note

Classification is name-based + spot-checked against read files (`box`, `flex`, `stack`,
`textarea`, `carousel`, `breadcrumbs`, `button`, `adapter/*`). The detailed pass should
open each component's `index.tsx` to confirm the ⚠️ and misfit calls before the taxonomy
is treated as final. Counts (~8 / ~40 / ~18 / 28) are approximate.
