# Design tensions — the reasoning that led to the solution

> The intellectual through-line behind the rules. Each tension was a genuine friction
> we hit; each resolution reframed the design. Captured so the *why* survives and we
> don't relitigate. Read this before challenging a rule — the rule is the residue of
> one of these.

---

## Tension 0 — Category confusion (the starting problem)

**The friction:** The same primitive existed in three layers (Chakra → thin re-export →
compat adapter), primitives followed contradictory rules (`Box` transparent, `Flex`
`Omit`, `Stack` `Pick`), and "design-system" components renamed/whitelisted props
arbitrarily. A component *looked* like Chakra but wasn't, accepted some Chakra props but
not others — guaranteeing workarounds.

**Resolution:** A three-category model — **primitives / design-system / features** — each
with a fixed contract, so a component's rules are predictable from its category.

**But:** the category turned out to govern *rules*, not everything (see Tension 5–6).

---

## Tension 1 — Is the wrapper a valuable guardrail, or the disease?

**The friction:** We wrap components (and forbid Chakra's). Is that protection, or the
cause of the inconsistency?

**The insight:** "Wrapper" was doing two jobs:
- **Wrapper-as-API-transformation** (`Omit`/`Pick`/rename/adapters) — the **drift surface**;
  the origin of every inconsistency. *Not a guardrail — the disease.*
- **Wrapper-as-export-boundary** (thin re-export / behavior wrapper) — cheap, stable; buys
  a single import source, dual-instance safety, a seam. *Valuable.*

**Resolution:** The real guardrails were never the wrapper — they are the **recipe**
(branding), the **lint** (restriction), and the **export boundary**. Keep the boundary;
drop the API-transformation.

---

## Tension 2 — Idiomatic Chakra vs. our wrapper approach

**The friction:** Chakra's own model is "brand via recipes, keep the open API, restrict via
lint" — no wrappers. Should we abandon wrapping and let apps import from Chakra directly?

**The insight:** Going fully idiomatic (apps import from Chakra) is the **maximum-breakage**
option — it rewrites every component import in every app, forces Chakra to a peer dep, makes
branding invisible — for benefits (recipe branding, no whitelisting, easy upgrades, typed
variants) we can get **while keeping our boundary.** Pure cost, no marginal benefit.

**Resolution:** **Keep the export boundary; go idiomatic *behind* it.** Idiomatic internals,
stable external boundary. (Recorded as a settled decision.)

---

## Tension 3 — If the recipe brands it, is the wrapper even needed?

**The friction:** For a thin visual component, a registered recipe alone renders identically
to our thin re-export. So `import { Badge } from '@chakra-ui/react'` (with our system) ==
our `Badge`. Is the design-system layer a *component library* or just a *theme*?

**The insight:** For visual components the recipe *is* the product; the wrapper adds nothing
to rendering. Its value is non-visual: single import source, dual-instance safety, a seam
for future behavior. Becoming "just a theme" would require peer-deps + direct Chakra imports
+ invisible branding — the Tension-2 costs again.

**Resolution:** Stay a **thin component library behind the boundary.** The wrapper is a
packaging/safety choice, not a rendering one — and a cheap one worth keeping.

---

## Tension 4 — Thin, curated, and custom components are indistinguishable

**The friction:** `Badge` (thin, Chakra-shaped), `Dialog` (closed, flattened semantic), and
`Button` (bespoke union) are imported from the same place, look like peers, but expose
completely different APIs. A consumer/agent can't tell which contract to expect. And
sometimes something *really* custom hides in there.

**The insight:** The **category governs rules but doesn't signal API shape.** The
design-system category spans several *archetypes* — that's why they feel indistinguishable.

**Resolution:** Add a second, orthogonal axis — **API archetype** (transparent /
branded-element / compound / curated) — declared per component and visible in the type
shape. Category = rules; archetype = contract.

---

## Tension 5 — Categorization can't do discovery

**The friction:** Even with clean categories, how does a human/agent know *which* component
to use and *why* — e.g. why Chakra's documented `Button` isn't available and ours is?

**The insight:** Categorization is a **producer/governance** taxonomy; discovery is a
**consumer/intent** question ("I need a button → what do I import?"). The tier is invisible
at the moment of reach, so it can't intercept a wrong reach. Different layer, different tool.

**Resolution:** Discovery is its own pillar. In practice it's largely already handled by
**discouraging direct Chakra imports** (the agent is pushed to the package), with a
**rationale-carrying lint message** and an **intent/Chakra-equivalent-keyed manifest** for
the rest. Don't make the categories carry discovery.

---

## Tension 6 — "Make the APIs consistent" is partly impossible

**The friction:** We kept circling: even with categories, archetypes, and wrappers, the APIs
are still very different — Chakra internals leak in some, behavior wrappers have a totally
different shape than the rest. It felt unsolvable because we kept restating it.

**The insight (the unlock):** Part of it *is* unsolvable as stated — you **cannot** make
`Dialog`'s API look like `Badge`'s; a dismissible overlay and a colored label *should*
differ. Split every API in two:
- **Essential difference** — what the component uniquely does. Legitimately varies; leave it.
- **Incidental difference** — how it expresses shared concepts (disabled, loading, size,
  variant, icon, value, events). This is the real enemy, and it's fully fixable.

**Consistency ≠ sameness.** Goal: no surprises in the *incidental* parts. Unify the
**vocabulary and sourcing**, not the APIs; let essential differences stand — now legible.

**Resolution:** A **shared prop vocabulary** (one canonical name/type per shared concept,
shipped as reusable TS mixins) that every component reuses. Divergence is confined to the
essential.

---

## Tension 7 — The wrappers were built to stop leaks, but leaked anyway

**The friction:** We adopted wrappers *because* Chakra internals were leaking into projects.
Yet the wrappers `extends ChakraXProps` / spread Chakra prop types — so they leaked anyway,
and each spread a *different* Chakra type, producing different shapes.

**The insight:** The leak *was* sourcing props from Chakra's prop types. Remove that source
and there's nothing to leak — and every component that instead composes from one shared
vocabulary ends up the *same* shape for shared concepts.

**Resolution (the actionable rule):** **Prop sourcing** — a design-system component's public
props come from **native element attributes + shared SMG vocabulary + its own semantic
props; never from Chakra's prop types.** This single rule kills the leak *and* the
whitelist treadmill *and* constrains divergent shapes.

---

## The through-line

Category confusion → three categories (**rules**) → separate wrapper-the-disease from
wrapper-the-boundary → keep the boundary, idiomatic internals → the wrapper is packaging,
not rendering → but thin/curated/custom look alike → add **archetype** (**contract**) →
categories can't do discovery → discovery is its own pillar → "consistent APIs" is partly
impossible → **consistency ≠ sameness**: unify **conventions**, not APIs → and the leaks
came from **sourcing props from Chakra**, so source from native + vocabulary instead.

The solution is the residue: **a stable export boundary, idiomatic internals, categories for
rules, archetypes for contracts, and a shared vocabulary + prop-sourcing rule so components
are predictably different rather than uniform.**
