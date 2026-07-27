# Idiomatic Chakra vs. wrapper components — and where we fit

> Context for the structure RFC: how teams customize Chakra components, why they
> wrap, and how our three-category approach relates to the ecosystem norm. This
> informs the *mechanism* behind our design-system rules — not the principle.

---

## 1. How Chakra intends customization to work

Chakra v3's customization story is the **theme/recipe system**, not wrapping:

1. Define a **system** (`createSystem` + `defineConfig`) with tokens, semantic tokens,
   and **recipes**.
2. Every built-in component (Button, Badge, Input…) is backed by a **recipe key**.
   Register a recipe under that key — `theme.recipes.button` — and **Chakra's own
   `<Button>` renders with your branding**, with `variant`/`size` values
   **type-generated** (`chakra typegen`) so `variant="primary"` is type-checked.
3. So the idiomatic "branded button" is: **override the `button` recipe, keep importing
   `Button` from Chakra, keep its full API.** No duplicate component. You constrain
   *appearance* via recipes/variants; you don't remove props.

**Philosophy:** Chakra treats the **open style-prop API as a feature**. Its answer to
"how do I stop `bg="pink.500"`?" is essentially *you don't — you brand via recipes, and
if you must restrict, you lint*. Chakra ships no prop-restriction mechanism because it
doesn't believe in one.

## 2. Why teams wrap anyway (legitimate reasons)

A wrapper earns its keep when recipes can't deliver:

- **Behavior** — loading spinner, icon slots, Next.js `Link` integration, analytics.
- **Defaults** — force a default variant/size.
- **Semantic props** — `fullWidth`, domain props.
- **Hard restriction** — make `bg` a *type error*, not just a lint warning.
- **Insulation** — a stable internal API so a Chakra major doesn't ripple to every
  consumer.

Rule of thumb in the ecosystem: **pure-visual branding → recipe only, no wrapper;
behavior/semantics → wrapper.**

## 3. The two axes every team implicitly picks

| Axis | Idiomatic-Chakra end | Hard end |
|---|---|---|
| **Branding** — how it gets its look | Register recipe on the system → base component branded for free | Apply recipe manually inside a wrapper |
| **Restriction** — how misuse is prevented | **Lint** flags `bg` / wrong imports; API stays open | Wrapper **removes props from the type**; API is closed |

- Chakra's default: **register + open + lint** (soft).
- A maximally-protected design system: **manual + closed wrapper** (hard).

---

## 4. Where our package actually sits (and the internal inconsistency)

Evidence from the theme wiring:

- The system `recipes` map (`src/themes/shared/recipes/index.ts`) registers:
  `avatar, badge, chip, link, rating, separator, skeleton, spinner, textarea, count,
  closeButton, radioListItem`.
- It does **not** register `button`. Instead `buttonRecipe`
  (`src/themes/shared/recipes/button.ts`) is imported **directly into the Button
  wrapper** (`src/components/button/index.tsx`) and applied by hand via `css={styles}`.

So the package **already mixes both models**:

| Component(s) | Model | Consequence |
|---|---|---|
| Badge, Chip, Link, Avatar, … | **Idiomatic** — recipe registered on the system | Chakra's base component would render branded |
| **Button** | **Hard wrapper** — recipe not registered; applied manually | Bare Chakra Button is deliberately *unbranded*; everyone is forced through the SMG wrapper's closed API |

This inconsistency is a key reason **we don't have to adhere to the current starting
point** — the starting point isn't even internally uniform.

Note: our own working notes already lean idiomatic on the restriction axis —
*"If we want to restrict props, we use a linter over omitting props."*

---

## 5. How our approach fits — principle vs. mechanism

Our locked design-system rule (no raw style props, enforced by a curated wrapper) sits
at the **hard end of both axes**. But principle and mechanism are **separable**:

- **Principle — keep it.** "Design-system components don't accept raw style props; the
  recipe owns the look." Sound regardless of mechanism.
- **Mechanism — reconsider toward idiomatic.** How we *brand* and how we *enforce*.

**Proposed hybrid (principle kept, mechanism idiomatic):**

> **Brand via a registered recipe** (appearance consistent even on the base component).
> **Wrap only when the component needs behavior / semantic props / integration**
> (e.g. Button, Input) — that's where curation naturally happens. **Enforce "no style
> props" by lint**, not by removing props from types — which also kills the whitelisting
> treadmill (the Textarea problem).

### Trade-offs of moving idiomatic

| | Idiomatic (register + lint, wrap for behavior) | Hard wrapper (manual recipe + closed type) |
|---|---|---|
| Wrapper code | Only where behavior is needed | Every component |
| Chakra docs apply | Yes | No (custom API) |
| Version upgrades | Trivial | Ripple through wrappers |
| Whitelisting friction | None (open API) | High (re-expose native props by hand) |
| Enforcement strength | Lint-time (escapable via `eslint-disable`) | Compile-time (airtight) |
| Consumer sees Chakra surface | More | Less |
| Bypass risk | Must also lint-restrict direct Chakra imports | Low |

### Working recommendation

**Keep the principle, move the mechanism toward Chakra-idiomatic:** register recipes for
everything, wrap only for behavior, lint for restriction. Less code, more upgrade-safe,
resolves the internal inconsistency, and matches what the notes already gestured at.

---

## 6. Open decisions this raises

- **D-A — Branding rule:** should every design-system component be branded via a
  **registered recipe** (so the base component is branded), making wrappers optional?
- **D-B — Enforcement mechanism:** restrict raw style props via **lint** (soft, open API)
  or **type-level removal in a wrapper** (hard, closed API)? (Crux of hard-vs-soft.)
- **D-C — Wrapper criteria:** wrap **only** when a component needs behavior / semantic
  props / integration; otherwise use the branded base component directly?
- **D-D — Bypass guard:** if the API stays open, we must **lint-restrict direct Chakra
  imports** of components that have a branded recipe, so branding isn't skipped.

These revisit the *enforcement* half of rules R2/R3 (and add a branding rule); the
*principle* (no raw style props, recipe owns the look) stands.
