# Design-system primitive Chakra props

**Status:** Exploring — recommendation for discussion, no decision yet.

## Question

Should every design-system primitive expose a consistent default set of Chakra
props? In particular, should components accept external-layout props such as
margin or width as a convenience for consumers?

This concerns the **design-system primitive** category (currently named “UI
primitives” in `component-categories.md`), not broad Chakra-backed layout
primitives. The latter are covered by
[Layout primitive Chakra props](./rfc-decision-layout-primitive-props.md).

## Evidence from automotive-web

The all-web inventory records Chakra-surface props actually used by
automotive-web. To compare aliases fairly, the table combines shorthand and
long-form usages into the long-form family mandated by the
[long-form decision](./rfc-decision-longform-props.md).

The grouped figures below are a discussion aid. See the complete,
component-by-component [automotive-web usage inventory](./automotive-web-inventory.md)
for every observed custom prop, Chakra-surface prop, resolved spread, and
unresolved spread.

| Chakra prop family | Primitives using it | Occurrences | Likely ownership |
| --- | ---: | ---: | --- |
| All margin props | 15 | 502 | **External layout** — spacing around a component |
| All padding props | 13 | 127 | **Internal presentation** — spacing inside a component |
| Width and min/max width props | 11 | 242 | Component-specific sizing; often affects the component’s own behaviour |
| Height and min/max height props | 7 | 71 | Component-specific sizing; often affects the component’s own behaviour |
| Typography props | 15 | 1,201 | Internal presentation, concentrated in text-like primitives |
| `display` | 11 | 67 | Component-specific visual/layout behaviour |

No Chakra-surface prop is used by every matched design-system primitive. The
largest cross-cutting external-layout candidate is the margin family, used by
15 components. That is still far short of a category-wide default, and the
components are not representative of inputs, selectors, overlays, compound
controls, or other semantic primitives.

Margin and padding also have meaningfully different usage shapes. Ten
components use both; `Badge`, `Breadcrumbs`, `H5`, `MarkedText`, and
`Pagination` use margin without padding; `AccordionButton`, `AccordionPanel`,
and `TabList` use padding without margin. This supports treating margin as
external placement and padding as component-internal presentation.

The evidence set contains 72 design-system primitive rows that match the
automotive-web inventory, with 3,165 Chakra-prop occurrences excluding
`children`. `component-categories.md` lists 96 UI primitives, but 24 of those
names do not appear in this inventory. That category/inventory mismatch should
be reconciled before adopting a policy that claims to cover every primitive.

## Interpretation

The evidence supports a distinction between **external layout** and
**component-internal presentation**:

- **External margin** is the only credible candidate for a category-wide
  convenience API. It controls space *around* a component and does not change
  that component’s visual construction.
- **Padding** controls space *inside* a component. It is therefore part of the
  component’s presentation and remains selective.
- **Width and height** influence a component’s own sizing and can have
  component-specific consequences (for example, full-width buttons or skeleton
  dimensions). They remain selective.
- **Typography, colour, text alignment, and display** are presentation or
  inner-layout controls. They are fair game where currently used, but should
  carry over only to those components; they are not defaults for other
  primitives.

The remaining foundational question is therefore narrow: whether external
margin is a deliberate convenience for **every** design-system primitive, or
remains an opt-in per-component capability. A margin prop may be convenient for
consumers, or it may be layout that belongs on a parent `Stack`, `Flex`, or
`Box`. Frequency alone cannot decide that ownership.

## Options

### A. No default Chakra props; selective per-component contracts

Each primitive keeps the evidence-backed Chakra props that are appropriate to
its semantics. The freeze-interface inventory is the starting allow-list;
design review decides whether each observed prop remains or moves to a parent
layout component.

This is the smallest and most predictable contract. It prevents an input,
tooltip, or compound control from becoming a generic visual container merely
because text components need margins.

### B. A universal external-margin contract

Every design-system primitive receives the full long-form external-margin
family (overall margin plus logical and physical directional margins, with
responsive values where Chakra supports them).

This makes outer spacing predictable and avoids wrapper elements for a common
consumer need. It is a deliberate ergonomics decision, not one demonstrated by
every current component, and expands the public contract of primitives that do
not use margins today.

### C. A broad universal Chakra base

Every design-system primitive receives a fixed set such as `margin*`, `width`,
and `display`.

This is easy to explain but is not supported by the observed usage. It would
make many semantic components accept styling knobs with no demonstrated need
and would create an additional public compatibility commitment.

## Recommendation for discussion

Choose **Option A** unless the team explicitly values universal margin
convenience enough to choose Option B. Use the actual all-web usage as the
initial per-component allow-list, then make an explicit design decision for
each prop that is more appropriately owned by the parent layout.

In either case, do **not** introduce padding, width, height, typography, colour,
text alignment, or display by default. Carry those props forward exactly where
they are used today, subject to the component-by-component interface freeze.

## Decisions still needed

- Is external margin an accepted convenience for every design-system primitive
  (Option B), or should it remain selective (Option A)?
- For a prop that is currently used but appears to be parent-owned layout,
  should we migrate automotive-web before freezing the narrower interface?
- How should the category/inventory mismatch be reconciled before this policy
  is applied to every design-system primitive?

## Next validation step

For each primitive, compare its exact inventory row, exported type, and
forwarding behaviour. Retain only its semantic props plus the selected Chakra
props; typecheck all automotive-web consumers after each component family is
narrowed. Treat the result as non-breaking only for the validated consumer set.

## Sources

- `automotive-web-inventory.md` at commit `f336598d`
- `component-categories.md`
- `rfc-decision-freeze-interfaces.md`
- `rfc-decision-longform-props.md`
