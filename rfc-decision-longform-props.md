# RFC decision: Use canonical Chakra utility prop names

> **Status:** Proposed

## Decision

Use canonical Chakra utility prop names in public component contracts and
application code. We will not preserve both an alias and its canonical Chakra
utility spelling for the same utility.

The canonical names are generally descriptive CSS-oriented names, such as
`width`, `marginBottom`, and `paddingInline`. They are more readable in JSX,
align with the existing migration guide's `marginTop` / `marginLeft`
convention, and make a component's frozen interface easier to understand
without Chakra alias knowledge.

The automotive-web inventory shows that both spellings are currently in use.
That is migration evidence, not a reason to keep duplicate public API. For
example, `Box` currently has 98 `mb` uses and 41 `marginBottom` uses, while it
has 74 `w` uses and 212 `width` uses.

## Why this matters

The RFC calls out a library where the same Chakra utility is expressed through
unpredictable vocabulary—such as `w` versus `width`—and where consumers must
inspect Chakra knowledge to understand the actual API. Keeping aliases doubles
that ambiguity in every frozen interface and in Storybook.

One readable spelling per Chakra utility makes interfaces easier to scan,
search, and teach. It also turns the migration into a mechanical, reviewable
change rather than leaving each component to choose a local alias convention.

This is a **vocabulary-only** decision. It does not decide which CSS,
HTML/native, Chakra, or composition-utility props a component exposes. Public
component contracts remain responsible for admitting deliberate prop sets; a
broad Chakra-backed prop type remains broad even when its aliases have been
removed.

## Canonical form

Use the descriptive CSS-oriented spelling:

| Use | Do not use |
| --- | --- |
| `width` | `w` |
| `height` | `h` |
| `marginTop` | `mt` |
| `marginBottom` | `mb` |
| `marginInline` | `mx`, `marginX` |
| `marginBlock` | `my`, `marginY` |
| `paddingInline` | `px`, `paddingX` |
| `paddingBlock` | `py`, `paddingY` |

The same rule applies to the remaining exact aliases supported by the pinned
Chakra version. Examples include `bg` → `background`, `bgColor` →
`backgroundColor`, `rounded` → `borderRadius`, `shadow` → `boxShadow`, and
`pos` → `position`.

`bg` and `bgColor` are not aliases of one another: `background` is the CSS
compound property, while `backgroundColor` sets only the colour. Likewise,
composition utilities such as `boxSize`, `lineClamp`, `spaceX`, and `ring` are
not aliases: they deliberately produce multiple declarations, selectors, or
conditions. They are outside this decision.

## Enforcement

The configured Chakra system is the authoritative alias map. Chakra records
each exact alias as a utility's `shorthand` entry. We remove selected
`shorthand` entries from the package's system configuration, then run Chakra
type generation. Removed aliases consequently cease to be accepted Chakra style
props and become TypeScript errors; no lint rule is required.

The filter retains every underlying Chakra utility. It only changes the
vocabulary used to reach that utility. It is not a blacklist of Chakra
utilities or CSS properties.

```ts
import { createSystem, defaultConfig } from "@chakra-ui/react"

const utilities = Object.fromEntries(
  Object.entries(defaultConfig.utilities ?? {}).map(([name, utility]) => [
    name,
    { ...utility, shorthand: undefined },
  ]),
)

export const system = createSystem({ ...defaultConfig, utilities })
```

The production implementation may retain any aliases deliberately left out of
the migration, but the filter is the enforcement point. It must replace the
base utility configuration; extending `defaultConfig` would retain its existing
aliases.

## Examples

### Margin

```tsx
// Before
<Box mb="4">Results</Box>

// After
<Box marginBottom="4">Results</Box>
```

### Width

```tsx
// Before
<Button w={{ base: "full", md: "auto" }}>Save</Button>

// After
<Button width={{ base: "full", md: "auto" }}>Save</Button>
```

Both rewrites retain the original value exactly, including responsive values.
They change only the prop spelling.

## Migration plan

1. **Filter exact aliases in the Chakra system configuration.** Use the pinned
   Chakra utility configuration as the source of truth for every removed
   `shorthand` entry.
2. **Generate the resulting Chakra types.** Run the package's existing Chakra
   type-generation step before typechecking so removed aliases are rejected.
3. **Use search only for discovery.** Search all automotive-web applications
   and package stories/tests to estimate scope and find unsupported contexts;
   do not use blind text replacement.
4. **Run an AST codemod for direct JSX attributes.** Rename only attributes on
   known Chakra-backed components or components whose frozen contract contains
   the alias. Preserve the value expression unchanged.
5. **Handle object properties and spreads separately.** Transform a static
   object key only when that object is demonstrably passed to an in-scope
   component. Leave computed keys, unknown spreads, and dynamically forwarded
   props in a review report.
6. **Detect conflicts rather than choosing precedence.** If an element already
   has both `mb` and `marginBottom` (or any mapped pair), do not rewrite it;
   flag it for a manual decision.
7. **Validate the vocabulary migration.** Run the codemod in dry-run mode,
   review its report, typecheck each automotive-web application, and run
   relevant tests and visual checks. The configuration filter and generated
   types then prevent reintroduction of removed aliases.

This sequencing makes the type change the enforcement mechanism, not a way to
discover the migration through broken builds.
