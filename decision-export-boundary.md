# Decision: keep the export boundary, go idiomatic behind it

> **Status:** Settled (internal architectural decision — *not* an open question to put to
> the team). Recorded for later reference so we don't relitigate it.
>
> **Decision:** Keep `@smg-automotive/components` as the **single import boundary**. Move
> the real work to **recipes + lint**. Wrap only for **behavior**. **Do not** push
> component imports to Chakra in the consuming apps.

---

## The question we were answering

Do we keep wrapping the way we do (is that a valuable guardrail), or opt for fully
idiomatic Chakra and import from Chakra in the applications — with all the refactoring
that follows?

## The reframe that resolved it

"Wrapper" was doing two different jobs; separating them dissolves the question:

- **Wrapper-as-API-transformation** — `Omit`/`Pick`/rename/curate/adapters. The **drift
  surface**; the origin of every inconsistency in this RFC. **Not a valuable guardrail —
  it's the disease.**
- **Wrapper-as-export-boundary** — a thin `export { Badge }`-with-our-recipe, or a
  behavior wrapper. Cheap, stable; buys a single import source + dual-instance safety + a
  seam. **Valuable and nearly free.**

**The real guardrails were never the wrapper** — they are the **recipe** (branding), the
**lint** (restriction), and the **export boundary** (single source / safety / seam). The
heavy API-transformation wrapper is the one guardrail that costs more than it protects.

---

## The three options and the verdict

### ❌ Keep wrapping as we do (heavy curated wrappers)
The way we wrap *is the problem statement of this RFC* — adapters, `Pick`/`Omit`, renamed
props, category confusion all live here. Keeping it keeps the drift.

### ❌ Fully idiomatic — import from Chakra in the apps
Maximum-breakage option for benefits obtainable without it:

- Every `import { Button } from '@smg/components'` in every app → `from '@chakra-ui/react'`
  — a **rewrite of every component import across all consumers.** The single most breaking
  change on the table; contradicts the "keep breaking changes low, don't refactor
  everything at once" philosophy.
- Forces **Chakra to a `peerDependency`**; pushes dual-instance risk onto every app.
- Branding becomes **invisible** (no signal a Chakra import is branded, or which variants
  exist).
- **Kicker:** recipe-based branding, no prop-whitelisting, easy upgrades, typed variants
  are **all obtainable while keeping our boundary.** Importing from Chakra in apps adds
  **no benefit** over "our boundary + idiomatic internals" — pure cost.

### ✅ Keep the boundary, idiomatic internals (chosen)
- **Consumers:** keep importing from `@smg-automotive/components`. **Zero app refactor.**
- **Internally:** register every recipe, replace `Omit`/`Pick` with lint, thin wrappers to
  re-export-or-behavior, retire adapters over time. Idiomatic — behind our boundary.
- **Chakra stays bundled** (no peer-dep migration); the "no direct Chakra imports" rule
  stays; dual-instance risk stays closed.
- **Guardrails that remain are the real ones:** recipe + lint + boundary.

---

## Trade-offs of the chosen approach (for the record)

| | Kept boundary + idiomatic internals (chosen) | Fully idiomatic (app imports from Chakra) |
|---|---|---|
| Consumer refactor | **None** — imports unchanged | **Rewrite every component import in every app** |
| Chakra dependency | Stays bundled | Must become a peer dep |
| Dual-instance risk | Closed (single source) | Live; managed per-app |
| Branding visibility | Explicit (our package) | Invisible (registered recipe magic) |
| Wrapper maintenance | Thin re-exports + behavior wrappers | ~zero (theme only) |
| Upgrade friction | Low (boundary absorbs Chakra changes) | Low, but coupling spread across apps |
| Import sources | One (`@smg/components`) | Two (Chakra for components, us for theme + behavior) |
| Future seam (add behavior later) | Preserved | Lost — re-adding a wrapper = migration |
| Fit with low-breakage philosophy | **Strong** | **Weak** |

**Cost we accept:** we maintain a thin re-export/behavior-wrapper layer instead of
shipping a pure theme. It's cheap, and it's what preserves the single import source, the
dual-instance safety, and the seam.

## What "idiomatic internals" concretely means

1. **Register every design-system recipe on the system** (fixes the Button-not-registered
   gap) — branding is uniform. *(non-breaking)*
2. **Restrict raw style props via lint**, not by removing props from types — kills the
   whitelisting treadmill. *(additive)*
3. **Wrap only for behavior** (Button, Input, Carousel…); everything else is a thin
   re-export behind our boundary.
4. **Retire adapters over time** (codemod → delete), batched into the coordinated major.

## Consequences

- The earlier locked decisions stand and are reinforced: transparent re-export, keep root
  exports, internal-only reorg, consumers guided by the TS API.
- This is **not** an RFC open question — it's a settled premise the RFC builds on.
