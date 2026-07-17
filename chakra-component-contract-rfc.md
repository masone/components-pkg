# RFC: Separate Chakra v3 from shared component abstractions

## Status

**Proposed.** This RFC makes a foundational product decision. It does not yet prescribe the
migration, codemods, lint rules, or the final API of every existing component.

## In one sentence

We will have two explicit shared UI contracts: a **design-system package that exposes configured
Chakra v3**, and a **shared-components package that exposes our own semantic APIs**.

## Why we are discussing this now

Using a component should not require reading its implementation first. A developer should be
able to see an import and know what documentation applies, what props are available, and who
owns the component’s behaviour.

That is not true today. A component can look like Chakra but accept only some Chakra props. It
can expose Chakra props while rendering a different component tree. Or it can look like one of
our components but silently depend on Chakra-specific behaviour. The result is slow, uncertain
work: people read source, try props until TypeScript accepts them, or add workarounds.

This matters even more because we are making the codebase agent-friendly and want it to provide
**feedforward**. A developer or agent needs enough information to choose and use a component
correctly *before* inspecting source or receiving a failure. At present, there is no concise,
reliable way to describe many component interfaces: “it is Chakra” is often false, while “it
is our semantic component” is often equally false.

The aim of this RFC is to restore that predictability before we decide how to migrate each
individual component.

## First decision: Chakra v3 remains our UI platform

We recommit to Chakra v3 and its documented patterns.

Our projects are already heavily Chakra-aware. They use Chakra layout and style props,
responsive values, compound component composition, state/detail callback models, and
Chakra-specific types. The current package also exposes many of those concepts through its
public APIs.

Moving to another UI framework would therefore be a full application and component rewrite,
not a dependency substitution. That kind of migration could only be insulated if this package
already provided a complete, stable, framework-neutral abstraction API. It does not.

We should not pretend the current leaky API buys us framework portability. Instead, we will:

- embrace Chakra v3 where Chakra is the intended developer experience;
- remove the remaining v2-ish dialect and compatibility mindset from the long-term API;
- treat a future framework replacement, if ever desired, as a separate strategic rewrite with
  its own business case.

## Two valid ways to build a component library

There are two coherent models. Neither is inherently better; the mistake is blending them in a
single API without telling consumers which model applies.

### Model A: Chakra customization layer

The library exposes Chakra itself, configured for our brands.

- Consumers use Chakra documentation, props, compound composition, and event/detail models.
- Tokens, recipes, slot recipes, and providers create the SMG visual language.
- A recipe changes the look; it does not require a wrapper or create a second API.

This model is efficient and honest when Chakra already supplies the required behaviour.

### Model B: owned abstraction layer

The library exposes a semantic API that it owns.

- Consumers use our documentation and our semantic props.
- Chakra is an internal implementation detail.
- The library owns the composition, behaviour, accessibility decisions, and migration burden.

This model is valuable when we provide a genuinely better reusable abstraction than Chakra,
but it is expensive. We must design, document, and maintain every bespoke API we expose.

### The model we must stop using: partial abstraction

The current problem is not that both models exist. It is that they are mixed within the same
export:

> A wrapper accepts Chakra props, but changes Chakra composition or behaviour.

Consumers can then follow neither Chakra documentation nor a small owned API with confidence.
This RFC prohibits that pattern going forward.

## Decision: an explicit split hybrid

We choose both valid models, but place them in **separate packages** with clear public
contracts. This is an explicit split hybrid—not a flat package that asks consumers to infer
intent from implementation details.

| Import from… | Consumer promise |
|---|---|
| **Design system** | “This is Chakra v3, configured for us. Chakra documentation and composition apply.” |
| **Shared components** | “This is ours. Use its documented semantic API; Chakra is internal.” |

The package boundary carries the promise. A consumer chooses a contract at import time.

Domain/product features are outside this RFC. Their ownership and placement should be decided
separately, and must not blur either shared package contract.

## Contract 1: the design system exposes Chakra v3

The design-system package answers: “How do I use our configured Chakra v3 system?”

- Its public API is the exact Chakra **v3** API.
- Its public compound composition, state model, and callback/detail shapes are Chakra v3’s.
- It directly re-exports from `@chakra-ui/react`: no `Pick`, `Omit`, prop renames, default
  behaviour, wrapper logic, or prop filtering.
- Recipes and slot recipes are registered in the Chakra system and provide branding. A recipe
  does not need a wrapper and must not become a reason to invent a parallel API.
- Chakra type generation makes registered recipe variants visible in TypeScript.
- Raw Chakra style props and responsive values remain available. Transparency is the contract;
  token discipline must not be implemented as an arbitrary prop whitelist.
- V2-style names and bridges do not belong in the long-term API: no `isDisabled`, `isLoading`,
  `spacing`, renamed Chakra component APIs, or synthetic v2 event models merely to make v3 look
  like v2.

Examples include Box, Flex, Stack, Grid, SimpleGrid, Center, AspectRatio, and—where we choose
the Chakra contract—Badge, Accordion, Skeleton, Table, and List.

## Contract 2: shared components expose owned semantic APIs

The shared-components package answers: “What reusable interaction or UI pattern do we own
beyond Chakra?”

- Public props come from native-element attributes, shared vocabulary, owned semantic props,
  and intentional recipe variants.
- Chakra prop types are never a source of public props: no Chakra type extension,
  intersection, `Pick`, or `Omit`.
- Raw Chakra style props are not public API. No `bg`, `p`, `m`, `css`, `className`, `style`,
  `colorPalette`, or `asChild` escape hatch.
- Valid native accessibility attributes, ids, names, and event handlers remain available.
- The component owns its composition and behaviour. Consumers do not need to understand Chakra
  slots to use it.
- A recipe/slot recipe owns the visual language where needed; the wrapper owns only the
  component’s actual behaviour and composition. External layout uses the design system.
- `as` is semantic correction only, never a behavioural replacement.

Shared vocabulary is consistent: `disabled`, `loading`/`loadingText`, `size`, `variant`,
`fullWidth`, `leftIcon`/`rightIcon`, `value`/`onChange`, `open`/`onOpenChange`, and `onClick`.

Examples likely include Button, Input, Alert, Dialog, Pagination, and domain-neutral form
controls, subject to later per-component decisions.

## What this means for recipes

Recipes are visual configuration, not a third component contract.

- In the design system, a registered recipe brands the matching Chakra component. Consumers
  retain the Chakra API and composition.
- In shared components, a recipe supports an owned semantic API and internal composition.
- Recipe variants are intentional visual API. They are not a place for external layout or
  context-specific positioning.
- A manually applied recipe is justified only when a shared component genuinely owns custom
  composition. It is not a way to create a partial Chakra wrapper.

## The current symptoms this decision addresses

The split gives a direct answer to the problems seen in the inventory:

- **Partial prop surfaces:** a Chakra export cannot whitelist Chakra props; a shared component
  cannot source its props from Chakra.
- **Changed composition with Chakra props:** changing composition means the export belongs in
  shared components and must close its Chakra surface.
- **Two styling mechanisms:** registered recipes are the normal design-system mechanism;
  manual slot styling belongs only to genuinely owned composition.
- **V2 dialect persistence:** the design-system package is explicitly Chakra v3, not a v2
  compatibility layer.
- **Unclear ownership:** the import source says whether Chakra or the package owns the API.

## Worked examples

### Accordion: target design system

Accordion currently exposes Chakra props but replaces Chakra’s compound composition with local
wrappers. Its only bespoke convenience is `leftIcon` on a separately exported trigger. That is
not enough value to justify a parallel Accordion API.

Its target is direct Chakra Accordion plus the registered `accordion` slot recipe in the design
system. The existing wrapper may require a later migration plan, but it does not define the
future contract. See `accordion-migration.md`.

### Alert: target shared component

Alert owns title, description, optional icon/link, dismissible state, and the full message
layout. That is a meaningful abstraction. It should remain a shared component, but its current
`Omit<ChakraAlertRootProps, …>` leak must disappear so consumers see one closed semantic API.
See `alert-migration.md`.

### Badge: target design system

Badge only maps `text` to Chakra children. Its recipe already supplies branding and variants.
It should become direct Chakra Badge; `text` to children is a small later codemod.

## Scope of this RFC

### Goals

- Make package API expectations predictable at import time.
- Recommit the design-system layer to Chakra v3.
- Preserve Chakra where Chakra is the intended developer experience.
- Make shared components genuinely owned, semantic, and documentable.
- Stop new partial abstractions, prop whitelists, and composition/API mismatches.
- Give humans and agents a reliable decision procedure for new exports.

### Not decided here

- Which existing exports move to design system versus shared components.
- How current APIs are migrated, deprecated, codemodded, or removed.
- Lint rules, CI checks, and other enforcement mechanics.
- Feature/domain package ownership.
- Detailed controlled-state, adapter-removal, and react-hook-form decisions.

## Alternatives considered

### Use only Chakra customization

This is consistent and cheap, but discards or relocates useful owned abstractions such as Alert.

### Hide Chakra behind a full abstraction layer

This is also consistent, but it commits us to inventing, documenting, migrating, and
maintaining a bespoke API for every Chakra interaction and compound component. It is a valid
strategic choice, but much larger than the current package implies.

### Keep the current flat mixed package

This retains local convenience but preserves documentation mismatch, type leakage, and API
surprises. It is rejected.

### Explicit split hybrid — proposed

Two packages make the valid Chakra and owned-abstraction models visible at import time. This
preserves Chakra where it is reasonable and supports genuine shared abstractions where they add
value.

## Consequences

- A Chakra component name in the design-system package means Chakra v3 rules apply exactly.
- A shared-component name promises a closed, owned API; consumers use the design system for
  layout and Chakra-level composition.
- Existing ambiguous exports are migration work, not precedent for future APIs.
- Package splitting and individual migrations have cost, but they replace recurring uncertainty
  with a durable, understandable contract.

## Related artifacts

- `component-inventory.md` — current API and pattern inventory.
- `chakra-migration-triage.md` — consumer migration effort triage.
- `accordion-migration.md` — current Accordion evidence and possible migration paths.
- `alert-migration.md` — current Alert evidence and possible migration paths.
