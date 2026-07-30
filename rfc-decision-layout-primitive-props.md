# Layout primitive Chakra props

**Status:** Proposed.

## Proposal

Which Chakra props should every Chakra-backed layout primitive expose consistently?

Expose the complete relevant Chakra surface for Chakra-backed layout primitives,
unless there is a documented reason to exclude a prop. Any allowed style prop
uses the long-form vocabulary defined in
[Use long-form style props](./rfc-decision-longform-props.md).

Components that already expose their complete relevant Chakra surface retain
that surface. The long-form rule standardises the supported style-prop
vocabulary; it is not a component-by-component narrowing exercise.

Opinionated layout utilities are out of scope for that rule. Their small
bespoke interfaces are intentional and should be assessed on their own
semantics.

## Current state

| Component | Kind | Current public contract | Chakra comparison | Proposed future contract |
| --- | --- | --- | --- | --- |
| `AspectRatio` | Chakra primitive | All `AspectRatioProps` | Complete Chakra surface | Keep the complete Chakra surface; apply the shared long-form style-prop vocabulary |
| `Box` | Chakra primitive + compatibility adapter | Chakra `BoxProps` except intended type-level `color`, plus legacy `textColor`, `spacing`, and polymorphic `as` handling | Nearly complete Chakra surface; vocabulary altered by adapter | Keep its complete Chakra surface. The naming adapter and its legacy vocabulary are decided separately in the adapter decision |
| `Center` | Opinionated layout utility | `children`, `padding` | No Chakra surface applies; deliberately selective | Keep the same selective contract, expressed as an explicit bespoke type rather than a Chakra `Pick` |
| `Flex` | Chakra primitive | `ChakraFlexProps` except `gap`, `columnGap`, `rowGap` | Complete except the three gap props | Expose the complete Chakra surface, including the three gap props; apply the shared long-form style-prop vocabulary |
| `Grid` | Chakra primitive | All `GridProps` | Complete Chakra surface | Keep the complete Chakra surface; apply the shared long-form style-prop vocabulary |
| `GridItem` | Chakra primitive | All `GridItemProps` | Complete Chakra surface | Keep the complete Chakra surface; apply the shared long-form style-prop vocabulary |
| `Stack` | Chakra primitive + compatibility adapter | All `ChakraStackProps`, plus legacy `spacing` | Complete Chakra surface, plus a legacy alias | Keep the complete Chakra surface. The compatibility adapter and its legacy vocabulary are decided separately in the adapter decision |
| `SimpleGrid` | Chakra primitive + compatibility adapter | All `ChakraSimpleGridProps`, plus `spacing`, `spacingX`, `spacingY` | Complete Chakra surface, plus legacy aliases | Keep the complete public Chakra surface; remove legacy spacing aliases and apply the shared long-form style-prop vocabulary |
| `FullHeight` | Opinionated layout utility | `children` only | No Chakra surface applies | Stay as-is |
| `Hide` | Opinionated layout utility | `children`, `above`, `below` | No Chakra surface applies | Stay as-is |
| `Show` | Opinionated layout utility | `children`, `above`, `below`, `showDisplay` | No Chakra surface applies | Stay as-is |

## Scope of the proposal

The public-interface work separates into three small groups:

1. **Already broad:** `AspectRatio`, `Grid`, `GridItem`, `Stack`, and
   `SimpleGrid` already expose the complete relevant Chakra surface. Keep that
   surface.
2. **Expand deliberately:** `Flex` gains its three currently excluded gap
   props, producing the complete Chakra surface. This is an additive contract
   change.
3. **Opinionated layout utilities:** `Center`, `FullHeight`, `Hide`, and `Show`
   keep their small bespoke visual-layout contracts. They do not need a Chakra
   prop surface.

`Box`, `Stack`, and `SimpleGrid` have compatibility/naming adapters. Their
underlying Chakra surfaces are already broad; decisions about their legacy
aliases and adapter behaviour belong to the separate adapter decision.

This makes the layout-primitive work relatively contained. The only public
surface expansion identified here is Flex gap support. Potential breaking
changes are limited to any separately agreed removal of legacy adapter aliases
or enforcement of the long-form style-prop vocabulary.

The resulting public interfaces contain no Chakra `Pick` types: broad Chakra
primitives use their complete Chakra prop types, while Center expresses its
unchanged two-prop contract explicitly. The existing internal `Pick` types in
the unexported Stack and inner SimpleGrid implementations are implementation
cleanup, not part of this public-interface proposal.

## Separate follow-up: internal and public API divergence

The public API is the contract decided in this proposal. A separate
implementation investigation is required because some internal imports do not
use that contract:

- The package-root `Stack` exposes the complete Chakra Stack surface (plus its
  current adapter alias), while internal imports of `@/src/components/stack`
  receive a narrower `Pick` type.
- The package-root `SimpleGrid` exposes the complete Chakra SimpleGrid surface,
  while its inner component is declared with a narrower `Pick` type.

This is an internal/external API conflict. It does not change the proposed
public interfaces above, and will be tackled separately: identify every direct
internal import, assess any workarounds or type differences, and converge on a
single canonical implementation and contract.

## Evidence to consider

- `Center` uses a component-local `Pick`.
- `Flex` explicitly omits `gap`, `columnGap`, and `rowGap`.
- The root package currently exports broad compatibility adapters for `Stack`
  and `SimpleGrid`, while their base implementations are narrower. This is an
  implementation inconsistency rather than a coherent public-interface policy.
- No browser-support policy or recorded rationale was found in this repository
  for the `Flex` gap exclusion. Browser compatibility is a plausible historical
  explanation: flexbox `gap` arrived in Safari 14.1 / iOS 14.5 and is not
  supported by Internet Explorer. It is now broadly supported by modern
  browsers. Whether this remains an exception depends on the product support
  matrix, which is not documented here.

## Significant change: Flex gap props

Expose all `ChakraFlexProps`, including `gap`, `columnGap`, and `rowGap`.

Those three props are the only explicit Chakra-prop blacklist on a standard
layout primitive. Their exclusion is plausibly historical browser-compatibility
workaround: flexbox gap was added in Safari 14.1 / iOS 14.5 and is not supported
by Internet Explorer. The repository contains no support policy that requires
those older browsers, and the package already uses gap-based layout elsewhere.

This is a significant, explicit expansion of the Flex contract, rather than an
incidental consequence of removing a `Pick` or adapter.

## Source locations

- `src/components/center/index.tsx`
- `src/components/flex/index.tsx`
- `src/components/stack/index.tsx`
- `src/components/simpleGrid/index.tsx`
- `src/components/adapter/Box/index.tsx`
- `src/components/adapter/Stack/index.tsx`
- `src/components/adapter/SimpleGrid/index.tsx`
- `src/components/index.ts`
