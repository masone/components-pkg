# Agent prompt — Component inventory & conformance analysis

> Hand this to a fresh agent working in the `@smg-automotive/components` repo. It is
> self-contained. The agent produces `component-inventory.md`; it changes no source.

---

## Your task

You are analyzing a React component library (`@smg-automotive/components`, built on
**Chakra UI v3**) that is being restructured for API consistency. Produce a **detailed,
accurate inventory** of every component covering four layers:

1. **Classification** — UI primitive / component / feature.
2. **Today's props surface** — the actual public API.
3. **Delta** — how today's surface differs from the *intended* props interface.
4. **Pattern-correctness** — whether the implementation uses the right patterns for its category.

**Analysis only — do not modify any source.** Output a single markdown file:
`component-inventory.md`.

## Step 0 — Read the design docs first (source of truth)

The repo root contains the target-state design docs. **Read these before starting**; this
prompt summarizes them but they are authoritative:

- `components-rules.md` — category rules of engagement
- `api-conventions.md` — prop conventions (sourcing rule, shared vocabulary, archetypes)
- `package-structure.md` — the three-package target model
- `components-classification.md` — a first-pass classification to validate/refine
- `adapter-removal.md` — the four kinds of adapter (①–④) and the migrate-or-reclassify rule
- `idiomatic-adoption-assessment.md`, `chakra-idiomatic-vs-wrapper.md`,
  `design-tensions.md`, `components-structure-research.md` — deeper background

(Skip this prompt file itself when reading context.)

## Step 1 — Classify each component into exactly one bucket

- **UI primitive** — a Chakra component exposed ~thinly; Chakra's props/API essentially
  apply; branded via a **registered recipe**, not by wrapping logic. "It's Chakra's X,
  maybe branded." No bespoke semantic API, no domain data.
  *e.g. Box, Flex, Stack, Text, Badge, Table, Skeleton.*
- **Component (generic / shared design-system)** — a **custom, generic** UI component with
  a bespoke semantic API and **no Chakra style props exposed**; may carry
  behavior/composition; not tied to any domain data.
  *e.g. Button, Input, Carousel, Dialog, Pagination.*
- **Feature** — **domain-bound**: takes a domain data object, or renders the fields of a
  business entity; no Chakra props; semantic/domain API.
  *e.g. VehicleReference, tenantSelection, topVehicleSharedBadge.*

Disambiguation tests:
- **Chakra-shaped vs custom** — does the public API essentially pass through Chakra's props
  (primitive) or present a bespoke semantic API (component/feature)?
- **Generic vs domain** — works for any content/project (component) vs encodes a specific
  business entity/data (feature)?
- **Scalar vs entity** — a scalar/enum parameterizing a self-contained branded visual →
  primitive/component; an object of domain fields + opinionated composition → feature.
  (EnergyLabel `efficiency` → not a feature; VehicleReference `vehicleTitle/price/sellerName`
  → feature.)
- **Mechanism vs view** — takes config + injected `children` (Carousel) → component; renders
  a specific entity's fields (VehicleReference) → feature.

Special handling:
- `src/components/adapter/*` — **legacy Chakra-v2→v3 compatibility shims**, slated for
  removal (this removal is a precondition for the wider cleanup). **Catalogue each adapter
  individually** — see the dedicated adapter task in Step 4.D. Do not fold them into the
  main per-component inventory.
- Infra (`themeProvider`, `translationProvider`, `icons`) — mark "out of scope / infra."

## Step 2 — Intended prop conventions (measure the delta against these)

- **Prop sourcing rule** — a component's public props come from (1) the **native element's
  attributes**, (2) a **shared vocabulary**, (3) its own **semantic/domain props** —
  **never** from Chakra's prop types. Sourcing from `ChakraXProps` (extends, `&`-spread, or
  `Omit<ChakraXProps, …>` blocklists) is a **leak**.
- **Shared vocabulary** (one canonical name per concept): `disabled`,
  `loading`/`loadingText`, `size`, `variant`, `fullWidth`, `leftIcon`/`rightIcon`,
  controlled `value`/`onChange`, overlay `open`/`onOpenChange`, `onClick`.
- **Banned v2 dialects**: `isDisabled`, `isLoading`, `isTruncated`, `noOfLines`, `spacing`,
  `textColor`, `align`.
- **No raw Chakra style props** on components/features (`bg`, `color`, `p`, `m`, `w`,
  `display`, …). Appearance → recipe; external layout → wrap in a primitive; unavoidable
  self-sizing → a semantic prop like `fullWidth`. Native HTML attributes
  (`aria-*`/`data-*`/`id`/`name`/`type`/handlers) are allowed.
- **Primitives are the exception** — they stay transparent (full Chakra API, props apply).

## Step 3 — Pattern-correctness layer

Assess whether the implementation uses the right patterns for its category:

- **Primitive** → transparent re-export, or a thin wrapper branded by a **registered
  recipe** (registered in `src/themes/shared/recipes/index.ts` or `slotRecipes/index.ts`).
  Should not hand-apply recipes unnecessarily or invent custom props. Check: recipe
  registered? wrapper thin?
- **Component** → look owned by a **recipe / slot recipe**; behavior in a thin wrapper;
  props per Step 2. Check: recipe present & registered? style-prop leaks? v2 dialects?
  `Omit`/`Pick` blocklists? bespoke unions? `as`-as-workaround? forwards refs?
- **Feature** → composes primitives/components; **probably should NOT own a recipe** (it
  renders domain content, not visual language). **Flag any feature that owns a
  recipe/slot recipe** as a possible pattern smell. Check: leaks Chakra props? reaches into
  a data object?

## Step 4 — Output (`component-inventory.md`)

### A. Summary table (every component)

| Component | Category | Pattern-correct | API conformance | Verdict |
|---|---|---|---|---|

Use ✅ fine / ⚠️ minor / ❌ needs work for the middle columns; one-line verdict.

### B. Detailed entry per component

For each folder under `src/components/*` (excluding adapters/infra as noted):

- **Category** — primitive / component / feature; confidence; which test drove it.
- **Today's props surface** — the actual exported props type, condensed (list props + how
  each is typed). Read `index.tsx` and any sub-files (compound components).
- **Nature of today's API** — bespoke / Chakra-derived / **mixed**, with specifics.
  *Worked example (Button):* "Mixed — recipe `variant`/`size` (idiomatic) + a bespoke
  tri-modal union (button / icon / link) + `Omit<ChakraButtonProps, style-blocklist>` that
  still leaks `p`/`m`/`w`; `loading` untyped."
- **Intended props interface** — what it should be under Step 2.
- **Delta** — concretely what differs and what would change.
- **Pattern-correctness** — Step 3 findings (recipe usage, registration, leaks, dialects,
  `as`, refs).
- **Verdict** — fine / needs work, and the work required.

### C. Cross-cutting summary

- Counts per category; how many ✅ / ⚠️ / ❌.
- Common issues (Chakra prop-type spreads, style-prop leaks, v2 dialects, unregistered or
  manually-applied recipes, features owning recipes).
- Notable outliers (e.g. Button, Input) and the blurry classification calls, with your call
  + reasoning.

### D. Adapter catalogue (`src/components/adapter/*`)

Removing the adapters is a **precondition** for the wider cleanup, so this catalogue is the
removal plan. Read `adapter-removal.md` first for the four-kind framework, then produce a
**row per adapter**:

| Adapter | Kind | Prop / API map (legacy → v3) | Reasonably migratable to v3? | Action | Effort |
|---|---|---|---|---|---|

- **Kind** — classify each into one of:
  - **① pure v2→v3 rename** (e.g. `Divider`→`Separator`; `Text`: `isTruncated→truncate`)
  - **② event-model bridge** — rebuilds a synthetic `ChangeEvent` from v3 detail callbacks
    (e.g. `Switch`/`Checkbox`/`Input`); collides with react-hook-form
  - **③ component rename / API reshape** (e.g. `Modal`→`Dialog`, `Table` flat→compound)
  - **④ genuine behavior mislabeled as an adapter** (e.g. `Popover`'s hover/touch fallback,
    `Hide`/`Show`, `Box` polymorphism, `Link`, the filter components)
- **Prop / API map** — the exact legacy→v3 mapping (this is the codemod spec). Quote real props.
- **Reasonably migratable to v3?** — yes / yes-with-a-decision / no.
- **Action** — `remove → v3-native` (①③, and ② once the RHF decision is made) **or**
  `reclassify → shared component (no Chakra API expected)` (④, and any ② where v3 migration
  isn't reasonable). Per the rule: if it can't reasonably migrate to a v3 interface, treat it
  as a shared component, **not** a Chakra primitive.
- **Effort** — low / medium / high, and call out the **react-hook-form / synthetic-event**
  decision explicitly wherever ② applies.

Close with a short summary: counts per kind, which adapters are trivial codemods vs which
need decisions (RHF), and which are really components to reclassify.

## Step 5 — How to work

- Components: `src/components/<name>/index.tsx` (compound ones have sub-files — read them).
- Recipes: `src/themes/shared/recipes/<name>.ts` and `slotRecipes/<name>.ts`; registration
  maps: `src/themes/shared/recipes/index.ts` and `slotRecipes/index.ts`.
- Enumerate the full set with `ls src/components` first (~80 folders).
- **Read the actual code — do not guess.** Quote the real props type. If a classification is
  uncertain, say so and give your best call with reasoning.
- Be faithful and specific; this report drives real refactoring decisions.
- **Do not modify any source.** Output only `component-inventory.md`.
