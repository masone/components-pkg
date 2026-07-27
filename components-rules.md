# Category rules of engagement

> The contract each category must obey, with the reasoning behind each rule. Agreed
> at the *what/why* level; implementation details (lint specifics, codemods, file
> moves) are deliberately out of scope here.

The three categories exist so that a component's contract is **predictable** — a
developer (or agent) knows the rules from the category alone. Import paths don't change
(everything still ships from the root); the **TypeScript API shape** is the signal to
consumers, the **on-disk folder + per-folder lint** is the signal to maintainers.

---

## ① Primitives

> "How do I compose layout?" — the Chakra layer, unchanged.

| Rule | Why |
|---|---|
| **Transparent re-export only** — public API *is* Chakra's; no `Pick`/`Omit`/wrapping | Predictability: "it's Chakra `Box`, use Chakra docs." Preserves style props, responsive props, tokens, and TS support — the whole value of Chakra |
| **Adds nothing** — no default styles, no behavior | If it needs either, it isn't a primitive. Keeps the layer thin and honest |
| **Barrel may only re-export from `@chakra-ui/react`** (lint-enforced) | This is exactly how `Flex`/`Stack` drifted into whitelists; the lint makes drift impossible |
| **Raw values allowed** — no type-level token enforcement | Transparency means you can't block `padding="13px"` without breaking the contract. Token discipline is convention/lint, and strict-tokens is backlog |

**Set:** `box, flex, stack, grid, simpleGrid, center, aspectRatio` (layout).
**Note:** `text` and `heading` are **not** primitives — they carry brand typography
identity and live in ② (see decision below).

---

## ② Design-system

> "How do I behave?" — branded, generic components. The look is owned by a recipe.

**The core principle (style props):**

> A design-system component owns its **appearance** (recipe) and its **content** — not
> its **external layout**. It accepts variants, behavior/semantic props, and native HTML
> attributes. It accepts **no Chakra style props**.
> - Appearance → recipe / variant
> - External layout → wrap it in a primitive
> - Unavoidable self-sizing → a named semantic prop (e.g. `fullWidth`)

| Rule | Why |
|---|---|
| **No Chakra style props** (appearance *or* layout) | Appearance props (`bg`, `color`, `boxShadow`, …) leak the design system — the exact thing we're killing. Layout props (`margin`, `alignSelf`, …) belong to the parent, handled by wrapping. One way to style, one way to lay out |
| **Native HTML attributes always allowed** — `aria-*`, `data-*`, `id`, `name`, `type`, `htmlFor`, event handlers | These are the element contract, not styling. "No style props" must not be misread as "no `aria-label`" |
| **Look lives in a recipe** (`defineRecipe`/`defineSlotRecipe`) | Type-safe, typegen-supported, single source of truth; variants prevent impossible combinations |
| **Generic — no domain data** | Takes a `listing`/`vehicle`? It's a feature (③). Guarantees reusability |
| **Escape hatch = wrap in a primitive** — no `css`/`className` on the component | Keeps the component surface pure and pushes styling to the layout layer, where it belongs. Avoids re-leaking styling through a side door |
| **Self-sizing → named semantic prop** — added only when a real recurring need appears (not pre-invented) | `fullWidth` is a *named intent* the component implements internally, not a raw style prop. Covers the one case wrapping can't (full-width) |
| **`as` = semantic correction only**; forwards refs; spreads valid remaining props | Composability without behavioral abuse (no `Button as="div"`) |
| **Partial Chakra-compat, stated honestly** — same `variant`/`size`/`disabled`/`loading` vocabulary, but *not* drop-in Chakra | A predictable curated surface beats full compatibility. We say so plainly so nobody expects `bg` to work |

**The trade-off, acknowledged:** more wrapping boilerplate for spacing, and a small set
of semantic props grows over time. **The win:** exactly one way to do each thing, and
the surface can never drift back into a leak.

**Includes:** `text`, `heading` (brand typography) plus the form / feedback / overlay /
structure-nav groups (see classification).

---

## ③ Features

> "What product problem do I solve?" — domain components; Chakra is an implementation detail.

| Rule | Why |
|---|---|
| **Domain / semantic props only** — no Chakra props, no style/layout escape hatches | Express product semantics; stop ad-hoc restyling of domain UI |
| **Chakra is internal** — composed from primitives + design-system components; no Chakra-compat expectation | Encapsulation |
| **May depend on a data object** (a listing, a vehicle) | That's what makes it a feature rather than a design-system component |
| **May expose semantic variants/sizes**, never raw style props | Legitimate flexibility without leaking Chakra |
| **No new brand styling invented inline** — reuse recipes / design-system | Consistency |
| **Inclusion bar: flag case-by-case, move later** — shared across apps / no hard app-coupling = keep; app-specific (e.g. `tenantSelection`, `vehicleReference`) = candidate to move to the app | Answers "is it part of the project or part of components?" without forcing migration now — matches the low-breaking philosophy |

---

## Cross-cutting (all categories)

| Rule | Why |
|---|---|
| **Tokens over raw values** (strict enforcement = backlog) | Consistency over flexibility; strict lint needs a token audit first |
| **`as` = semantic correction only**, never behavioral replacement | Avoids `Button as="div"`, `Link as="button"` accessibility breakage |
| **HTML is fair game** when Chakra isn't needed | `<span>` over `<Box as="span">` — no need to pay for Chakra when plain HTML does the job |
| **CSS variables discouraged** — prefer tokens; reserve for genuinely dynamic runtime values | Tokens are the design-system currency; CSS vars are an escape, not a default |
| **Nobody imports branded Chakra components directly** (lint) | Bypassing the package skips our recipe/branding |

---

## Decisions locked (this pass)

| # | Decision |
|---|---|
| R1 | Primitives: transparent re-export, adds nothing, barrel re-export-only (lint), raw values allowed |
| R2 | Design-system: **no Chakra style props** (appearance or layout); native HTML attributes stay |
| R3 | Design-system escape hatch: **wrap in a primitive** — no `css`/`className` on the component |
| R4 | Design-system self-sizing: **named semantic prop** (e.g. `fullWidth`), added on real need only |
| R5 | Typography: **`text` + `heading` are design-system (②)**, not primitives |
| R6 | Features: domain-only API, Chakra internal; **inclusion bar = flag case-by-case, move later** |
| R7 | Design-system is **partial Chakra-compat, stated honestly** (not drop-in) |
