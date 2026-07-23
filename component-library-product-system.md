# The component library as the product-facing system

## Core idea

**Chakra is the toolkit and implementation vocabulary; the component library is the product-facing system.**

The library is not a collection of unnecessary one-to-one wrappers. It is an abstraction with a deliberate public contract. Chakra supplies accessible behaviour, composition mechanisms, recipes, style props, tokens, and implementation patterns. The component library decides which of those capabilities it promises to application developers.

A component does not need to expose every underlying Chakra prop to be “true Chakra”. Selective exposure is a valid and often necessary part of the abstraction.

```text
Application code
  → component-library API: supported contract
    → Chakra: implementation capability
```

## Source of truth

For consumers of the component library, the library itself is authoritative:

- exported TypeScript types define the supported props;
- component documentation and Storybook explain intended usage;
- tests define promised behaviour.

Chakra documentation is primarily implementation material for component-library maintainers. It may explain a familiar pattern behind an exported API, but it does not expand that API.

This remains true when a library component deliberately uses Chakra naming or behaviour:

```tsx
<Button loading />
<Stack gap="md" />
<Accordion multiple />
```

The similarity is useful: it provides familiar patterns, Chakra’s behaviour, and strong types. It is still a deliberate library-level contract.

## Component categories

### Layout primitives

Examples: `Box`, `Stack`, `Grid`, `Flex`.

These are intentionally generous. Their role is composition and layout, so they may expose a broad Chakra-like layout and styling surface. Even then, the exported library API remains the contract; a close mapping to Chakra is an intentional choice, not an obligation.

### UI and behaviour primitives

Examples: `Button`, `Input`, `Accordion`, `Dialog`, `Tabs`.

These expose a curated API. They may deliberately retain selected Chakra behaviour—such as disabled state, loading state, events, accessibility, controlled values, or composition—but should not automatically expose all Chakra styling props.

Their visual identity belongs to recipes and named variants. Their supported behaviour and limited layout needs belong to their public API.

### Product components

Examples: `ArticleTeaser`, `VehicleCard`, `SearchResultItem`, `PriceSummary`.

These represent reusable product patterns. Their API should be semantic and content-driven, with styling owned by their recipe. Broad generic styling props are usually a sign that a variant, slot, or lower-level primitive should be exposed instead.

## Prop policy

A useful distinction is between shared vocabulary and component-specific meaning.

For UI and behaviour primitives, deliberately expose:

- behaviour the product intends to guarantee, such as `disabled`, `loading`, event handlers, and accessible labels;
- limited layout capability where it is meaningful, such as width on a control;
- product-specific behaviour at the shared boundary, such as safe external-link handling.

Usually keep controlled by the component:

- colours, backgrounds, borders, typography, and standard internal spacing;
- arbitrary visual overrides that bypass recipes and variants;
- Chakra props that the product does not intend to support long term.

The goal is not to create uniform APIs for unrelated components. It is to make shared concepts predictable while allowing essential differences to remain clear.

## Recipes and Chakra counterparts

A Chakra counterpart is not a category test. A component can map neatly to Chakra and still be a product component; a product primitive can use a recipe; and a shared product pattern can be built from several Chakra elements.

Recipes answer a styling question:

> How do the component and its variants remain visually consistent?

They do not decide whether the public API should be broad, curated, or semantic.

## Types

Deriving a prop from Chakra can preserve valuable type information: theme tokens, responsive syntax, event types, and accessibility-related types. It does not require exposing Chakra’s entire prop surface.

TypeScript types are a compile-time contract, not runtime validation. Chakra style props commonly accept theme tokens *and* ordinary CSS values. Where the product requires a strict set of values, define a narrower component-level type.

The public type should help consumers understand the API. If a component requires dense unions, exclusions, and polymorphic branches merely to represent several semantic components, separate public components can be clearer—for example `Button`, `ButtonLink`, and `IconButton` sharing a private implementation.

## Practical implication

Application developers should start with the component library’s docs, stories, and types. Component-library maintainers should use Chakra docs to make deliberate implementation and API decisions.

The library can lean into Chakra patterns without surrendering its own product-facing contract.
