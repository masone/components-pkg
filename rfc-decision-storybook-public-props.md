# RFC decision: Document every public prop in Storybook

> **Status:** Proposed

## Proposal

Storybook must display every prop in a component's exported TypeScript contract.
The generated Docs prop table is authoritative for the public surface; a prop
must not disappear simply because it is inherited from Chakra, HTML, or React.

We keep automatic prop inference through `react-docgen-typescript` and retain
manual story metadata. Manual `argTypes` improve an inferred prop's control,
options, mapping, description, or table treatment; they do not define the
complete documented API. Story `args` remain example/default values only.

This is an additive documentation change. The frozen public interfaces keep the
result readable; a prop removed from the TypeScript contract disappears from
Storybook automatically.

## Why this matters

The RFC identifies an incomplete Storybook surface as a central developer
experience problem: consumers currently have to infer available capabilities
from Chakra documentation, types, or component implementation. That defeats
the package's role as the product-facing API.

Showing the actual exported contract makes Storybook, TypeScript, and component
documentation agree. It also makes inherited props a deliberate, visible
commitment rather than an accidental escape hatch.

## Current implementation

Automatic extraction is already enabled in
[.storybook/main.ts](./.storybook/main.ts):

```ts
typescript: {
  reactDocgen: 'react-docgen-typescript',
  reactDocgenTypescriptOptions: { /* … */ },
}
```

However, the same file's `propFilter` removes every Chakra Styled System prop,
inherited HTML/DOM prop, and React prop. It also removes `as`, `asChild`, and
`recipe`; only `variant` and `size` are explicitly retained.

Stories then add manual metadata on top:

- 77 of the 94 current stories declare `argTypes`; 90 declare `args`.
- [Badge's story](./src/components/badge/index.stories.tsx) adds controls
  generated from its registered recipe.
- [Stack's story](./src/components/stack/index.stories.tsx) manually configures
  interactive controls and mappings for selected props.
- [Recipe controls](./.storybook/preview/controls/recipe.ts) are generated from
  the configured recipe variants.
- [Global controls](./.storybook/preview.tsx) improve common token and colour
  controls when the corresponding prop exists.

This is the intended split: docgen supplies the complete prop table; manual
metadata improves how selected props are demonstrated and edited.

## Necessary changes

1. **Remove the global `propFilter` from
   [.storybook/main.ts](./.storybook/main.ts).** Do not exclude Styled System,
   HTML/DOM, React, `as`, `asChild`, or `recipe` at extraction time.
2. **Keep `react-docgen-typescript` enabled.** It remains the source for the
   generated Docs table and inferred controls.
3. **Preserve existing story `argTypes` and `args`.** They continue to
   override or augment individual inferred prop configurations; they are not
   a replacement for generated documentation.
4. **Use `control: false` for non-interactive or complex props where needed.**
   This keeps a prop documented while avoiding an unusable control. Do not use
   `table.disable` for public props, because that hides the Docs row.
5. **Review the initial unfiltered build.** Confirm every public prop appears
   in Docs; add descriptions, categories, token controls, or disabled controls
   only where they improve comprehension.
6. **Add a regression check.** For a representative transparent primitive and
   an owned component, assert that the generated Docs output includes every
   exported prop. Run `npm run build:storybook` in CI.

## Examples

`Badge` currently obtains `variant` controls from its recipe helper:

```ts
argTypes: {
  ...getRecipeControls('badge'),
}
```

After the change, that remains an enhancement. The generated Docs table also
shows every public `BadgeProps` member without each story manually listing it.

`Stack` manually supplies select controls and JSX mappings for `direction`,
`justify`, `wrap`, `children`, and `separator`. Those controls remain.
Its other exported props are still documented automatically, rather than being
hidden because they originate in Chakra's type hierarchy.

## Validation

- Build Storybook and inspect the Docs prop tables for `Box`, `Stack`,
  `Badge`, and `Button`.
- Confirm a manual control still overrides the inferred control for a recipe
  variant and a mapped JSX value.
- Confirm that a complex event or React-node prop can be documented with its
  control disabled.
- Compare the visible Docs rows with each component's exported TypeScript
  interface after the interface-freeze work.

