# RFC: Separate Chakra v3 from shared component abstractions

## Status

**Proposed.** This RFC makes the foundational product decision that precedes component-by-
component migration planning. It intentionally does not define codemods, deprecations, lint
rules, or a refactoring schedule.

## Summary

The current package is an implicit hybrid. Some exports are direct or nearly-direct Chakra
components, some are useful bespoke shared components, and some are partial wrappers that
forward Chakra props while replacing Chakra composition. The result is unpredictable for both
developers and agents: a consumer cannot know whether Chakra documentation applies, whether a
prop is available, or which layer owns a component's behaviour.

This RFC establishes an **explicit split hybrid** with two package-level contracts:

1. A **design-system package** that is the configured Chakra **v3** customization layer.
   Chakra API, composition, state model, and naming are public and authoritative.
2. A **shared-components package** that contains owned, bespoke, generic abstractions. Its
   API is semantic and closed; Chakra is an implementation detail.

This is not a fourth alternative to an explicit hybrid. It is the enforceable form of it. The
import source carries the contract rather than requiring a developer to inspect implementation
or guess from a component name.

The central rule is:

> A wrapper either preserves Chakra v3 API and composition exactly, or it owns a closed
> semantic API. It must not selectively forward Chakra props while replacing Chakra
> composition.

## Why this RFC now

### Current pain points

The package is difficult to use safely without opening implementation source. A developer—or an
agent—cannot reliably answer basic questions from an import alone:

- Is this Chakra v3, or a package-owned component with a different API?
- Do Chakra documentation and examples apply to this import?
- Which Chakra props are accepted, ignored, renamed, or deliberately withheld?
- Does a recipe brand the Chakra component automatically, or must a wrapper reproduce slots
  and apply styles itself?
- Is a surprising prop/behaviour intentional design-system policy, v2 compatibility, or an
  accidental implementation detail?

This uncertainty makes ordinary work slower and riskier. Consumers either read source, try
props until TypeScript accepts them, or add workarounds. Maintainers and agents cannot use a
simple rule to decide whether to re-export, compose, or wrap a Chakra component.

### Observable symptoms

The ambiguity is visible in the current API surface:

- **Partial Chakra prop surfaces:** `Pick`/`Omit` wrappers expose an arbitrary subset of
  Chakra, so a component looks Chakra-shaped but is not safely documented by Chakra.
- **Changed composition with Chakra props:** wrappers can accept Chakra root/slot props while
  rendering a different compound tree. Accordion is the clearest example.
- **Two styling mechanisms at once:** recipes may be correctly registered in the Chakra system
  while wrappers also manually obtain and apply those recipes.
- **V2 dialect persistence:** adapters and v2-ish names/event models make Chakra v3 adoption
  incomplete and teach consumers a second API.
- **Undiscoverable abstractions:** a small bespoke convenience can be attached to one compound
  subcomponent, while the root continues to look like a Chakra export.
- **Unclear ownership:** generic abstractions, Chakra customization, and domain/product
  composition share one public surface.

### The challenge

Classification alone cannot solve these symptoms. Each current export has consumer usage and
some have real behaviour, so forcing one uniform API immediately would cause avoidable breaking
changes. Conversely, accepting the current implementation patterns preserves the ambiguity for
every future export.

The team needs to settle the contracts first: what the design system promises, what shared
components promise, and when Chakra is intentionally visible. That decision lets later RFCs
plan migration component by component without reopening the architecture each time.

## Decision 0: Recommit to Chakra v3

Chakra v3 is the adopted UI platform for the design system and for projects that consume this
library. This RFC does not treat the package as a framework-neutral portability layer.

Replacing Chakra with another UI framework would be a full application and component rewrite,
not a dependency substitution. Projects are already heavily Chakra-aware: they use Chakra
layout/style props, responsive values, compound component composition, state/detail callback
models, and Chakra-specific types. The current package also exposes those concepts through
many leaky public interfaces.

Such a framework migration would only be materially insulated if this package already exposed
a complete, stable, bespoke abstraction API. It does not. Pretending otherwise would hide the
real cost and encourage further partial abstractions.

Therefore:

- We recommit to Chakra v3 and its documented patterns as the foundation for this work.
- We remove the remaining v2-ish compatibility dialect rather than preserve it as a parallel
  public API.
- Framework replacement, if ever desired, is a separate strategic rewrite decision with its
  own business case; it is not an outcome this RFC is intended to enable.

## Problem

A developer cannot presently infer an export's API contract:

- Chakra types are often `Pick`ed or `Omit`ted, so some Chakra props work and others do not.
- Components can expose broad Chakra root/slot props while creating a different compound tree.
- Recipes can be registered and also manually re-applied by wrappers.
- A small wrapper convenience can be hidden on one compound slot while the root looks Chakra-
  shaped.
- Chakra-v2 compatibility dialects and event bridges sit beside v3-oriented components.

This is neither a reliable Chakra customization layer nor a reliable abstraction layer. It
creates documentation mismatch, API surprises, and a steady supply of workaround props.

## Context and evidence

### Accordion: Chakra API with changed composition

Accordion exposes Chakra root and slot props such as `colorPalette`, `asChild`,
`defaultValue`, `lazyMount`, and controlled value props. But it replaces Chakra's compound
composition with `AccordionItem`, `AccordionButton`, and `AccordionPanel`; it injects an
indicator, wraps trigger contents, creates content/body slots, and clones children to propagate
the recipe variant. Its only bespoke prop is `leftIcon` on the separately exported trigger.

This is not a useful long-term abstraction. Its target belongs in the design-system package as
direct Chakra Accordion plus the registered slot recipe. The consumer migration question is
real, but deliberately out of scope here. See `accordion-migration.md`.

### Alert: a useful abstraction with a Chakra leak

Alert owns a meaningful message API: title, required description, optional icon/link, and
dismissible local state. It builds a complete message layout. That is a valid shared-component
responsibility.

It nevertheless extends an omitted Chakra Alert root type, exposing broad Chakra props and
style configuration despite owning the composition. Its target belongs in shared components as
a closed semantic Alert. See `alert-migration.md`.

### Badge: recipe branding without wrapper value

Badge only maps a `text` prop to Chakra children. The `badge` recipe is already registered and
defines its visual variants. It adds no behaviour or composition value, so it belongs in the
design-system package as direct Chakra Badge. Its `text` migration is a straightforward
codemod.

These examples establish the need for two hard public contracts, not a new case-by-case wrapper
style.

## Decision

Adopt two distinct packages with the following consumer promises:

| Package | Consumer promise | Chakra relationship |
|---|---|---|
| **Design system** | “This is Chakra v3. Chakra docs, props, callback/detail shapes, and compound composition apply.” | Chakra is the public API; this package configures and re-exports it. |
| **Shared components** | “This is ours. Use its semantic API and its documentation.” | Chakra is an internal implementation detail. |

Domain/product features are out of scope for this RFC. Their ownership and placement must not
blur either shared package contract.

## Contract 1: Design system is the Chakra v3 customization layer

The design-system package answers: “How do I use our configured Chakra v3 system?”

- Its public API is the exact Chakra **v3** API.
- Its public compound composition, state model, and callback/detail shapes are the exact Chakra
  v3 contracts.
- Exports are direct re-exports from `@chakra-ui/react`; there are no `Pick`s, `Omit`s,
  renamed props, default behaviour, wrapper logic, or prop filtering.
- Tokens, providers, recipes, and slot recipes configure the Chakra system and provide the SMG
  visual language. A recipe does not need a wrapper and must not become a reason to invent a
  parallel API.
- Chakra type generation is part of adopting the v3 customization model, so registered recipe
  variants are visible in TypeScript.
- Raw Chakra style props and responsive values remain allowed. Transparency is the contract;
  token discipline must not be implemented as an arbitrary prop whitelist.
- Chakra-v2 dialects and bridges do not belong in the long-term public API: no `isDisabled`,
  `isLoading`, `spacing`, renamed component APIs, or synthetic v2 event models merely to make
  v3 look like v2.

Examples include Box, Flex, Stack, Grid, SimpleGrid, Center, AspectRatio, and—where the team
selects the Chakra contract—Badge, Accordion, Skeleton, Table, and List.

## Contract 2: Shared components are owned abstractions

The shared-components package answers: “What reusable interaction or UI pattern do we own
beyond Chakra?”

- Public props come only from native-element attributes, shared vocabulary, owned semantic
  props, and intentional recipe variants.
- Chakra prop types are never a source of public props: no extension/intersection, `Pick`, or
  `Omit` of Chakra prop types.
- Raw Chakra style props are not exposed. No `bg`, `p`, `m`, `css`, `className`, `style`,
  `colorPalette`, or `asChild` escape hatch belongs on the public API.
- Valid native accessibility attributes, ids, names, and event handlers remain available.
- The component owns its composition, behaviour, and documentation. Consumers do not need to
  understand Chakra slots to use it.
- Visual language is owned by a recipe/slot recipe where needed; behaviour belongs in a thin
  wrapper. External layout is composed with the design-system package.
- `as` is semantic correction only, never a behavioural replacement.

Examples likely include Button, Input, Alert, Dialog, Pagination, and domain-neutral form
controls, subject to individual target decisions.

## Recipe policy

1. Recipes brand design-system exports through the configured Chakra system.
2. The registered recipe key is the normal Chakra-v3 mechanism. A manually applied recipe is
   appropriate only where a shared component genuinely owns custom composition.
3. Recipe variants are intentional visual API; they are not a route for external layout or
   context-specific positional styling.
4. A recipe belongs to a shared component only when that component owns a stable generic visual
   language.
5. A generic component name must not conceal a context-specific layout variant—for example, a
   navigation badge's positional offset belongs to its parent or to a specifically named
   navigation component.

## Public vocabulary policy

For shared components, use one canonical name per shared concept:

- `disabled`
- `loading` and `loadingText`
- `size` and `variant`
- `fullWidth`
- `leftIcon` and `rightIcon`
- controlled `value` and `onChange`
- overlay `open` and `onOpenChange`
- `onClick`

Do not introduce v2 dialects into new shared APIs: `isDisabled`, `isLoading`, `isTruncated`,
`noOfLines`, `spacing`, `textColor`, and `align` are examples of names that do not cross the
new package boundary.

For design-system exports, Chakra v3 vocabulary is authoritative. The package does not create
a second dialect.

## Goals

- Make package API expectations predictable at import time.
- Recommit the design-system layer to Chakra v3 rather than preserve a v2-ish dialect.
- Preserve Chakra wherever Chakra is the intended developer experience.
- Make shared components genuinely owned, semantic, and documentable.
- Stop partial abstractions, prop whitelists, and composition/API mismatches from becoming new
  patterns.
- Give humans and agents a reliable decision procedure for new exports.

## Non-goals

- Refactor every current export.
- Decide every individual component's target package/API.
- Specify compatibility, codemod, deprecation, or removal policy.
- Specify lint rules, CI checks, or package-migration mechanics.
- Decide feature/domain package ownership.
- Solve adapter removal, controlled state, or react-hook-form migration.

## Alternatives considered

### Pure Chakra customization package

Expose Chakra broadly and use recipes for all branding.

- **Benefit:** maximal Chakra consistency and minimal wrapper code.
- **Cost:** useful owned abstractions need another home or are discarded.

### Full abstraction layer

Expose only bespoke APIs and hide Chakra everywhere.

- **Benefit:** one public API philosophy.
- **Cost:** the team must invent, document, migrate, and maintain a parallel API for every
  Chakra interaction and compound component. This is a legitimate choice, but a large one.

### Accidental mixed wrapper model (current state)

Continue allowing partial Chakra forwarding and custom composition case by case.

- **Benefit:** local convenience.
- **Cost:** unpredictable APIs, documentation mismatch, type leakage, and continuing debt.
  Rejected.

### Explicit split hybrid model (proposed)

Use two packages with hard, visible contracts: Chakra-v3 design system and owned shared
components.

- **Benefit:** consumers choose their contract at import time; Chakra remains available where
  reasonable, and genuine abstractions remain possible.
- **Cost:** requires a package split and later migration work.

## Consequences

- A Chakra component name in the design-system package means Chakra v3 rules apply exactly.
- A shared-component name promises a closed, owned API; consumers use primitives for layout.
- Existing ambiguous exports are migration work, not precedent for future APIs.
- The inventory and migration notes are evidence and backlog input; they do not define the two
  fundamental contracts.

## Deliberately deferred decisions

1. Which existing exports move to design system versus shared components?
2. How current APIs are migrated, deprecated, codemodded, or removed.
3. How the two contracts are mechanically enforced.
4. Where features/domain components live.
5. Detailed controlled-state and form-integration conventions.

## Related artifacts

- `component-inventory.md` — current API and pattern inventory.
- `chakra-migration-triage.md` — consumer migration effort triage.
- `accordion-migration.md` — current Accordion evidence and possible migration paths.
- `alert-migration.md` — current Alert evidence and possible migration paths.
