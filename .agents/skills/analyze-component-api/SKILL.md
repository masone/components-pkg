---
name: analyze-component-api
description: Analyze a component's minimum evidence-based public API using a seller-web component inventory and prop-contract usage report. Use when narrowing a component's TypeScript props, assessing unused Chakra/native surface area, or producing an Accordion-style allow-list and type pattern.
---

# Component API analysis

Use the prop-contract usage report as the API decision table and the component
inventory as the evidence and coverage table. Analyze one named component at a
time unless the user asks for a batch.

## Inputs

Require the component inventory report, prop-contract usage report, and
component source when proposing a type. Reports may be local, in a pinned Git
revision, or at user-supplied URLs.

Read the exact row for the requested component in both reports. Confirm their
JSX-use counts match; otherwise stop and report the discrepancy.

## Interpretation

- **Used exposed props** is the initial type allow-list.
- **Used outside named ComponentProps** is not automatically included. Treat it
  as a compatibility decision or call-site bug. Handle `key`, `data-*`, and
  other special/index-signature attributes with an explicit policy.
- **Unused component-declared props** are removal/deprecation candidates, not
  proof that removal is safe for every consumer.
- **Unused inherited Chakra/native props** measures broad inherited surface
  area. It is not a list of package-authored props; use it as the reason to
  replace broad inheritance with an explicit boundary.
- **Unresolved spreads** lower confidence. Validate the narrowed contract with
  all consumer typechecks even when the unresolved count is zero.
- Do not use the inventory's `custom`/ `Chakra-surface` classification as the
  type boundary; re-exports and conditional native props can mislead it.

## Workflow

1. Reconcile the two JSX-use counts.
2. Report used exposed props, outside-contract props, unused declared props,
   and resolved/unresolved spread counts.
3. Inspect the exported prop type and forwarding behavior. Verify selected
   inherited props exist before putting them in a `Pick`.
4. Produce an initial narrow type:
   - export a recipe variant type when variants are a deliberate public API;
   - use `Pick<ChakraComponentProps, ...>` only for evidence-backed inherited
     props that remain public;
   - add local semantic props directly; and
   - make `children` optional only when that remains intentional.
5. Narrow the type, typecheck every consumer, add only props required by
   existing valid use, then run tests and visual checks.

## Type pattern

Adapt names and keys to the requested component. Never copy the keys without
evidence from its contract-usage row.

```tsx
type RootUsageProps = Pick<ChakraRootProps, 'value' | 'onValueChange'>;

export type ComponentVariantProps = RecipeVariantProps<typeof componentRecipe>;

export type ComponentProps = PropsWithChildren<
  RootUsageProps & ComponentVariantProps
>;
```

If a recipe has more variants than the application currently uses, decide
whether the full recipe contract is intentional; use a `Pick` of the variant
type when narrowing it too.

## Required output

Lead with the conclusion. Include JSX count reconciliation, the initial
allow-list, outside-contract items intentionally excluded, unresolved-spread
risk, the proposed type shape, and the validation scope. Say "non-breaking for
the validated consumer set" unless all package consumers were typechecked.

