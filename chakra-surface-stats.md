# Seller-web Chakra surface-prop learnings

This memo turns the seller-web usage inventory into decision support for
narrowing component prop contracts. It is evidence about **seller-web**, not a
universal contract for every consumer of `@smg-automotive/components`.

## Sources and scope

- [Seller-web prop-contract usage](https://github.com/masone/components-pkg/blob/93fdb93ade00cd22204eca2e2e524819147c1b20/seller-web-prop-contract-usage.md)
- [Seller-web inventory](https://github.com/masone/components-pkg/blob/876643d0b1cf6d269ef754ffa3255c7dd43c385e/seller-web-inventory.md)
- [Component categories](https://github.com/masone/components-pkg/blob/93fdb93ade00cd22204eca2e2e524819147c1b20/seller-web-component-categories.md)

The inventory covers 130 public JSX components in 2,978 seller-web source
files. Counts are JSX prop occurrences, rather than a count of files or a
measure of visual importance. Explicit props and statically resolved spreads
are counted; unresolved spreads can hide additional uses.

`children`, `key`, `ref`, `data-*`, ARIA attributes, and other native/React
attributes are not evidence for a common Chakra styling contract. They need
their own intentional compatibility policy.

## Headline

The observed usage does **not** support a broad Chakra base type shared by all
UI primitives. No Chakra-surface prop is used by every UI primitive; even
`children` appears on 68 of the 94 UI primitives (75 have at least one JSX
use). The broadly inherited Chakra surface therefore remains much larger than
seller-web needs.

The useful distinction is:

1. **A shared vocabulary:** names and conventions that are acceptable when a
   component is intended to be customizable in that way.
2. **A per-component allow-list:** the actual props selected from that
   vocabulary, plus semantic component props.

This avoids both a 1,000+ prop inherited surface and a rigid rule that every
UI primitive must behave like a layout primitive.

## Where Chakra-surface usage occurs

| Category | Chakra prop occurrences | Excluding `children` | Distinct props observed | Interpretation |
| --- | ---: | ---: | ---: | --- |
| Layout primitives | 6,707 | 4,598 | 200 | The right place for the broadest curated layout surface. |
| UI primitives | 3,788 | 1,877 | 134 | Surface use is concentrated in text, headings, buttons, links, and a few wrappers. |
| Shared product components | 54 | 2 | 3 | Strong evidence for domain-owned, narrow contracts rather than generic Chakra pass-through. |

The largest individual consumers of non-`children` Chakra props are `Box`
(2,503), `Flex` (830), `Text` (807), `GridItem` (438), `Stack` (367), `Link`
(321), and `Grid` (314). This concentration is why layout primitives and a
small group of text-like UI primitives should not define the public contract
of every component.

## Alias conventions seen in seller-web

The data does not support an "always use shorthand" rule. It supports a
family-specific convention instead.

| Chakra surface family | Shorthand uses | Longhand uses | Seller-web convention indicated by usage |
| --- | ---: | ---: | --- |
| Width (`w` / `width`) | 91 | 332 | Prefer longhand. |
| Height (`h` / `height`) | 44 | 120 | Prefer longhand. |
| Min/max dimensions | 71 | 63 | Close; shorthand is slightly ahead. |
| Margin | 503 | 276 | Shorthand is common, especially directional margins. |
| Padding | 133 | 371 | Prefer longhand. |
| Background | 78 | 183 | Prefer longhand. |
| Radius | 10 | 158 | Strongly prefer `borderRadius`. |
| Position | 1 | 83 | Strongly prefer `position`. |
| Flex alignment and direction | 75 | 549 | Prefer `alignItems`, `justifyContent`, and `flexDirection`. |

Directional margin shorthands are the meaningful exception: `mb` has 219
uses, `mt` 118, `ml` 59, and `mr` 58. A future normalization could prefer
longhand names, but removing these aliases first would create avoidable
seller-web churn. If they are not retained in the contract, codemod callers
before enforcing the narrower type.

## UI primitives: evidence for a shared vocabulary

The following table is deliberately a *reverse index*. It shows which UI
primitives actually create the evidence for the most common surface props.
The number in parentheses is the number of JSX uses.

| Prop | Components | UI primitives using it |
| --- | ---: | --- |
| `display` | 11 | AccordionButton (2), Button (1), CardFooter (1), Divider (2), H2 (1), H3 (1), H4 (1), Link (7), List (4), ListItem (2), Text (17) |
| `width` | 9 | Button (50), Divider (4), H3 (1), Link (10), List (1), ModalCloseButton (2), Skeleton (3), Td (1), Text (2) |
| `w` | 8 | AccordionButton (2), Button (13), H2 (1), H3 (1), Link (3), Table (2), Td (1), Text (1) |
| `height` | 6 | Button (14), CardHeader (1), Divider (1), ModalCloseButton (2), Skeleton (3), Text (1) |
| `marginTop` | 6 | Button (1), CardHeader (1), Divider (1), Link (1), Pagination (7), Text (2) |
| `mt` | 7 | Button (11), Divider (5), H1 (1), H3 (4), H4 (3), Link (9), Text (22) |
| `marginBottom` | 9 | Button (6), CardHeader (1), Divider (7), H1 (8), H2 (5), H3 (5), H4 (3), Pagination (4), Text (20) |
| `mb` | 11 | Button (4), Divider (3), H1 (5), H2 (5), H3 (13), H4 (13), H5 (1), ListItem (1), MarkedText (2), Table (2), Text (59) |
| `marginLeft` | 2 | Button (2), Text (3) |
| `ml` | 6 | Button (1), H2 (1), H4 (6), Link (5), ModalCloseButton (1), Text (18) |
| `marginRight` | 2 | Button (1), Text (2) |
| `mr` | 4 | Button (3), MarkedText (2), ModalCloseButton (1), Text (24) |
| `padding` | 5 | AccordionPanel (2), CardBody (1), Link (13), ModalCloseButton (1), Text (1) |
| `paddingX` | 7 | AccordionButton (2), AccordionPanel (1), Button (3), CardHeader (1), DrawerBody (1), Link (1), Text (4) |
| `px` | 3 | AccordionButton (1), AccordionPanel (2), TabList (1) |
| `paddingY` | 4 | DrawerBody (1), H3 (1), Link (1), Text (6) |
| `py` | 3 | AccordionButton (2), Td (6), Text (3) |
| `color` | 8 | AccordionButton (1), H1 (1), H2 (1), H3 (10), H4 (1), Link (37), ModalCloseButton (3), Text (113) |
| `fontWeight` | 10 | AccordionButton (1), Button (4), H2 (1), H3 (2), H4 (4), H5 (4), Link (3), MarkedText (1), Td (2), Text (114) |
| `textAlign` | 8 | CardHeader (1), H1 (1), H2 (7), H3 (4), H4 (4), Link (3), List (2), Text (49) |
| `textStyle` | 10 | AccordionButton (1), AccordionPanel (1), H1 (7), H2 (3), H3 (2), H5 (2), Link (4), ListItem (1), MarkedText (2), Text (157) |

The apparent cross-component usage is still modest: the highest non-React
surface coverage is 11 of 94 UI primitives (about 12%). `Text` and heading
components account for much of the usage of `textStyle`, `color`,
`fontWeight`, and `textAlign`. These are strong candidates for text-like
components, not evidence that `Select`, `Tooltip`, `Checkbox`, and every
other UI primitive should accept them.

## Decision options

### Option A — Per-component allow-lists (recommended)

For each component, start with the observed props in the usage report, then
allow only the appropriate subset of the shared vocabulary. This is the
smallest evidence-based public contract and keeps semantic components from
turning into generic layout containers.

Examples of appropriate prop clusters:

- **Text and headings:** `color`, `fontWeight`, `textStyle`, `textAlign`, and
  the relevant margin props.
- **Buttons and links:** dimensions plus the few margin and layout props that
  are actually used.
- **Panels and card parts:** only the padding, background, border, or layout
  props demonstrated for that particular part.
- **Inputs, selectors, overlays, and compound controls:** semantic props by
  default; add individual surface props only where usage makes the case.

### Option B — A global UI-primitive base type (not supported by current usage)

If the team chooses a global base type for ergonomics, it should be an
explicit product decision rather than a conclusion from seller-web evidence.
A deliberately small candidate vocabulary would be:

```ts
type CommonUiSurfaceProps = Pick<ChakraProps,
  | 'display'
  | 'width'
  | 'marginTop'
  | 'marginBottom'
  | 'color'
  | 'fontWeight'
  | 'textAlign'
  | 'textStyle'
>;
```

This should be opt-in for text- or layout-capable UI primitives, not inherited
by all 94. Existing shorthand callers (`mb`, `mt`, `w`) require either
component-specific compatibility aliases or a deliberate migration.

## Reviewing one-off and concentrated uses

One-off use is not automatically a reason to retain a public prop globally.
Review it in the owning component's allow-list, using this order:

1. **High occurrence count on one component:** retain or migrate deliberately.
   For example, `width` on `Button` (50), `color` on `Link` (37), and
   `textStyle` on `Text` (157) are concentrated but clearly meaningful.
2. **Used by several component kinds:** consider adding it to the shared
   vocabulary, but still select it per component. `mb`, `display`,
   `fontWeight`, and `textStyle` belong in this discussion.
3. **One or a few occurrences:** treat it as a targeted design review. Decide
   whether it describes real component customization, should move to the
   outer layout, or is better expressed by a semantic prop.
4. **No direct or resolved-spread use:** remove from the seller-web contract
   candidate unless another consumer establishes a requirement.

The important design question is often whether spacing belongs to the
component or to the parent layout. The usage data shows that margin and
padding are common, but it cannot answer that ownership question. The team
can deliberately choose to move outer margins to layout wrappers and then
refactor seller-web before narrowing component types.

## Safe adoption loop

1. Define the vocabulary and the prop-sourcing rule.
2. Produce a per-component allow-list from the usage report.
3. Decide intentional exceptions, including spacing ownership and alias
   normalization.
4. Narrow one component type at a time.
5. Typecheck seller-web, inspect unresolved spreads, and add only props that
   prove necessary for valid consumers.
6. Run component tests and visual checks.

The resulting contract is non-breaking for the validated seller-web consumer
set, not automatically for every consumer of the package.
