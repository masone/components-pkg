# Accordion migration notes

## Purpose

This note records the Accordion discussion so it can be reviewed independently of the
component inventory. It describes the current implementation, why it is hard to classify,
and a low-breakage path to the intended end state. It is a migration/design note, not an
implementation request.

## The underlying decision

The package must make its contract explicit for each export:

1. **Customization layer / primitive**: it is Chakra. Consumers use Chakra documentation,
   Chakra composition, and Chakra props. The package supplies the configured theme and
   registered recipes.
2. **Abstraction / generic component**: it is an owned semantic API. Consumers use package
   documentation and semantic props; Chakra types, style props, and composition are internal.

The problematic state is a **partial abstraction**: exposing most Chakra props while changing
the composition and rendering. The consumer cannot reliably use either the Chakra docs or a
small owned API.

The preferred target for Accordion is **a Chakra primitive with a registered slot recipe**.
The current Accordion wrapper is best understood as a temporary compatibility facade, not as
the desired generic-component design.

## Current implementation

`src/components/accordion/` exports four wrappers:

- `Accordion`: `ChakraAccordionRootProps & RecipeVariantProps<typeof accordionRecipe>`
- `AccordionItem`: `ChakraAccordionItemProps & RecipeVariantProps<typeof accordionRecipe>`
- `AccordionButton`: `ChakraAccordionItemTriggerProps & RecipeVariantProps<typeof accordionRecipe> & { leftIcon?: ReactNode }`
- `AccordionPanel`: `ChakraAccordionItemBodyProps & RecipeVariantProps<typeof accordionRecipe>`

The `accordion` slot recipe is already registered in
`src/themes/shared/slotRecipes/index.ts`. Its only public visual variant is:

```ts
variant?: 'light' | 'dark' | 'minimal'
```

It has no `size` variant today.

The wrappers do the following:

- Root splits out recipe props and forwards all remaining Chakra root props to
  `ChakraAccordion.Root`.
- Root clones immediate children solely to inject `variant`.
- Item, trigger, and panel accept and forward their Chakra slot prop types.
- Trigger adds `leftIcon`, wraps the label in a flexing left-aligned span, and automatically
  renders `Accordion.ItemIndicator`.
- Panel automatically creates `ItemContent` and `ItemBody`; it gives Chakra's existing `pb`
  prop special treatment by merging it into recipe CSS.

Consequently, current callers can pass Chakra-specific root props such as `colorPalette`,
`asChild`, `defaultValue`, `value`, `onValueChange`, `multiple`, `collapsible`, `lazyMount`,
and `unmountOnExit`; other Chakra style/layout props also flow through. The same broad Chakra
surface is exposed on individual slot wrappers.

`leftIcon` is **not** a root prop. It is only present on the separately exported
`AccordionButtonProps` type. The root does not forward it through `...rest`; callers pass it
directly to `<AccordionButton leftIcon={...}>`.

## Why this is confusing

The implementation does not literally send a root prop to an unrelated Chakra slot: root
props go to `Root`, item props to `Item`, trigger props to `ItemTrigger`, and body props to
`ItemBody`. The problem is the overall public contract:

- It exposes almost all Chakra props, signalling that Chakra documentation applies.
- It changes the JSX composition that consumers are expected to write.
- It inserts rendering and layout decisions (indicator, label wrapper, content/body nesting).
- It manually propagates the visual variant through child cloning.
- It lets recipe-related types appear on all public slots, rather than selecting one root
  visual decision.

Chakra's behavioural prop documentation is therefore mostly relevant, while its compound
composition documentation is not a reliable description of the package export. That is the
partial-abstraction problem.

## The idiomatic primitive target

For a customization-layer primitive, retain the recipe and expose Chakra's compound namespace
unchanged. The recipe owns the slots; Chakra owns behaviour, accessibility, mounting, and
composition.

Conceptually:

```ts
export { Accordion } from '@chakra-ui/react';
export type {
  AccordionRootProps,
  AccordionItemProps,
  AccordionItemTriggerProps,
  AccordionItemBodyProps,
} from '@chakra-ui/react';
```

Run Chakra type generation after registering the slot recipe so the recipe's
`light | dark | minimal` variant is available to TypeScript.

Consumers would use the documented Chakra composition:

```tsx
<Accordion.Root defaultValue={['details']} collapsible variant="light">
  <Accordion.Item value="details">
    <Accordion.ItemTrigger>
      <HStack flex="1" gap="sm">
        <InfoIcon />
        <span>Details</span>
      </HStack>
      <Accordion.ItemIndicator />
    </Accordion.ItemTrigger>

    <Accordion.ItemContent>
      <Accordion.ItemBody>Content</Accordion.ItemBody>
    </Accordion.ItemContent>
  </Accordion.Item>
</Accordion.Root>
```

No wrapper, manual `useSlotRecipe`, child cloning, or bespoke slot prop type is needed.
`leftIcon` becomes normal explicit composition. If icon-plus-label triggers prove to be a
repeated product pattern, they can become a separately named component with an honestly
closed semantic API; they should not justify a near-complete alternate Accordion API.

## What a direct re-export would change

It would preserve:

- Chakra state and accessibility behaviour.
- Controlled and uncontrolled values, multiple/collapsible state, lazy mounting, and the
  rest of Chakra's root API.
- The `light`, `dark`, and `minimal` visual styling, assuming the registered recipe is active.
- Chakra slot style props for callers who intentionally use the primitive API.

It would remove wrapper convenience and require a JSX migration:

| Current facade | Chakra primitive |
|---|---|
| `Accordion` | `Accordion.Root` |
| `AccordionItem` | `Accordion.Item` |
| `AccordionButton` | `Accordion.ItemTrigger` |
| `AccordionPanel` | `Accordion.ItemContent` + `Accordion.ItemBody` |
| `leftIcon` prop | explicit icon/label children |
| automatic indicator | explicit `Accordion.ItemIndicator` |

That is a real consumer migration, even though it does not discard meaningful Accordion
behaviour.

## Options

### Option A — immediate primitive conversion

Replace the wrappers with direct re-exports now and migrate all call sites in one change.

- **Architecture:** clean immediately.
- **Breakage:** high; import names and JSX composition change everywhere.
- **Use when:** a coordinated major/codemod is already planned and the usage count is known.

### Option B — compatibility facade plus additive primitive API (recommended)

Keep the present exports working temporarily, but classify them as a **legacy compatibility
facade with a primitive target**. Add an explicit direct-Chakra Accordion API alongside it—for
example in a `primitives` package/subpath where it can correctly be named `Accordion`.

- Existing applications continue to use the facade without a migration.
- New code uses the primitive API and follows Chakra documentation.
- Mark facade exports deprecated in types/docs; freeze their API and prevent new use with lint
  where practical.
- Add an AST codemod and migrate opportunistically or in a later major.
- Delete the facade only after adoption has reduced its usage.

If a primitives package/subpath is not available yet, an additive transition export such as
`AccordionPrimitive` is less elegant but makes the contract clear. Do not call the transition
export a generic component.

### Option C — retain as a true generic component

Keep the custom JSX composition, but close the API: own and document root state props,
`variant`, `leftIcon`, and explicit semantic item/trigger/panel props; remove all Chakra prop
type extension/spreading and raw style props.

- **Architecture:** internally coherent but much more implementation/design work.
- **Breakage:** substantial because all currently accepted Chakra props must be removed or
  replaced.
- **Assessment:** not justified by the current value added (`leftIcon` and a little automatic
  markup). This option should be chosen only if product requirements identify a genuinely
  distinctive Accordion experience.

## Recommended path and guardrails

Choose **Option B**. It acknowledges reality without requiring an immediate rewrite:

1. Confirm the direct Chakra namespace receives the registered slot recipe in every package
   theme/system; run type generation.
2. Add the primitive export as an additive API.
3. Document the existing wrappers as legacy; do not add props or new wrapper patterns.
4. Prefer the primitive in all new code.
5. Build a codemod that renames tags, expands `AccordionPanel`, adds the indicator, and turns
   `leftIcon` into explicit children. Review unusual trigger content manually.
6. Set a later-major removal criterion based on measured facade usage.

This avoids treating all existing wrappers as urgent refactors while preventing the package
from accumulating further partial abstractions.
