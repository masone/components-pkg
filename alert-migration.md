# Alert migration notes

## Purpose

This note captures the Alert assessment separately from the component inventory. It explains
why Alert differs from Accordion, the current API and implementation, and the options for its
target contract. It is a design/migration note, not an implementation request.

## The contract test

The package must choose one of two honest contracts for an export:

1. **Customization layer / primitive**: Chakra API and documented Chakra composition remain
   intact; the package provides theme configuration and registered recipes.
2. **Generic component / abstraction**: the package owns a semantic API and composition;
   Chakra prop types, style props, and slot composition are internal.

The invalid middle ground exposes a large Chakra prop surface while changing the component
composition and rendering.

## Current implementation

`src/components/alert/index.tsx` defines the public type from:

```ts
type RootProps = Omit<ChakraAlertRootProps, 'status' | 'variant' | 'title'>;

type AlertProps = RootProps
  & Omit<RecipeVariantProps<typeof alertRecipe>, 'status'>
  & {
    description: string | ReactNode;
    title?: string;
    link?: { as?: 'link' | 'button' | ElementType; text: string; url?: string;
             isExternal?: boolean; onClick?: () => void };
    icon?: ReactNode;
    type?: 'error' | 'warning' | 'info' | 'success';
    dismissible?: boolean;
    onDismiss?: () => void;
  };
```

The dismissible/non-dismissible union requires `onDismiss` only when `dismissible` is true.

The component manually creates the complete Chakra compound structure:

- `Alert.Root`, with `status={type}`;
- `Alert.Indicator`, optionally containing the supplied icon;
- `Alert.Content`, `Alert.Title`, and `Alert.Description`;
- an optional custom `AlertLink` built on the package Link component;
- an optional package `CloseButton`.

Dismissal uses an internal `useDisclosure({ defaultOpen: true })`: clicking the close button
hides the alert locally and calls `onDismiss`. There is no public controlled visibility API.

`alertRecipe` is a registered slot recipe with slots `root`, `indicator`, `content`, `title`,
`description`, and `toastClose`. Its only variant is:

```ts
status?: 'info' | 'success' | 'warning' | 'error'
```

Despite registration, the component consumes the recipe manually with
`useSlotRecipe({ recipe: alertRecipe })` and manually applies CSS to every slot.

## What is valuable here

Unlike Accordion, this wrapper does more than rename Chakra parts. It turns compound JSX into
a stable message API:

```tsx
<Alert
  type="warning"
  title="Your browser is outdated"
  description="Update to continue."
  link={{ text: 'Learn more', url: '/help' }}
  dismissible
  onDismiss={trackDismissal}
/>
```

It owns the message layout, optional link, optional close action, and local dismissal
behaviour. Those are plausible generic component responsibilities.

## The current problem

The wrapper changes Chakra composition but also accepts almost all remaining
`ChakraAlertRootProps`. That leaks Chakra-specific configuration and styling such as
`colorPalette`, `asChild`, raw style/layout props, and arbitrary root attributes through a
component whose structure is fixed by the wrapper.

Therefore Chakra's root-prop documentation is partly applicable, while Chakra's compound
composition documentation is not. This is the partial-abstraction failure.

The implementation does not route a root prop to an unrelated slot. The issue is the public
contract, not a literal wrong-slot spread.

Additional inconsistencies:

- Public prop `type` maps to the recipe/Chakra concept named `status`.
- The nested `link` object carries its own `as`, `url`, and `isExternal` dialect rather than a
  clearly owned action/slot contract.
- Local dismissal is neither a fully controlled state model nor pure caller composition.
- Passing a recipe object directly rather than using the registered key makes recipe ownership
  less uniform.

## Options

### Option A — direct Chakra primitive

Expose Chakra's `Alert` compound namespace directly, retain the registered slot recipe, and
delete this wrapper.

```tsx
<Alert.Root status="warning">
  <Alert.Indicator />
  <Alert.Content>
    <Alert.Title>Your browser is outdated</Alert.Title>
    <Alert.Description>Update to continue.</Alert.Description>
  </Alert.Content>
</Alert.Root>
```

This is architecturally clean but removes the current title/description/link/dismissible
convenience API and requires consumer JSX migration. Use only if the team decides that the
semantic message abstraction has no value.

### Option B — honest generic semantic Alert (recommended)

Keep the fixed composition and message API, but remove Chakra prop sourcing and style
escapes. The recipe remains appropriate because Alert owns a reusable visual language.

An indicative target shape is:

```ts
type AlertProps = NativeDivAttributes & {
  status?: 'info' | 'success' | 'warning' | 'error';
  title?: ReactNode;
  description: ReactNode;
  icon?: ReactNode;
  action?: ReactNode; // or a deliberately typed link/action model
  dismissible?: boolean;
  onDismiss?: () => void;
};
```

The real native attribute type must exclude styling escape hatches such as `className` and
`style` under the component rules. No Chakra root type, `asChild`, `colorPalette`, `bg`, `p`,
or `css` belongs in this surface.

The implementation should consume the registered `alert` recipe consistently (for example by
key), keep all slot styling internal, and make an explicit decision about dismissal:

- preserve the current uncontrolled dismissal semantics; or
- introduce a deliberate controlled visibility API if applications need it.

`type → status` is a desirable vocabulary migration. A deprecated compatibility alias can
avoid an immediate break.

### Option C — temporary compatibility facade plus new primitive

Keep the current Alert API as a frozen legacy facade and add a direct Chakra Alert primitive
through a clear primitives package/subpath. New code uses Chakra composition; the old facade
migrates later.

This is appropriate if the team cannot yet decide whether the semantic message API is a
supported design-system component. It avoids forcing an immediate JSX migration but should
not become a permanent ambiguous export.

## Recommendation

Prefer **Option B**. Alert is meaningfully more than a recipe wrapper: it supplies a closed
message/notice composition, not just a renamed Chakra namespace. It is close to being an
honest generic component; the primary work is to replace `ChakraAlertRootProps` with native
attributes plus the owned semantic props, then settle the small naming/state decisions.

This makes Alert and Accordion useful counterexamples:

- **Accordion:** almost no bespoke value; primitive target with a temporary compatibility
  facade.
- **Alert:** meaningful semantic composition; generic-component target with a closed API.
