# Going idiomatic — effort & breakage assessment

> What adopting the idiomatic-Chakra mechanism would actually mean for our components:
> which are affected, in what way, and whether it's a rewrite / breaking-change turn.
> Evidence-based: a scan of all design-system component `index` files (LOC, Chakra
> import, `Omit`/`Pick` presence, recipe-hook usage, `forwardRef`) plus reads of
> representative components (badge, card, input, button, textarea).

---

## Headline

**Going idiomatic is mostly a *subtraction* exercise — remove wrappers and blocklists,
register recipes — not a rewrite, and mostly non-breaking.** ~32 of 35 design-system
components need only thin/additive changes. The two genuinely heavy wrappers (Input,
Button) are heavy because of *behavior*, which idiomatic **preserves**.

---

## Primitives

Confirmed direction: **stop wrapping (transparent re-export); only high-level
discouragement lints** — e.g. "don't import branded Chakra components directly," "prefer
our layout conventions over raw `Stack`." **No per-prop linting** on primitives.
Low-effort, fully idiomatic.

---

## Design-system — tiered by the action idiomatic requires

35 components scanned. Sorted by what "going idiomatic" demands:

| Tier | What it is | Action to go idiomatic | Breaking? | Members (~count) |
|---|---|---|---|---|
| **1 — Already there** | Thin wrapper / plain re-export, recipe registered | Nothing, or drop tiny sugar; style discipline via lint | **No (additive)** | badge, spinner, skeleton, closeButton, count, avatar, separator, link, linkOverlay, list, card, progress, collapse, rating (**~14**) |
| **2 — Compound** | Slot-recipe multipart components | Register slot recipe (mostly done) + thin to Chakra compound + replace `Omit` blocklists with lint | **Mostly additive** | dialog, drawer, popover, hoverCard, menu, accordion, breadcrumbs, tab, table, pagination, field, checkbox, radio, switch, select, chip, alert, tooltip (**~18**) |
| **3 — Behavior wrappers** | Heavy for legit reasons (debounce, clear, addons, loading, link, icon) | **Keep the wrapper**; register recipe globally; `Omit`/manual-strip → lint | Breaking bits already scheduled | button, input (**~2**) |
| **4 — Pure whitelist** | Wrapper that adds nothing | Delete the wrapper | **No (additive)** | textarea (**~1**) |

### Scan signals behind the tiering

- **`Omit`/`Pick` present** (blocklist anti-pattern → replace with lint): `alert, button,
  select, switch, drawer, tooltip, pagination, tab, collapse` (9).
- **Heavy (>100 LOC)**: `input (205), button (166), dialog (126), menu (118), alert (103)`.
- **Consumes a recipe via hook** (manual application): `chip, alert, count, closeButton,
  rating, avatar, button, input, checkbox, radio, select, field, dialog, menu, accordion,
  breadcrumbs, pagination, tab` (18).
- **Very thin (<30 LOC)**: `badge, count, closeButton, spinner, skeleton, avatar, link,
  linkOverlay, separator, list, collapse` (11).

### Representative reads

- **Badge (13 LOC)** — thin wrapper over Chakra `Badge`, adds a `text` prop, spreads the
  rest. Recipe registered → branding idiomatic. Only non-idiomatic bit: `text` sugar +
  leaked style props.
- **Card (31 LOC)** — essentially a re-export of Chakra's `Card` compound
  (Root/Header/Body/Footer) with displayNames + deprecated flat aliases. Already
  idiomatic.
- **Input (205 LOC)** — heavy because of **behavior**: `use-debounce`, clear button,
  icon/addon slots, controlled/debounced/readonly union; branded via registered
  `inputSlotRecipe`. Idiomatic **keeps** this wrapper; only tightens the style-prop leak.
- **Button (166 LOC)** — hard wrapper: recipe **not** registered globally, applied
  manually via `css`; curated closed union; `Omit` blocklist. The main Tier-3 work.
- **Textarea (34 LOC)** — pure whitelist, no behavior. Tier-4 delete.

---

## Tier 2 deep-dive — compound components (dialog, drawer, popover, hoverCard, menu, …)

### Branding: already idiomatic (done)

Slot recipes are **registered for essentially every compound component**: `dialog, drawer,
popover, hoverCard, menu, accordion, tabs, table, tooltip, pagination, field, checkbox,
radio, select, alert, slider, list, card, …` (the `src/themes/shared/slotRecipes` map). And
they're applied. The branding mechanism — the part that would be real work — **already
exists**. For compound components you are not building recipe infrastructure.

### API shape: three sub-patterns coexist (an inconsistency finding in itself)

| Pattern | Examples | Shape |
|---|---|---|
| **Full namespace re-export** | `table`, `card` | `Table.Root/Header/Body` — thin, closest to raw Chakra compound |
| **Partial slot exposure** | `drawer` | flattened `<Drawer>` root **+** exports `DrawerOverlay/Content/Body` |
| **Fully flattened opinionated wrapper** | `dialog`, `popover`, `hoverCard`, `menu`, `tooltip`, `pagination`, `select` | one component with a curated prop API; composes the whole Chakra compound internally |

**Archetype — Dialog (126 LOC):** public API is `title`, `primaryActionButton`,
`secondaryActionButton`, `disableBodyPadding`, `size`, `variant`, `open`/`onOpenChange`. It
hardcodes the entire structure internally (title→`H3`, close button, up-to-2 footer buttons)
and applies the slot recipe manually via `useSlotRecipe({key:'dialog'})` + `css={styles.x}`
per slot. Header/footer are not customizable — you get exactly what it offers.

### Are the flattened ones "not idiomatic"? No.

The flattened API is a **legitimate curated *behavior* wrapper — exactly what the export-
boundary decision says to keep.** Branding is idiomatic (registered slot recipe); the
flattened shape is a deliberate DX choice, not a defect. Dialog/Popover/Menu don't need to
be "made idiomatic."

### Work to do — the low-risk part

- **Standard cleanup (additive, low risk):** remove `Omit`/`Pick` blocklists (`drawer, tab,
  tooltip, pagination, select, alert, switch, collapse`) → lint. Retire adapter dialects.
- **Optional internal tidy:** several apply the slot recipe *manually* even though the
  registered recipe may auto-apply on Chakra's own slots. Simplifying is an internal, low-
  risk refactor with no public-API impact — verify per component.

### The risk to avoid

Converting the flattened APIs into Chakra's compound namespace (`Dialog.Root/Trigger/
Content`) for flexibility would be a **large breaking change across every consumer**, throw
away the curated DX, and contradict the export-boundary decision. **Do not do it here.**

### The one genuine deferred risk

The three coexisting patterns make the compound layer **internally inconsistent** (Dialog
fully flattened, Drawer half-compound, Table full namespace). Harmonizing it is **per-
component breaking API work** → backlog, coordinated major. Not part of the structure RFC.

### Net verdict

**Compound components are the low-risk tier.** Branding is done, no restructuring needed,
cleanup is additive. Risk only appears if we opt into API harmonization / compound-
namespace conversion — which is deferred, not part of going idiomatic.

---

## Answering the two questions

### Are we on a good track, or a full rewrite?

**Firmly on a good track.** ~32/35 need only thin/additive changes. The heavy wrappers
(Input, Button) are heavy for behavior idiomatic *preserves*. **Not a full rewrite** — it's
registering a few recipes, deleting blocklists, and thinning two wrappers.

### Is it a breaking-change turn?

**Mostly not.** The core idiomatic moves are additive:

- **Removing an `Omit`/`Pick`/whitelist widens accepted props → non-breaking** (Tiers 1, 2, 4).
- **Registering recipes globally** changes internal wiring, not the public API → non-breaking.

The breaking surface is small and **already in the "coordinated major" bucket**:

- custom sugar removals (`Badge.text` → children, `Button`'s curated union),
- the adapter prop-name dialect (`isDisabled`, `isLoading`, `spacing`, …).

So idiomatic **doesn't add breaking surface — it shrinks it**, because we carry *less*
custom API that would otherwise have to be broken later.

---

## The three mechanical changes (the whole story)

1. **Register every design-system recipe on the system** — fixes the Button-not-registered
   gap; uniform branding. *(non-breaking)*
2. **Replace `Omit`/`Pick` blocklists + manual style-prop stripping with lint** (~9 `Omit`
   users + Input/Button). *(additive)*
3. **Thin the two behavior wrappers to behavior-only** and **delete pure-whitelist
   wrappers** (Textarea). *(breaking parts already scheduled for the major)*

---

## Follow-ups (optional)

- Sanity-check the Tier-2 `Omit` users (`alert, select, switch, drawer, tooltip,
  pagination, tab, collapse`) to confirm each `Omit` is a style-prop blocklist and not a
  legitimate type refinement.
- Confirm which design-system recipes are **not** yet registered on the system (Button is
  the known one) — that list is the exact scope of mechanical change #1.
