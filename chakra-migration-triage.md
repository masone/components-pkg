# Chakra migration triage

## Purpose and scope

This is a consumer-migration triage for the non-adapter `src/components` surface. It answers a
narrow question: **if an export should move to a direct Chakra primitive/customization-layer
API, how much consumer work would that take?** It is not a mandate to turn every component
into Chakra. Domain features and honest generic components are explicitly kept out of that
forced choice.

The three migration buckets are:

1. **Direct Chakra primitive — no practical consumer cleanup:** implementation can become a
   direct re-export; existing callers keep their JSX and props.
2. **Easy codemod:** a small number of bespoke names/props or a shallow namespace change map
   mechanically to Chakra composition.
3. **Chakra-like but heavily customized:** it changes composition, state, or content shape;
   moving callers to raw Chakra is a real consumer refactor. This may be a valid generic
   component target instead of a migration candidate.

Adapters and infra (`themeProvider`, `translationProvider`, `icons`) are excluded.

## 1. Direct Chakra primitive — no practical consumer cleanup

These are source cleanups. The implementation should be a direct Chakra re-export, recipe
registration supplies any branding, and existing consumers do not need to change.

| Export | Evidence / action |
|---|---|
| **Box** | Already exact Chakra `BoxProps` re-export. Keep direct. |
| **Flex** | Only removes `gap`, `columnGap`, and `rowGap` with `Omit`. Re-exporting widens the API. |
| **Stack** (+ `StackSeparator`) | Only a `Pick` whitelist. Direct re-export widens the API; all current props remain valid. |
| **Grid** + **GridItem** | Existing aliases only assign Storybook display names. Direct namespace exports preserve consumer JSX. |
| **SimpleGrid** | Only a `Pick` whitelist. Direct re-export widens the API. |
| **Center** | Only a `Pick` whitelist. Direct re-export widens the API. |
| **AspectRatio** | Chakra alias plus display name only. |
| **CloseButton** | Current wrapper accepts only `disabled`/`onClick` and applies registered recipe styling. Direct re-export preserves those props and adds the normal Chakra surface. |
| **LinkBox / LinkOverlay** | Already exact Chakra re-exports. |
| **List** (`List.Root`, `List.Item`) | Existing namespace mirrors Chakra’s namespace; display-name aliases and deprecated flat exports are the only local layer. |
| **Table** (`Table.Root`, slots) | Existing namespace mirrors Chakra’s compound namespace; display-name aliases/deprecated flat exports are the only local layer. |
| **Skeleton** | Chakra alias plus display name only. |
| **Spinner** | Wrapper claims full `SpinnerProps` but currently drops every prop except `size`. Direct re-export is additive/corrective for callers. |
| **Separator** | Chakra alias plus display name only. |
| **Textarea** | A prop whitelist that adds no behaviour and forwards directly to Chakra `Textarea`. Direct re-export is additive for existing usages. |

### Important exclusions from this bucket

- **Text** is technically already a Chakra alias, but the agreed design direction treats
  branded typography as a generic component, not a primitive. Do not use technical thinness
  alone to override that product decision.
- A registered recipe is not a reason to retain a wrapper. For the entries above, it should
  brand the Chakra component through the configured system.

## 2. Easy codemod — small, mechanical consumer changes

These have a Chakra target and a bounded migration. They should receive a deprecation period
or codemod rather than an ad-hoc rewrite.

| Export | Current custom layer | Mechanical target |
|---|---|---|
| **Badge** | Only `text` sugar over `ChakraBadgeProps`. Note: `Exclude<BadgeProps, 'children'>` does not actually remove `children`; it is a type bug. | `<Badge text="New" />` → `<Badge>New</Badge>`. Recipe is already registered. |
| **Link** | `isExternal` sets target/rel; otherwise it is Chakra Link + recipe. | `isExternal` → explicit `target="_blank" rel="noopener noreferrer"`; direct Chakra Link. |
| **Card** | Chakra slots are re-exported under `CardComponents` and deprecated flat `Card`, `CardBody`, etc. | `CardComponents.Root`/flat aliases → `Card.Root`/`Card.Body`, etc. Preserve recipe registration. |
| **Heading** (`H1`–`H6`) | Fixed element/default `textStyle` around Chakra Heading. | `H1` → Chakra `Heading as="h1" textStyle="heading1"`, etc. This is a simple AST/tag import rewrite if the team chooses a primitive API. |
| **Collapse** | v2-shaped `in`, optional `animateOpacity`, and automatic `Collapsible.Content`. | `in` → `open`; wrap children in `Collapsible.Content`. `animateOpacity` becomes explicit Chakra animation styling. Structural, but uniform and codemoddable. |
| **ColorPicker** | Chakra Input with `type="color"`, fixed visual defaults, and `invalid`. | Chakra `Input type="color"`; put validation in `Field.Root`. Defaults become recipe/component-level decision. |
| **DatePicker** | Chakra Input with `type="date"`, `min: Date` conversion, and recipe styling. | Chakra `Input type="date"`; convert `Date` to the native `YYYY-MM-DD` string where used; compose Field only where needed. |
| **TimePicker** | Chakra Input with `type="time"`, Field wrapper, and recipe styling. | Chakra `Input type="time"` + explicit `Field.Root` where validation is needed. |
| **Select** | `options[]` array plus NativeSelect Root/Field/Indicator composition. | Expand `options` into `<option>` children and use `NativeSelect.Root/Field/Indicator`. Usually AST-codemoddable, but validate custom option rendering before bulk conversion. |
| **Tab / Tabs** | Chakra slots plus manual recipe-variant cloning and legacy naming. | `Tabs`/`TabList`/`Tab`/`TabPanels`/`TabPanel` → Chakra `Tabs.Root/List/Trigger/Content`; put recipe variant at root. Coordinate with adapter removal because current public root exports use adapter Tabs. |

These are “easy” only when usage is conventional. Instances relying on arbitrary style props,
custom `as` behaviour, or unusual child composition should be flagged by the codemod for
manual review.

## 3. Chakra-like but heavily customized — consumer refactor required

These components use Chakra internally but change enough composition, behaviour, or data shape
that callers cannot simply follow Chakra documentation. A raw-Chakra target needs a migration
plan; in several cases the better target is an honest generic component with a closed API.

| Export | Why direct Chakra is not a shallow migration | Likely target |
|---|---|---|
| **Accordion** (+ Item/Button/Panel, MobileOnlyAccordion) | Full Chakra props are exposed, but public composition is replaced by custom components; trigger adds `leftIcon`/indicator and panel inserts content/body. | Primitive target with temporary compatibility facade; see `accordion-migration.md`. |
| **Alert** | Fixed title/description/link/icon/dismissible message abstraction with internal dismissal, while leaking Chakra root props. | Honest generic component; see `alert-migration.md`. |
| **Button** | Tri-modal button/link/icon union, manual unregistered recipe, icon insertion, NextLink behaviour, and partial Chakra prop leak. | Generic component; preserve behaviour but replace Chakra prop sourcing. |
| **Input** (+ SearchField) | Debounce/clear/addon/icon/read-only union and full Chakra input prop leak. | Generic component; preserve behaviour and close API. |
| **Breadcrumbs** | Wrapper creates root/list/separators and clones children; associated Item/Link wrappers alter composition. | Either direct compound Chakra with JSX migration, or a closed generic breadcrumb API. |
| **Checkbox / CheckboxGroup** | Label/fullWidth/indeterminate/font/padding semantics and custom callback; group inherits the single-control surface. | Generic form component, or substantial Chakra compound migration. |
| **Radio / RadioList** | `items[]` data model, optional render function, and synthetic `ChangeEvent` bridge. | Generic component; resolve event model before any Chakra migration. |
| **Switch** | Required `id`, optional label, custom reduced root API and custom slot assembly. | Generic control or compound Chakra migration. |
| **RangeSlider** | Array callbacks over a rendered two-thumb Chakra compound slider. | Consumer must adopt root/control/track/thumb composition; involved migration. |
| **DiscreteSlider** | Generic marks/value abstraction over Slider, with value-index mapping and indentation behaviour. | Generic component; no benefit in exposing raw Slider directly. |
| **Dialog** | Curated title/actions/footer/body API and responsive/full-screen policy. | Generic overlay; direct Dialog requires caller-built slots/actions. |
| **Drawer** | Partial compound wrapper with a custom root API and exported subparts. | Decide a single overlay archetype; consumer migration is structural. |
| **Popover** | Curated trigger/content API, positioning and visual props, controlled state. | Generic overlay or raw compound migration. |
| **HoverCard** | Curated trigger/content API with padding/positioning/arrow defaults. | Generic overlay or raw compound migration. |
| **Tooltip** | `label` API, cloned trigger handlers, internal open state. | Raw Tooltip needs compound JSX and a different interaction model. |
| **Menu** | `items[]`, title/icon/checkmark data model, and styling props. | Generic menu, or involved conversion to Chakra item composition. |
| **Pagination** | `totalPages/currentPage/onChange` abstraction that renders all Chakra pagination slots and controls. | Generic pagination; raw Chakra requires caller rendering triggers/context. |
| **Progress** | Validates/clamps values, renders label/completion icon and layout. | Generic progress/status component, or structural raw Chakra migration. |
| **Rating** | Custom CSS-star display and `rating` scalar; not Chakra RatingGroup composition. | Generic display component; a Chakra RatingGroup move changes rendering/API. |
| **Field / FormControlSection** | Fixed field/error/hint/tooltip layout and semantics. | Generic form composition, not a direct primitive. |
| **Chip** | Click/link/selected/disabled semantics rendered through a Flex with keyboard handling. | Generic interactive component; direct Chakra migration changes element and behaviour. |
| **Link adapter-like behaviours in features** | Some feature components compose routing/links, filters, or domain state around Chakra. | Keep custom/domain APIs; do not recast as primitives. |

## Custom/domain components — do not force into the three Chakra buckets

The following are not honest direct-Chakra candidates. Their appropriate next question is
whether they are a closed generic component or a feature, not how to re-export a Chakra
primitive:

- **Generic but custom:** `Avatar`, `Count`, `EnergyLabel`, `MissingImage`, `Section`,
  `SimpleHeader`, `FullHeight`, `FormControlSection`, `Carousel`, `RangeSliderWithChart`,
  `RangeSliderWithScale`, `TopListingBadge`, and marked/highlighted text helpers.
- **Filter/form patterns:** `CheckboxFilter`, `RangeFilterInput`,
  `RangeFilterInputWithSlider`, and `filterPatterns/*`.
- **Domain/product features:** `ArticleTeaser`, `ErrorPage`, `FocusedHeader`,
  `GalleryHeader`, `navigation/*`, `tenantSelection`, `TopVehicleSharedBadge`,
  `VehicleReference`, `devOverlay`, and application/layout compositions.
- **Infrastructure:** `Toast`/toaster setup, providers, icons, and translation helpers.

Some classification calls can change after product review—for example, Carousel is generic
rather than domain-bound—but none of these becomes a Chakra primitive merely because its
implementation imports Chakra.

## Recommended sequencing

1. Ship bucket 1 as additive source cleanups. They remove inconsistent prop restrictions
   without making callers change.
2. Inventory actual usage for bucket 2, provide deprecation guidance/codemods, then migrate
   in small batches.
3. Do not launch bucket 3 as one refactor. First decide whether each component should be an
   honest generic component or a primitive target with a compatibility facade.
4. Freeze new partial abstractions immediately: wrappers must either preserve Chakra API and
   composition exactly, or own a closed semantic API.
