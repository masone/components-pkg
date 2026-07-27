# Component interface examples

This document records the narrow-interface pattern used when replacing a broad
Chakra prop inheritance with the minimum public API used by our applications.

## Accordion

`Accordion` currently inherits the full Chakra root-prop surface. The initial
allow-list below contains the props observed in the seller-web inventory. Any
dynamic use omitted from the inventory is intentionally discovered by the
consumer typecheck and added only when required.

```tsx
import React, { type FC, type PropsWithChildren } from 'react';
import {
  Accordion as ChakraAccordion,
  type AccordionRootProps as ChakraAccordionRootProps,
  type RecipeVariantProps,
  useSlotRecipe,
} from '@chakra-ui/react';

import { accordionRecipe } from '@/src/themes/shared/slotRecipes/accordion';

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

export type AccordionVariantProps = RecipeVariantProps<
  typeof accordionRecipe
>;

export type AccordionProps = PropsWithChildren<
  AccordionRootUsageProps & AccordionVariantProps
>;

export const Accordion: FC<AccordionProps> = (props) => {
  const recipe = useSlotRecipe({ key: 'accordion' });
  const [recipeProps, restProps] = recipe.splitVariantProps(props);
  const { children, ...rest } = restProps;

  return (
    <ChakraAccordion.Root {...rest}>
      {React.Children.map(children, (child) =>
        React.isValidElement(child)
          ? React.cloneElement(
              child as React.ReactElement<{
                variant: typeof recipeProps.variant;
              }>,
              { variant: recipeProps.variant },
            )
          : child,
      )}
    </ChakraAccordion.Root>
  );
};
```

`AccordionVariantProps` remains public because recipe variants are a deliberate
part of the component contract. The explicit `Pick` is the boundary: it keeps
only the inherited Chakra root props used today, rather than exposing the
entire Chakra root surface.

## Migration loop

1. Seed the interface with the props observed in the consumer inventory.
2. Replace broad Chakra prop inheritance with an explicit `Pick` or locally
   owned semantic props.
3. Typecheck every consumer. Add a prop only when an existing use requires it.
4. Run tests and visual checks to catch changes in forwarded DOM or style props.

This gives each component a minimal, evidence-based API while keeping recipe
variants explicit and reusable.
