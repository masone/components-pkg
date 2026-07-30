# Retire polymorphic layout workarounds

**Status:** Proposed.

## Proposal

After the adapter-alias decision has been implemented and layout primitives
expose their chosen Chakra prop surfaces consistently, remove existing uses of
`Box as={LayoutPrimitive}` that exist only to obtain props unavailable on the
target primitive.

Use the layout primitive directly whenever it is a drop-in replacement. Keep
polymorphic composition only where one DOM node genuinely needs both roles, and
document those cases as explicit exceptions. Once the migration is complete,
add a targeted validation rule that rejects new Box-to-layout-primitive
substitutions unless they are an approved composition exception.

This proposal does not ban `as` in general. Rendering as an intrinsic element,
a framework link, or another deliberately composed component remains a valid
composition mechanism.

## Problem

`Box` has a broad polymorphic prop type. When it is rendered as a layout
primitive, overlapping props are checked as Box props rather than the target
primitive's props. This lets application code obtain a broader style surface
than the target component declares.

The clearest current example is Flex. Its public type omits `gap`, `columnGap`,
and `rowGap`, but application code uses `Box as={Flex}` with `gap`. The runtime
result is a Flex layout with a gap, while a direct `<Flex gap="..." />` is
rejected by TypeScript.

The same pattern occurs with Stack and SimpleGrid. Some instances are merely
historical or redundant wrappers; others combine a grid-item role with a layout
primitive on one DOM node. Those two cases must not be treated alike.

The automotive-web application audit found 23 `as={Stack}`, 4
`as={SimpleGrid}`, and 11 `as={Flex}` occurrences. In contrast, many
`as={NextLink}` uses are intentional semantic polymorphism and are out of
scope.

## Preconditions

This is deliberately a later decision. Do not start the migration until all of
the following are complete:

1. **Adapter aliases:** Decide and implement the public status of legacy
   aliases such as `spacing`, `spacingX`, `spacingY`, and `textColor`. A direct
   replacement must not silently lose an alias conversion supplied by Box or an
   adapter.
2. **Layout primitive surfaces:** Expose the selected Chakra prop surfaces
   consistently. In particular, Flex must expose its chosen gap props.
3. **Canonical contracts:** Stack and SimpleGrid must no longer have a narrower
   internal type competing with their public contract.
4. **Documentation:** Storybook and component types must describe the chosen
   direct-use contract.

## Migration categories

Every current occurrence is classified before changing it.

| Category | Example | Action |
| --- | --- | --- |
| Direct workaround | `<Box as={Flex} gap="md" />` | Replace with `<Flex gap="md" />` after Flex supports `gap`. |
| Redundant wrapper | `<Box as={Stack} ...>` using only Stack's supported surface | Replace with `<Stack ...>`. Convert any removed alias explicitly. |
| Same-node layout composition | `<GridItem as={Stack} area="content" />` | Keep only when one node must be both a grid item and a Stack; otherwise use a direct primitive or an explicit wrapper. |
| Semantic polymorphism | `<Box as={NextLink} href="..." />` | Retain; this is outside the rule. |

The migration must not add a wrapper solely to satisfy a type. Extra DOM nodes
can change grid, flex, selector, accessibility, and spacing behavior. A
same-node composition exception is therefore preferable when the layout
requires it, but it must be named and reviewable.

## Validation rule

Introduce a focused lint or static-analysis rule after the migration:

- reject `Box as={Stack}`, `Box as={SimpleGrid}`, and `Box as={Flex}` by
  default;
- allow an explicit, documented exception for approved same-node composition;
- do not flag intrinsic elements, framework links, or unrelated polymorphic
  component use;
- begin as a warning while the inventory is migrated, then promote it to an
  error once the exception policy is stable.

The rule is a guardrail against bypassing an intentional component boundary;
it is not a substitute for deciding the boundary itself.

## Compatibility requirements

- Preserve visual layout and DOM shape for each migrated call site unless an
  explicitly reviewed component change says otherwise.
- Preserve the selected adapter-alias behavior established by the earlier
  decision.
- Typecheck every affected application after the change.
- Do not claim a direct replacement until the target primitive accepts every
  prop used by that call site, after any agreed alias conversion.

## Non-goals

- Deciding which Chakra props layout primitives expose.
- Keeping, renaming, or removing adapter aliases.
- Replacing all use of `as`.
- Changing design-system primitive or shared product-component contracts.

## Source locations

- `src/components/adapter/Box/index.tsx`
- `src/components/flex/index.tsx`
- `src/components/stack/index.tsx`
- `src/components/simpleGrid/index.tsx`
- `automotive-web/packages/components/src/components/adapter/Box/index.tsx`
- `automotive-web/apps/seller-web/shared/components/packages/card/indicator/progressBar/index.tsx`
- `automotive-web/apps/listings-web/features/sellers/presentation/about/ImageTextSection.tsx`
