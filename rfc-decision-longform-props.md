# RFC decision: Use long-form style props

> **Status:** Proposed

## Decision

Use long-form Chakra style-prop names in the public component contracts and in
application code. We will not preserve both a shorthand and a long-form spelling
for the same CSS concept.

Long-form names are more readable in JSX, align with the existing migration
guide's `marginTop` / `marginLeft` convention, and make a component's frozen
interface easier to understand without Chakra alias knowledge.

The [automotive-web inventory](./automotive-web-inventory.md) shows that both
spellings are currently in use. That is migration evidence, not a reason to
keep duplicate public API. For example, `Box` currently has 98 `mb` uses and
41 `marginBottom` uses, while it has 74 `w` uses and 212 `width` uses.

## Why this matters

The RFC calls out a library where equivalent concepts are expressed through
unpredictable vocabulary—such as `bg` versus `background`—and where consumers
must inspect Chakra knowledge to understand the actual API. Keeping shorthand
and long-form aliases doubles that ambiguity in every frozen interface and in
Storybook.

One readable spelling per concept makes interfaces easier to scan, search, and
teach. It also turns the migration into a mechanical, reviewable change rather
than leaving each component to choose a local alias convention. This is a
consistency decision; it does not change the styling capability or value model.

## Canonical form

Use the descriptive CSS-oriented spelling:

| Use | Do not use |
| --- | --- |
| `width` | `w` |
| `height` | `h` |
| `marginTop` | `mt` |
| `marginBottom` | `mb` |
| `marginInline` | `mx` |
| `marginBlock` | `my` |
| `paddingInline` | `px` |
| `paddingBlock` | `py` |

The same rule applies to the remaining directional and min/max aliases. The
codemod mapping is the authoritative implementation list and must be verified
against Chakra v3's supported prop names before it is applied.

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
<Button w={{ base: 'full', md: 'auto' }}>Save</Button>

// After
<Button width={{ base: 'full', md: 'auto' }}>Save</Button>
```

Both rewrites retain the original value exactly, including responsive values.
They change only the prop spelling.

## Migration plan

1. **Build an alias map.** Record each shorthand-to-long-form mapping, the
   Chakra v3 prop it targets, and whether it is a direct rename. Start with
   `w`, `h`, `minW`, `maxW`, `minH`, `maxH`, `m`, `mt`, `mr`,
   `mb`, `ml`, `mx`, `my`, `p`, `pt`, `pr`, `pb`, `pl`, `px`,
   and `py`.
2. **Use search only for discovery.** Search all automotive-web applications
   and package stories/tests to estimate scope and find unsupported contexts;
   do not use blind text replacement.
3. **Run an AST codemod for direct JSX attributes.** Rename only attributes on
   known Chakra-backed components or components whose frozen contract contains
   the alias. Preserve the value expression unchanged.
4. **Handle object properties and spreads separately.** Transform a static
   object key only when that object is demonstrably passed to an in-scope
   component. Leave computed keys, unknown spreads, and dynamically forwarded
   props in a review report.
5. **Detect conflicts rather than choosing precedence.** If an element already
   has both `mb` and `marginBottom` (or any mapped pair), do not rewrite it;
   flag it for a manual decision.
6. **Validate before narrowing types.** Run the codemod in dry-run mode, review
   its report, typecheck each automotive-web application, and run relevant
   tests and visual checks. Only then remove the shorthand names from the
   component allow-lists and add a lint rule preventing new shorthand props.

This sequencing makes the type change a final enforcement step, not a way to
discover migrations through broken builds.
