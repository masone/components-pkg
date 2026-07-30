# RFC decision: Freeze interfaces

> **Status:** Proposed

## Proposal

We freeze each public component interface to the exact set of props currently
used in automotive-web. The [automotive-web inventory](./automotive-web-inventory.md)
is the source of truth: it deduplicates the public component set and aggregates
auctions-web, listings-web, and seller-web usage. For a component, the initial
allow-list is the union of its **Custom props by usage** and **Chakra-surface
props by usage** cells.

This is a compatibility boundary, not a claim that every currently used prop is
ideal. It gives us a small, explicit, stable interface to document and improve
later, rather than continuing to inherit a changing Chakra surface by default.
No Chakra prop is retained merely because the underlying implementation happens
to accept it.

## Why this matters

Today, consumers cannot tell whether a component supports a Chakra prop because
it is part of our contract or because it happens to leak through an inherited
type. That makes the package documentation incomplete and forces people to
infer capabilities from Chakra types, documentation, or implementation.

Freezing the observed surface turns that implicit behaviour into an explicit
package contract. It is the lowest-risk first move: existing automotive-web
behaviour is preserved while Storybook and exported types can become the
authoritative interface. Later decisions can improve or remove a prop
deliberately instead of inheriting more Chakra surface by default.

## Rules

- The table is the complete initial allow-list. Each explicit prop and each
  statically resolved spread key in the table is retained; no additional Chakra
  props are inferred.
- React-only attributes such as `key` remain React semantics, not named public
  component props.
- An unresolved spread is not silently expanded into the interface. We narrow
  the type, typecheck all automotive-web applications, and add a prop only when
  a valid existing call site proves it is required.
- Shorthand, longhand, and object aliases are retained for this freeze when
  they appear in the table. The [long-form style-prop decision](./rfc-decision-longform-props.md)
  defines their canonical replacement before the frozen interface becomes the
  long-term API.
- Storybook must reflect the resulting public type exactly; see the
  [Storybook public-props decision](./rfc-decision-storybook-public-props.md).
  Chakra documentation and implementation are not an additional source of
  supported props.

## Examples

The examples follow the established narrow-interface pattern: use an explicit
`Pick` for inherited Chakra props and retain locally owned props and recipe
variants separately.

### Accordion

The inventory records 30 uses and zero unresolved spreads. Its exact current
set is `variant` plus `children`, `multiple`, `collapsible`, `onValueChange`,
`borderRadius`, `defaultValue`, `value`, `background`, `boxShadow`, `bg`,
`borderColor`, `borderWidth`, and `overflow`.

```tsx
type AccordionRootUsageProps = Pick<
  ChakraAccordionRootProps,
  | 'background'
  | 'bg'
  | 'borderColor'
  | 'borderRadius'
  | 'borderWidth'
  | 'boxShadow'
  | 'collapsible'
  | 'defaultValue'
  | 'multiple'
  | 'onValueChange'
  | 'overflow'
  | 'value'
>;

export type AccordionProps = PropsWithChildren<
  AccordionRootUsageProps & RecipeVariantProps<typeof accordionRecipe>
>;
```

This replaces broad `AccordionRootProps` inheritance. `children` comes from
`PropsWithChildren`; `variant` remains public through the registered recipe.

### Badge

The inventory records 18 uses and zero unresolved spreads. Its exact current
set is the owned `text` prop plus `children`, `flexShrink`, and `mb`.

```tsx
type BadgeUsageProps = Pick<
  ChakraBadgeProps,
  'children' | 'flexShrink' | 'mb'
>;

export type BadgeProps = BadgeUsageProps & {
  text: string;
};
```

This deliberately keeps the currently used `children` surface even though the
component's `text` API is the owned content model today. Whether `text` should
be replaced by `children`, and whether the layout props remain appropriate,
are later API-design decisions—not reasons to widen this freeze.
