# Strict Chakra tokens

**Status:** Exploring — discussion proposal, no decision yet.

## Question

Should components-pkg enable Chakra UI's `strictTokens` option so that Chakra
style props use design-system tokens by default?

This proposal concerns the AutoScout24 and MotoScout24 design-system themes.
It deliberately excludes HCI, which has its own configuration and runtime
token requirements.

## What this would mean

Chakra's `strictTokens` option is a TypeScript guardrail. Once enabled and its
theme types are generated, a Chakra style prop must use a known token instead
of an arbitrary CSS value.

```tsx
// Token usage
<Box marginTop="xl" width="full" />

// Raw values that strict mode would surface for review
<Box marginTop="13px" width="320px" />
```

This does **not** mean that every number in JSX is prohibited. Native HTML and
SVG attributes, inline `style` objects, CSS files, dynamic strings, and type
assertions are outside this check. Chakra also deliberately permits an explicit
escape hatch such as `width="[320px]"`. The proposal is therefore about a
token-first default and visible exceptions, not an absolute CSS ban.

## Current state

The shared theme already defines design tokens for spacing, sizes, radii,
colours, typography, shadows, and related design decisions. It currently sets
`strictTokens: false`, so raw Chakra values remain type-valid. The package also
already runs Chakra type generation as part of its build.

The current spacing scale contains named values from `xxs` through `7xl`, plus
`0`, `auto`, and a small number of deliberate special values. The size scale
contains named dimensions such as `full`, `half`, `fit`, and the container
sizes. A strict policy would make these names the normal public vocabulary.

## Evidence from automotive-web

The initial broad text scan was not used as a migration estimate: it also
matched token names such as `"2xl"` and non-Chakra attributes. A corrected,
exact-value scan limited to margin, padding, and gap props gives a more useful
starting point.

| Exact raw spacing values | Occurrences | Source files |
| --- | ---: | ---: |
| All values, excluding HCI, stories, tests, and theme definitions | 97 | 41 |
| `0` | 45 | — |
| `8` | 30 | — |
| Other values, including `6px`, `-12px`, and `40` | 22 | — |

The design-system package accounts for 7 of the 97 occurrences across 5 files.
The rest is concentrated in applications: seller-web has 71 occurrences,
listings-web 12, and auctions-web 7. Many `0` values are intentional resets;
the repeated `8` values are concentrated in one application file. These
figures are evidence that a spacing-focused migration is practical, not a
claim that every result is necessarily a Chakra type error.

Other categories need separate validation. Width, height, positioning, colour,
typography, and composite CSS values have legitimate use cases and are more
likely to require either an existing token, a reusable new token, or an
explicit escape hatch. Turning on strict mode in a branch and running typecheck
is the authoritative way to produce that backlog.

## Benefits and costs

### Benefits

- Makes design-system choices discoverable and consistent in Chakra props.
- Replaces silent, ad-hoc values with compiler feedback and autocomplete.
- Makes intentional exceptions explicit in code review.
- Reduces the chance that component consumers depend on arbitrary visual
  details that are difficult to change later.

### Costs and limitations

- Existing raw Chakra values become a migration backlog.
- The token set must be maintained deliberately; adding a token for every
  existing number would defeat the purpose.
- The check cannot police non-Chakra styling paths, and it has a documented
  escape hatch. A lint or review convention is needed for those paths.
- Chakra type generation must run whenever the theme changes, locally and in
  CI, so the enforced types remain accurate.

## Options

### A. Keep the current permissive configuration

Leave `strictTokens: false` and encourage token use through documentation and
review.

This has no migration cost and retains maximum flexibility, but it cannot
prevent new raw Chakra values from accumulating.

### B. Token-first convention, without compiler enforcement

Agree that new Chakra values should use tokens where available, document when
raw values are acceptable, and add linting or review guidance for new literals.

This is a low-risk transition path. It improves shared expectations but relies
on consistent review rather than TypeScript, and it does not automatically
surface existing values.

### C. Enable strict Chakra tokens in stages

Enable `strictTokens: true` for the shared design-system themes, regenerate
Chakra types, and use resulting type errors as the migration backlog. Start
with components-pkg and spacing; then move through application consumers and
other token categories. Permit Chakra's bracket syntax only for documented,
reviewed exceptions.

This gives the strongest lasting enforcement while keeping the migration
bounded. It requires an agreed escape-hatch policy and a clear process for
proposing genuinely reusable new tokens.

## Decisions needed

- Do we want compiler enforcement (Option C), a convention-only approach
  (Option B), or to retain the current flexibility (Option A)?
- If choosing Option C, should the first enforced scope be components-pkg only,
  or components-pkg plus all application consumers?
- Which exception syntax and review rule should apply to raw Chakra values?
- Who decides whether a recurring raw value is a reusable token, rather than a
  one-off exception?

## Proposed next step if the team chooses Option C

1. Enable strict mode in a temporary branch and regenerate Chakra types.
2. Run component and consumer typechecks to capture the real error list.
3. Resolve components-pkg spacing values first; use existing tokens whenever
   they express the design decision.
4. Categorise the remaining errors as token replacement, new reusable token,
   or explicit exception.
5. Add a small lint or review rule for bracket escape hatches and non-Chakra
   inline styling.

## Sources

- Chakra UI, [Theming overview](https://chakra-ui.com/docs/theming/overview)
- Chakra UI, [CLI and type generation](https://chakra-ui.com/docs/get-started/cli)
- automotive-web shared theme and static inventory, July 2026
