# RFC: Re-establish the Chakra and component-library contract

## Status

**Proposed.** This is a foundational design decision. It intentionally precedes and constrains
the per-component cleanup work in `component-inventory.md` and
`chakra-migration-triage.md`.

## Summary

The package currently has no predictable public-component contract. Some exports are Chakra
aliases, some are recipe-branded wrappers, some are semantic components, and some are legacy
compatibility shims. The difficult cases mix those roles: they expose broad Chakra prop types
but replace Chakra's documented compound composition.

This RFC proposes an explicit hybrid library with hard boundaries:

1. **Chakra primitives** preserve Chakra API and composition exactly; recipes provide
   branding.
2. **Owned generic components** expose closed semantic APIs; Chakra is internal.
3. **Domain features** expose domain/data APIs; Chakra is internal.
4. **Compatibility facades** are temporary migration state, with a declared target and no
   permission to grow.

The central rule is:

> A wrapper either preserves Chakra’s API and composition exactly, or it owns a closed
> semantic API. It must not selectively forward Chakra props while replacing Chakra
> composition.

This is not a proposal to refactor every component now. It is a proposal to re-establish the
rules that make incremental migration safe and make new code predictable.

## Problem

A developer cannot currently infer an export's contract from its name, package, or TypeScript
surface:

- A component can accept most Chakra props but not all of them because of `Pick` or `Omit`.
- A component can expose Chakra root/slot props while constructing a different compound tree.
- A recipe can be registered but also manually applied by a wrapper.
- A custom prop can be hidden on one separately exported compound part rather than visible on
  the root API.
- Legacy Chakra-v2 compatibility adapters share the public barrel with v3-oriented exports.

This makes both human and agent work unreliable. Chakra documentation is sometimes relevant,
but consumers cannot know whether it describes the import they are using. Package-owned
documentation is also insufficient because many components retain an uncontrolled Chakra
surface.

The result is neither a consistent Chakra customization layer nor a consistent abstraction
layer.

## Context and evidence

The current investigation produced three representative examples.

### Accordion — almost no abstraction value

Accordion exposes Chakra root and slot prop types, including root configuration such as
`colorPalette`, `asChild`, `defaultValue`, `lazyMount`, and controlled value props. However,
it replaces Chakra's compound JSX composition with `AccordionItem`, `AccordionButton`, and
`AccordionPanel`; it injects an indicator, wraps trigger contents, creates content/body slots,
and manually clones children to propagate recipe `variant`.

The only bespoke prop is `leftIcon` on the separately exported `AccordionButton`, not the root.
This does not justify a parallel Accordion API. Its desired target is a Chakra primitive with a
registered slot recipe, but immediate conversion would be a consumer JSX migration. The
current implementation should therefore be treated as a temporary compatibility facade, not a
valid long-term category. See `accordion-migration.md`.

### Alert — real semantic abstraction, leaky boundary

Alert owns a meaningful message API: `title`, required `description`, optional icon/link,
and dismissible local state. It constructs the full root/indicator/content/title/description
tree and close action. That is a plausible owned generic component.

It also intersects its API with an `Omit<ChakraAlertRootProps, ...>`, thereby exposing broad
Chakra configuration and style props despite owning the composition. Its desired target is a
closed generic component, not necessarily a primitive. See `alert-migration.md`.

### Badge — recipe branding without wrapper value

Badge is a Chakra wrapper that only changes `children` into `text`. Its registered recipe
already defines the visual variants. The wrapper adds no behaviour or composition value and
should become a direct Chakra primitive; `text` to children is a simple codemod.

These cases demonstrate why a single category based only on current implementation is not
enough. The RFC needs a target contract and an explicit temporary state for migration.

## Decision

Adopt an explicit hybrid component-library model:

| Contract | Consumer expectation | Chakra relationship |
|---|---|---|
| Primitive | “This is Chakra. Chakra docs, props, and compound composition apply.” | Direct transparent export; recipe brands it. |
| Generic component | “This is ours. Use its semantic API and our docs.” | Internal implementation detail. |
| Feature | “This solves a domain/product problem.” | Internal implementation detail. |
| Compatibility facade | “This is legacy. It has a declared migration target.” | Temporary bridge only. |

The long-term package structure may reflect these contracts through separate packages or
subpaths. That packaging decision is not required to adopt the rules now, but the public API
must eventually make the distinction discoverable.

## Contracts

### 1. Chakra primitive / customization layer

Primitives answer: “How do I compose layout or use a Chakra building block?”

- Public API is the exact Chakra API.
- Public compound composition is the exact Chakra composition.
- The implementation is a direct re-export from `@chakra-ui/react`; no `Pick`, `Omit`, prop
  rename, default behaviour, wrapper logic, or style filtering.
- Recipes and slot recipes are registered in the configured Chakra system and provide
  branding. A recipe does not need a wrapper.
- Chakra type generation is run so registered recipe variants are type-safe.
- Raw Chakra style props and responsive values are allowed because transparency is the
  contract. Token discipline is handled by convention/lint, not a partial type whitelist.

Examples: Box, Flex, Stack, Grid, SimpleGrid, Center, AspectRatio, and—where the team elects
the Chakra contract—Badge, Accordion, Skeleton, Table, and List.

### 2. Owned generic component / abstraction layer

Generic components answer: “What reusable interaction or UI pattern does the package own?”

- Public props come only from native-element attributes, shared vocabulary, semantic props,
  and intentional recipe variants.
- No Chakra prop types or Chakra style props are exposed: no extension/intersection with a
  Chakra props type, no `Pick`/`Omit` workaround, and no raw `bg`, `p`, `css`, `className`, or
  `style` escape hatch.
- Native accessibility attributes, ids, names, valid event handlers, and refs remain
  available.
- Look is owned by a registered recipe or slot recipe; behaviour/composition is owned by a
  thin wrapper.
- `as` is only semantic correction, never a behavioural replacement.
- External layout is composed with a primitive; unavoidable self-sizing gets an explicit
  semantic prop such as `fullWidth` only when justified.

Examples likely include Button, Input, Alert, Dialog, Pagination, and domain-neutral form
controls—subject to individual target decisions.

### 3. Feature

Features answer: “What product/domain problem does this solve?”

- Public API contains domain data and domain semantics only.
- Chakra and generic components are implementation details.
- Features normally compose existing visual language rather than owning a new recipe.
- App-specific features are candidates to move closer to their owning application/domain.

Examples: VehicleReference, tenantSelection, filter patterns, navigation, and error-page
composition.

### 4. Compatibility facade

Compatibility facades are not a permanent library category. They are an explicit temporary
state for an export whose present API cannot be converted without consumer migration.

Every facade must declare:

- its target contract (primitive, generic component, or feature);
- its migration path and whether a codemod is possible;
- the breaking-change boundary for removal;
- a freeze: no new bespoke props and no new consumers where lint/documentation can prevent it.

Accordion is the canonical example: a legacy facade targeting a primitive. It may remain
temporarily, but it must not be mistaken for the desired primitive API.

## Recipe policy

1. Recipes brand Chakra primitives through the configured Chakra system.
2. Registered recipe keys are the normal mechanism. Manually applying a recipe inside a wrapper
   is appropriate only when the wrapper genuinely owns custom composition; it must not be used
   to turn a Chakra primitive into a partial abstraction.
3. Recipe variants are the only intentional visual API. They are not a route for external
   layout or context-specific positional styling.
4. A recipe belongs to a generic component only when that component owns a stable generic
   visual language. Features normally reuse component/primitive recipes.
5. A recipe variant such as a navigation badge's positional offset should be challenged: external
   placement belongs to the parent or a specifically named component, not a generic primitive.

## Public API and vocabulary policy

For owned generic components and features:

- No Chakra prop type can be extended, intersected, picked, or omitted as a source of public
  props.
- Native element attributes are allowed, excluding styling escape hatches where the component
  rules prohibit them.
- Shared names are used consistently: `disabled`, `loading`/`loadingText`, `size`, `variant`,
  `fullWidth`, `leftIcon`/`rightIcon`, `value`/`onChange`, `open`/`onOpenChange`, and `onClick`.
- Legacy/v2 names such as `isDisabled`, `isLoading`, `isTruncated`, `noOfLines`, `spacing`,
  `textColor`, and `align` are not introduced into new owned APIs.

For primitives, Chakra's vocabulary remains authoritative; the package does not create a
second dialect.

## Enforcement direction

The RFC establishes the policy; concrete tooling should follow in a later implementation plan.

- Per-folder/package lint rules enforce the intended contract.
- Primitive barrels may only re-export from `@chakra-ui/react`.
- Owned components/features reject Chakra prop-type imports and raw style prop exposure.
- Direct imports of branded Chakra components are governed by the package boundary/lint so
  recipe configuration is not accidentally bypassed.
- Recipe registration and type generation are checked in CI.
- Compatibility facades carry deprecation metadata and are excluded from new use.
- A manifest maps export → target contract → current migration state → one-line contract.

## Migration strategy

This RFC does **not** require an 80-component rewrite.

1. Separate **current implementation state** from **target contract** in the inventory.
2. Ship direct-re-export source cleanups first. They are generally additive: Flex, Stack,
   SimpleGrid, Center, Grid aliases, Skeleton, Separator, and similar thin layers.
3. For easy custom-prop migrations, provide deprecation guidance and codemods. Badge `text` to
   children is the model.
4. For compatibility facades, add a correct primitive/generic API alongside the legacy export
   when necessary; migrate consumers gradually and remove in a coordinated major.
5. For real generic components, preserve valuable behaviour while closing the Chakra prop
   boundary. Do not convert them to raw Chakra merely because they import Chakra internally.
6. Freeze new partial abstractions immediately. Every new wrapper must select one contract.

`chakra-migration-triage.md` records the current consumer-migration effort assessment; it is a
backlog input, not the decision itself.

## Goals

- Make a component's expected API predictable before source inspection.
- Preserve Chakra where Chakra is the intended contract.
- Make package-owned components genuinely semantic and documented.
- Stop API drift caused by partial wrappers and prop whitelists.
- Enable incremental migration with known semver/codemod costs.
- Give humans and agents a reliable decision procedure for new components.

## Non-goals

- Refactor every current export in this RFC.
- Decide every individual component's final API in this RFC.
- Complete the package/subpath split immediately.
- Enforce token-only/raw-value restrictions for primitives.
- Delete adapters or compatibility facades before their migration plans are ready.
- Solve all controlled-state or react-hook-form decisions here.

## Alternatives considered

### A. Pure Chakra customization package

Expose Chakra broadly and use recipes for all branding.

- **Benefit:** maximum Chakra consistency and minimal wrapper code.
- **Cost:** existing semantic components/features need a different home or must be rewritten;
  this throws away useful owned behaviours such as Alert's message API.

### B. Full abstraction layer

Expose only owned, closed APIs; Chakra is always hidden.

- **Benefit:** one public API philosophy.
- **Cost:** major rewrite and continued maintenance of a parallel design-system API;
  reimplementing Chakra composition has high cost and obscures Chakra's existing capability.

### C. Accidental mixed wrapper model (current state)

Continue allowing partial Chakra forwarding and custom composition case by case.

- **Benefit:** short-term local convenience.
- **Cost:** unpredictable APIs, documentation mismatch, type leakage, and continued migration
  debt. Rejected.

### D. Explicit hybrid model (proposed)

Keep both primitives and owned components, but make their contracts hard and visible.

- **Benefit:** preserves Chakra strengths and useful package abstractions without ambiguity.
- **Cost:** requires discipline, enforcement, and migration planning.

## Consequences and trade-offs

- More explicit layout composition is expected around owned components; use primitives for
  margins, positioning, and responsive layout.
- Some existing exports will be marked legacy rather than corrected immediately. This is an
  honest transitional cost, not a failure to decide.
- New primitive APIs may initially need additive names/subpaths while legacy names remain,
  until a major version can make the clean name canonical.
- The inventory becomes a migration backlog, not an architecture specification by itself.
- The package boundary/package split remains an open delivery decision, but the type and
  composition contracts can be enforced immediately.

## Open decisions to resolve after accepting this RFC

1. Does the target structure use three packages (`primitives`, `components`, `features`) or a
   two-package boundary with internal feature ownership rules?
2. Which exports become direct Chakra primitives versus retained generic components?
3. What is the standard compatibility-facade lifecycle, deprecation annotation, and removal
   threshold?
4. Which generic components require controlled state conventions beyond the shared vocabulary?
5. How are recipe type generation and recipe registration verified in CI?
6. Which style/token restrictions are linted for primitives versus owned components?
7. What are the sequencing and react-hook-form decisions for the adapter-removal program?

## Follow-up artifacts

- `component-inventory.md` — current API and pattern inventory.
- `chakra-migration-triage.md` — consumer migration effort triage.
- `accordion-migration.md` — compatibility-facade-to-primitive example.
- `alert-migration.md` — partial-abstraction-to-generic-component example.
