# Target package structure — packages as contracts

> Where the structure discussion landed: use **package boundaries** as the hard signal
> for "what API to expect," instead of labels/folders inside one package. Evolves the
> earlier "internal-only folders, keep root export" decision (that was optimised for
> minimal breakage; in the monorepo the import-path change is a codemod, so a clean cut
> is now cheap).

---

## The core insight

A **package boundary is a far harder signal than any in-package label.** The problem we
kept hitting — a bespoke `Button` hiding among Chakra-shaped components, with no label
reliably telling them apart — dissolves when the *import source* is the contract:

| Import from… | Contract |
|---|---|
| the primitives package | It's Chakra. Chakra props apply. Use Chakra docs. Not here → not a Chakra component. |
| the custom-components package | It's ours. Custom semantic API. **Expect no Chakra props.** |

## It dissolves the conform-vs-exempt dilemma

We were stuck on: "must we force `Button` into a Chakra shape, or grandfather it?"
Packages remove the question. **`Button` doesn't need to conform to a Chakra shape,
because it isn't in the Chakra package.** It lives in the custom package, where "bespoke
semantic API" *is* the expected contract. No exemption — because there's no Chakra-shape
expectation to be exempt from.

## Two axes, one boundary can only follow one

- **API contract** — Chakra-shaped vs custom-semantic. (Great for "what to expect".)
- **Role / change-cadence** — foundation vs generic design-system vs domain feature.
  (Great for ownership/governance.)

A two-package cut (Chakra vs custom) follows *contract* — but then lumps generic building
blocks (`Button`, `Carousel`) with domain features (`tenantSelection`, `topVehicle`) that
share a contract but differ in role and change fast vs slow. It moves the junk drawer, it
doesn't remove it.

---

## Proposed target: three packages

Reconstitutes the three categories **as real packages** — each boundary a hard contract
signal, and generic-vs-domain preserved:

| Package | Contract | Examples |
|---|---|---|
| **`@smg/primitives`** | thin Chakra; **props apply** | Box, Flex, Stack, Badge, Table |
| **`@smg/components`** | custom, **generic**, design-system-grade; **no Chakra props** | Button, Input, Carousel, Dialog |
| **`@smg/features`** ⚠️ *(not final — see below)* | custom, **domain-bound**, may take data; no Chakra props | tenantSelection, topVehicle, VehicleReference |

Reads cleanly for all three: primitives = Chakra; components = custom generic; features =
custom domain. `Button` → components. `tenantSelection` → features. Maps onto the
monorepo's shared-vs-project story: `features` holds *cross-project* domain components;
single-project ones stay project-local.

### Two-package fallback

`primitives` + `components`, with generic-vs-domain handled by **folders** inside
`components` (+ per-folder lint/ownership). Saves one boundary, but the domain/generic
signal goes soft again.

**Decision on two vs three: deferred (not important right now).**

---

## Open decision — the `@smg/features` package ⚠️

**Not yet sold.** Whether domain/feature components deserve their *own package* (vs living
project-local, or as a folder in `components`, or in domain-scoped shared packages) needs a
final decision. **Kept here for inspiration, not committed.**

Relevant context for that decision:
- In the monorepo, sharing is lightweight — a shared *package* may be heavier than needed
  for domain components that only 2 projects use.
- Domain features change fast and are product-owned; generic components change carefully and
  are design-system-owned. That cadence/ownership split is the main argument *for* a
  separate boundary.
- Alternative homes: project-local (`feature/` convention already exists), domain-scoped
  vertical-slice packages (`listing`, `vehicle`), or a single shared-features package.

## Breaking-change note

Separate packages = new import paths, i.e. more breaking than "internal folders + root
export." **In the monorepo this is a rule-based codemod (search/replace), so it's not a
concern** — clean cut, no back-compat facade needed. (A facade `@smg/components` re-export
remains an option if ever wanted, but isn't planned.)

## Naming notes

- `primitives` slightly undersells a package that also holds branded thin components
  (`Badge`, `Alert`) — but the *signal* ("Chakra applies here") is what matters.
- Avoid "shared-*" names: *everything* is shared across projects, so "shared" isn't the
  distinguishing property; let the role name it (`components` / `features`).

---

## Relationship to earlier decisions

- **Supersedes** the "internal-only reorg, keep root export" mechanism — now packages, clean
  cut (cost removed by the monorepo codemod reality).
- **Keeps** everything else: the export-boundary principle (we still own the boundary, just
  as N packages), idiomatic internals (recipes + lint), the prop-sourcing rule + shared
  vocabulary (applies within `components`/`features`), primitives transparent.
- **Simplifies** the archetype axis: the package boundary now carries most of the "what to
  expect" signal that archetypes were straining to provide inside one package. Archetypes
  still help *within* a package but are no longer load-bearing for discovery.
