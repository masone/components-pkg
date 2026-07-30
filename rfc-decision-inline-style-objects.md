# Inline style-object policy

**Status:** Exploring — discussion proposal; no implementation decision yet.

## Question

What policy should guide new uses of inline style objects in product code and
the public APIs of components-pkg?

The term “inline style object” here includes:

- React's `style={{ … }}` and `style={someStyle}`;
- Chakra's `css={{ … }}` and `sx={{ … }}`; and
- public component props that deliberately expose one of those escape hatches.

It does not include static stylesheet files, theme/recipe definitions, or a
named style object that is internal to a component unless it is passed through
one of the APIs above.

## Why discuss this now

An inventory of automotive-web found these render-time call sites:

| Surface | Call sites | Files |
| --- | ---: | ---: |
| React `style` prop | 125 | 82 |
| Chakra `sx` prop | 1 | 1 |
| Chakra `css` prop | 206 | 106 |

Chakra 3 uses `css` much more than `sx`, so a policy focused only on
`style` or `sx` would not address the main styling escape hatch.

The inventory also shows that the uses are not one category:

- many `style` uses are image fit/position, dynamic drag or crop geometry,
  CSS custom properties, or browser/accessibility workarounds;
- some are static layout, raw colours, borders, spacing, and dimensions that
  could instead use component tokens, Chakra props, recipes, or named styles;
- some `css` uses compose Chakra recipes or runtime CSS variables and are
  implementation mechanisms for component primitives, not arbitrary product
  styling.

Frequency alone therefore does not establish that all of these APIs should be
removed or linted.

## Goals

- Make the intended ownership of visual rules clear.
- Preserve legitimate runtime and integration use cases.
- Avoid turning every component into an unrestricted visual container by
  exposing generic style escape hatches.
- Keep any enforcement understandable and low-friction.

## Non-goals

- Retrofitting every existing call site before a policy is agreed.
- Banning Chakra recipes, CSS variables, print/media rules, or third-party
  integration styles by default.
- Treating every style object as equally problematic.

## Options

### A. Keep the current capabilities; document an explicit policy

Retain existing `style`, `css`, and `sx` capabilities. Document a preference
for tokens, component props, recipes, and named styles for static visual rules,
while allowing inline objects where they make the code clearer.

This is a valid outcome if the team judges the current flexibility more useful
than the cost of a hard boundary. It supplies shared review guidance without
introducing migration work or lint exceptions.

### B. Discourage new ad-hoc inline objects; no API removal

Document a stronger default: static layout and visual styling should use the
design system, component props, recipes, or a named colocated style. Permit
inline objects for dynamic values, image/third-party contracts, CSS custom
properties, media/print rules, and narrowly scoped browser workarounds.

Initially apply this in code review. A lint rule could later warn on direct
`style={{ … }}`, `css={{ … }}`, and `sx={{ … }}` usage, with a documented
suppression mechanism, once the exception cases and false-positive rate are
understood.

### C. Restrict component APIs and introduce targeted linting

Adopt Option B and also stop exposing generic styling escape hatches from
design-system primitives by default. New public `style`, `css`, or `sx` props
would require an explicit component-level rationale; existing public props
would be retained, narrowed, or migrated only after consumer validation.

A targeted lint rule could reject raw colour and spacing literals inside these
objects, or reject inline object literals while allowing named recipe objects.
This gives the strongest nudge but requires careful design to avoid punishing
legitimate dynamic and framework-integration code.

## Decision points

- Is the problem primarily inconsistent values, unclear component contracts,
  or the readability/maintainability of local style objects?
- Should the policy distinguish product applications from the components-pkg
  implementation?
- Which exceptions are intentionally permanent: image fit/position, dynamic
  geometry, third-party contracts, CSS variables, and print/media rules?
- Should public component APIs expose generic style props at all, and does that
  answer differ for layout primitives versus semantic primitives?
- If linting is desired, should it begin as reporting-only, and what is the
  smallest rule that would be useful without generating routine suppressions?

## Proposed discussion outcome

Choose one of Options A–C, then record a short policy with examples of allowed
and discouraged uses. Do not remove existing component props or add a blocking
lint rule until the selected policy has identified its exceptions and a sample
rule has been evaluated against the existing inventory.

## Sources

- automotive-web inline-style inventory, July 2026
- [Design-system primitive Chakra props](./rfc-decision-design-system-primitive-props.md)
- [Freeze interfaces](./rfc-decision-freeze-interfaces.md)
