# Component inventory

_Analysis date: 2026-07-16. No source was changed. The requested RFC documents are not
present in this checkout; their existing repository worktree at
`.claude/worktrees/components-structure-rfc-6e1337/` was used as the design source of
truth. APIs below are from this checkout's exports and implementation, not inferred from
names. “Chakra-derived” includes `Pick`, `Omit`, `Exclude`, and intersections with Chakra
prop types; all are leaks for a component/feature under `api-conventions.md`._

## A. Summary

| Component / export group | Category | Pattern-correct | API conformance | Verdict | Action |
|---|---|---:|---:|---|---|
| Box, Flex, Stack, Grid, SimpleGrid, Center, AspectRatio | primitive | ❌ | ✅ | Use one uniform primitive contract. | **Direct 1:1 Chakra re-exports** |
| Alert, Avatar, Badge, Button, Card, Checkbox, Chip, CloseButton, Collapse, ColorPicker, Count, DatePicker, Dialog, Drawer, Field, Heading, HoverCard, Input, Link, LinkOverlay, List, Menu, Pagination, Popover, Progress, Radio, RangeSlider, Rating, Select, Separator, Skeleton, Spinner, Switch, Tab, Text, Textarea, TimePicker, Tooltip | component | ⚠️ | ❌ | Generic, but most leak Chakra types/style props or use inconsistent event/controlled names. | — |
| ArticleTeaser, Carousel, CheckboxFilter, DiscreteSlider, EnergyLabel, ErrorPage, filter patterns, FocusedHeader, FormControlSection, FullHeight, GalleryHeader, layouts, MissingImage, MobileOnlyAccordion, Navigation, range-filter inputs, Section, SimpleHeader, tenantSelection, TopListingBadge, TopVehicleSharedBadge, VehicleReference | feature | ⚠️ | ❌ | Semantic/domain composition predominates, but several surfaces expose Chakra layout/style types and several own recipes. | — |
| themeProvider, translationProvider, icons | out of scope / infra | — | — | Providers/assets, not catalogued as components. | — |

The confirmed set is **7 primitives, 37 components, and 25 feature/composition exports**
(some folders export multiple public components). The strict “one folder = one component”
count is not meaningful for `layout`, `navigation`, `filterPatterns`, `text`, and compound
folders; their public exports are called out below. On the folder-level verdict, 3 are ✅
(Box, AspectRatio, Separator), 17 are ⚠️, and the remainder need work.

## B. Detailed inventory

### Primitives

| Component | Today’s props surface and nature | Intended interface / delta | Pattern-correctness and verdict | Action |
|---|---|---|---|---|
| **Box** | Exact `BoxProps` re-export. Transparent Chakra API. | Keep exact Chakra API. | ✅ Correct transparent re-export. | Keep as a direct re-export. |
| **AspectRatio** | `AspectRatioProps` re-export, assigned alias. | Keep exact Chakra API. | ✅ Thin alias only; a direct re-export would be even cleaner. | Replace alias with direct re-export. |
| **Flex** | `Omit<ChakraFlexProps, 'gap'\|'columnGap'\|'rowGap'>`. | Exact Chakra `FlexProps`; restore gaps and remove wrapper. | ❌ Violates transparent primitive rule through `Omit`. | Replace with direct Chakra re-export. |
| **Stack** | `Pick<ChakraStackProps, align, children, direction, justify, gap, wrap, margin*, padding*, separator, alignItems, width>`. | Exact Chakra `StackProps` plus `StackSeparator` direct re-export. | ❌ Whitelist and v2 `align` leak/omission contradict transparency. | Replace Stack and StackSeparator with direct Chakra re-exports. |
| **Grid** | Chakra-derived exported `GridProps`/thin wrapper (including style props). | Exact Chakra `Grid` re-export. | ⚠️ Display-name alias only. | Replace Grid and GridItem with direct Chakra re-exports; preserve Storybook naming through metadata. |
| **SimpleGrid** | `Pick<SimpleGridProps, minChildWidth, columns, children, alignItems, width, rowGap, gap>`. | Exact Chakra `SimpleGridProps`. | ❌ Whitelist is an API transformation. | Replace with direct Chakra re-export. |
| **Center** | `Pick<ChakraCenterProps, children, padding>`. | Exact Chakra `CenterProps`. | ❌ Whitelist is an API transformation. | Replace with direct Chakra re-export. |

### Generic components

Each row’s “intended” means native element attributes plus recipe variants and the listed
semantic props—never a Chakra prop type or `className`/`style` escape hatch.

| Component | Today’s props surface and nature | Intended interface / delta | Pattern-correctness and verdict |
|---|---|---|---|
| **Accordion** (+ Item/Button/Panel) | `AccordionProps = ChakraAccordionRootProps & recipe variants`; subparts likewise intersect Chakra item/trigger/body props. | Curated compound API: `value/defaultValue/onValueChange`, `multiple/collapsible`, children and variants. | ❌ Registered slot recipe, but Chakra-type spread leaks styles; manual child cloning. |
| **Alert** | `Omit<ChakraAlertRootProps,status\|variant\|title> & recipe variants`: `description`, optional `title/link/icon/type`, `dismissible/onDismiss`. | Keep semantic content/dismissal and native attrs; replace `type` with recipe `status` or a documented semantic union; no root props. | ❌ Registered recipe but `Omit` leak; manually applies recipe; root arbitrary style props pass through. |
| **Avatar** | `withNotification?`, `color?: BoxProps['color']`. | `withNotification?`, semantic color token only if genuinely needed (otherwise recipe variant). | ⚠️ Registered recipe; `BoxProps['color']` is a style-type leak. |
| **Badge** | `Exclude<ChakraBadgeProps,'children'> & {text:string}`. | Branded-element native `span` attrs + variants; `children` rather than `text`. | ❌ Recipe registered but Chakra type leaks and `text` sugar departs from element convention. |
| **Breadcrumbs** (+ Item/Link) | Chakra root props plus `separator?`; child Link uses Chakra props/Omit. | Compound/curated children, `separator?`, semantic link attrs only. | ❌ Slot recipe registered; Chakra root/type leaks and clone composition. |
| **Button** | Recipe variants; tri-modal discriminated union for submit/button/link/icon; `children`, icons, `ariaLabel`, `disabled`, link fields; `Omit<ChakraButtonProps,...>` still passes many style props. No declared `loading/loadingText` although adapter maps `isLoading`. | Native button/anchor attrs (minus style/className), recipe variants, `disabled`, `loading/loadingText`, `fullWidth`, `leftIcon/rightIcon`; separate semantic link treatment or a coherent branded-element shape. | ❌ Behaviour/ref forwarding is legitimate, but manual unregistered recipe, `as` behavioural workaround, Chakra `Omit` leak, `ariaLabel` instead of native `aria-label`, and inconsistent loading. |
| **Card** | Chakra `Card.Root/Header/Body/Footer` namespace and deprecated flat aliases; Chakra slot prop types. | Either transparent primitive (then direct re-export) or curated compound with no Chakra surface; classification call: generic component because branded slot recipe exists. | ⚠️ Registered slot recipe and thin compound export, but it intentionally exposes Chakra props; remove deprecated aliases and decide contract. |
| **Checkbox** | Recipe variants plus `name`, `value?`, `disabled?`, `checked?`, `invalid?`, `indeterminate?`, `readOnly?`, `fullWidth?`, `label?`, `paddingY?: BoxProps[...]`, `fontWeight`, `onChange(details)`. | Native input attrs + `checked/onChange`, `disabled`, label, variants, `fullWidth`; remove `paddingY`/font style controls and align callback name/detail convention. | ❌ Recipe registered/ref forwarded, but `paddingY` leaks styling and controlled event is non-native; internal width/style application. |
| **CheckboxGroup** | Extends `CheckboxProps` with group/value selection API. | A controlled `value/onChange` group with item semantics, not inherited single-checkbox styling API. | ❌ Inheritance carries checkbox and style leakage; normalize group semantics. |
| **Chip** | `onClick?`, `href?`, `aria-label?`, `disabled?`, recipe variants. | Native button/anchor attrs + variants, `disabled`, selected; no behavioural `as` switching exposed. | ⚠️ Registered recipe but uses a Flex `as` workaround and applies interactive style props inline; ref is not forwarded. |
| **CloseButton** | Semantic `onClick?` plus a small local props type. | Native button attrs + variants, `aria-label`, `onClick`. | ⚠️ Recipe registered; verify native attributes/ref are forwarded (current surface is too narrow). |
| **Collapse** | v2-shaped `in`, `animateOpacity?` plus `Omit<CollapsibleRootProps,'open'>`. | `open/onOpenChange` or a deliberately controlled `open` semantic API; native attrs only. | ❌ Banned v2 `in`/Chakra Omit leak; no recipe. |
| **ColorPicker** | `interface extends InputProps`, `invalid?`; all Chakra Input/style props accepted. | Native color-input attributes plus `value/onChange`, `disabled`, `invalid`; appearance in input recipe. | ❌ Chakra type and style props leak; no own recipe and hardcoded inline appearance. |
| **Count** | recipe variants + `count:number`, `ariaLabel?`. | Keep count/variant; native `aria-label` spelling and span attrs. | ⚠️ Registered recipe and closed semantic surface, but non-native `ariaLabel` spelling. |
| **DatePicker / TimePicker** | `Pick<InputProps,onFocus/onBlur/onChange> & input variants & {value?, invalid?}`; date has `min?:Date`, time forces type. | Native input attributes (`name`, `value`, `min`, handlers, disabled, etc.) and controlled `value/onChange`; semantic date conversion may remain. | ❌ Chakra prop sourcing; missing native attributes; DatePicker does not wrap `Field.Root`; manual recipe styles. |
| **Dialog** | Curated: `title?`, `open?`, `onOpenChange?`, `size`, `variant`, `overlayColor`, `motionPreset`, two action descriptors, `disableBodyPadding?`, children. | Same curated overlay vocabulary, perhaps `actions`/slots; native dialog attrs only where valid. | ⚠️ Registered slot recipe and semantic generic API; applies layout/style internally correctly, but no ref and action model is rigid. |
| **Drawer** | `Omit<DrawerRootProps,'open'\|'onOpenChange'>` plus custom open/onOpenChange/content/side fields and slot subexports. | Curated overlay API like Dialog. | ❌ Registered recipe but Chakra-type Omit leak/partial compound inconsistency. |
| **Field** | Recipe variants plus semantic `id,label,errorMessage,hint,tooltip,required,disabled,...` and Chakra-derived portions. | Generic field semantic props + native `htmlFor/id`; no arbitrary Chakra field/style props. | ❌ Registered slot recipe but Chakra prop sourcing and manual styles. |
| **Heading** (H1–H6) | `Omit<ChakraHeadingProps,'as'\|'asChild'>`, fixed HTML heading/default textStyle. | Native heading attrs plus typography recipe variants/semantic level. | ❌ Correct category (brand typography), but Chakra Omit style leak; no registered heading recipe. |
| **HoverCard** | `content`, children, placement, size, padding/maxWidth/position, gutter, showArrow. | Curated trigger/content and `open/onOpenChange` if control is needed; no raw positional/layout props. | ⚠️ Registered recipe exists but component hardcodes styles rather than using it; native Chakra prop-derived placement types. |
| **Input** (+ SearchField) | `ChakraInputProps & input variants` combined with controlled/debounced/read-only union; `name`, icon/addon/clear/search semantic options. | Native input attributes (minus style) + `value/onChange`, disabled/readOnly/invalid, variants and explicit addons/clear/debounce props. | ❌ Legitimate behaviour/ref wrapper and registered slot recipe, but full Chakra spread leaks styles, union is complex, and manual slot styling remains. |
| **Link** | `ChakraLinkProps & {isExternal?} & recipe variants`. | Native anchor attrs + variants and perhaps `external?`; no Chakra styling surface. | ❌ Registered recipe but total Chakra leak and v2-ish `isExternal`; no ref wrapper. |
| **LinkOverlay** | Exact Chakra `LinkBox`/`LinkOverlay` re-exports. | Classification is ambiguous: primitive if kept transparent, otherwise a custom link component. | ⚠️ Thin and correct as a transparent Chakra export, but conflicts with its generic-component classification. |
| **List** (+ SearchableList) | Chakra List Root/Item namespace plus deprecated aliases; SearchableList has own `items`, query/selection semantics. | Split: transparent List primitive or curated generic List; SearchableList should be a generic component with semantic items API. | ⚠️ Slot recipe registered but base export exposes Chakra; separate SearchableList rather than mixing archetypes. |
| **Menu** | `title`, `items[{text,value,onClick}]`, value, typography/color/gap/placement controls, checkmark/icon flags. | Semantic title/items/value/onChange, icon, placement; move `fontWeightTitle/menuColor/menuOptionColor/iconSpacing` to recipe variants. | ❌ Registered slot recipe, but several public Chakra style-derived props leak. |
| **Pagination** | `totalPages`, zero-based `currentPage`, `onChange(page)`, plus `marginTop/marginBottom: BoxProps`. | `page` or `value`, `onChange`, total/count; external margins removed. | ❌ Registered recipe but raw layout props and non-canonical controlled name. |
| **Popover** | Curated content/trigger, padding/maxWidth/position/placement/size/open/onOpenChange/arrow/close/gutter/autofocus. | Keep content/trigger and Openable vocabulary; move visual sizing/padding/position to recipe or a small semantic variant. | ⚠️ Registered recipe but never consumed; manual Chakra styling and Chakra-derived placement type. |
| **Progress** | `current`, `max`, `label(current,max)`. | This is a sound closed semantic API; permit native aria label if needed. | ⚠️ Registered slot recipe exists and Chakra progress gets it, but wrapping Stack/Text causes layout coupling; no ref. |
| **Radio** (+ RadioList) | variants, `value?`, `items[{value,label?,invalid?,disabled?}]`, orientation, name, `onChange(ChangeEvent)`, optional render function. | Controlled `value/onChange(value or details)` (choose one), items/name/orientation; no synthetic event bridge. | ❌ Registered recipe, ref forwarded, but recreates synthetic event and uses a Chakra Stack directly; likely RHF decision needed. |
| **RangeSlider** | `Omit<Slider.Root props,'onChange'>`, `onChange?(number[])`, `onChangeEnd?(number[])`, all remaining Chakra style/slider props. | Curated `value/defaultValue/onChange/onChangeEnd/min/max`, variants; no Chakra spread. | ❌ Chakra Omit leak and nonstandard v2 callback names. |
| **Rating** | recipe variants + `rating:number`. | Keep rating/semantic aria label; optional max/readOnly only if needed. | ⚠️ Registered recipe, closed API; dynamic CSS variables/raw colour should move into recipe/token model. |
| **Select** | recipe variants + `disabled/invalid`, full `NativeSelectFieldProps`, `name`, options/value union. | Native select attrs minus styles + `options`, controlled `value/onChange`, disabled/invalid, variants. | ❌ Registered recipe/ref forwarding, but Chakra field spread leaks styles. |
| **Separator** | Chakra `SeparatorProps` alias. | If a primitive: exact direct re-export. | ✅ Thin alias; recipe is registered. |
| **Skeleton** | Chakra `SkeletonProps` direct alias. | Either classify primitive and direct re-export, or custom semantic `loading` wrapper. | ⚠️ Thin and registered recipe; category boundary needs decision because it exposes Chakra API. |
| **Spinner** | `ChakraSpinnerProps & {size?: xs\|sm\|md\|lg}`, but implementation passes only size (drops all others). | Native span attrs + variants, accessible label; or transparent re-export. | ❌ Declared Chakra surface is false: props silently disappear; recipe registered. |
| **Switch** | `Pick<Switch.RootProps,onCheckedChange\|checked\|disabled> & {id,label?}`. | Native checkbox attrs + controlled `checked/onChange`, label, variants. | ❌ Registered recipe but incomplete props (does not pass id), no ref, and non-uniform callback naming. |
| **Tab** (+ TabList/Panel/Panels) | `Omit<ChakraTabs*Props,'variant'> & recipe variants`; root clone-injects variant. | One compound API with `value/defaultValue/onValueChange`, children and variants. | ❌ Registered recipe but Chakra Omit leaks and custom cloning. |
| **Text** (+ HighlightedText/MarkedText) | Exact `TextProps` Chakra alias; MarkedText additionally `Exclude<BoxProps,...>` + variants/highlightColor. | Text is correctly a **component** (brand typography): native span attrs + text style/variants; MarkedText only its semantic variant/colors. | ❌ Text has no closed branded API; MarkedText leaks Box props and owns registered slot recipe despite being content composition. |
| **Textarea** | Hand whitelist: name, value, placeholder, disabled, focus/change/keyboard handlers, rows/cols, autoresize, textStyle, min/maxHeight, padding. | Native textarea attrs minus `style/className`; variants, `value/onChange`, disabled, loading if real. | ❌ Pure whitelist that still exposes style props; recipe registered but wrapper adds no behaviour. |
| **Tooltip** | children from Chakra root plus `label`, placement, `maxWidth`. It owns open state internally. | trigger + label/content, optional controlled `open/onOpenChange`, semantic size variant. | ❌ Slot recipe registered but unused; `maxWidth` leak and child event handlers are overwritten rather than composed. |

### Features and compositions

| Component | Today’s props surface and nature | Intended interface / delta | Pattern-correctness and verdict |
|---|---|---|---|
| **ArticleTeaser** | recipe variants + `title,text,imageUrl,url,maxImgW?: BoxProps['maxWidth']`. | Content fields plus semantic image size variant if required; remove Box type. | ❌ Generic-looking but an editorial entity feature; registered slot recipe is a feature-recipe smell and `maxImgW` leaks style. |
| **Carousel** | `startIndex`, callbacks, responsive `slidesPerView`, loop, scroll count; union for normal ReactNode slides vs full-screen `{slide,thumbnail,onSlideEnter,onSlideLeave}`; pagination enum. | This is actually a **generic component** by mechanism/content-injection test, not a feature; retain semantic config/children, normalize `value/onChange` if controlled. | ⚠️ Registered slot recipe and real behaviour; reclassify to component, remove internally passed primitive style props only. |
| **CheckboxFilter** | Generic filter/item/query callbacks and facet presentation subcomponents. | Domain filter object/value/callbacks only; no Chakra prop types. | ⚠️ Feature call driven by filter domain; inspect subparts when redesigning because it composes UI and filters. |
| **DiscreteSlider** | `marks[{value,label,stepValue?}]`, `value`, `onValueChanged`, `applyIndentation?`. | Generic component rather than feature: controlled `value/onChange`, marks, indentation semantic option. | ⚠️ No recipe despite visual control; noncanonical `onValueChanged`; reclassify component. |
| **EnergyLabel** | recipe `efficiency` required plus label semantics. | Keep scalar efficiency/variant and native aria props. | ⚠️ Correct generic component by scalar test (not feature); recipe registered; reclassify. |
| **ErrorPage** | HOC-wrapped `ErrorPageContentProps`: `statusCode`, `language`, `onButtonClick?`. | Domain/error semantic props; language should be supplied by provider where possible. | ⚠️ Feature correct; no own recipe; direct Chakra composition stays internal. |
| **FilterPatterns: DialogFilter, PopoverFilter, Heading, OpenFilterButton** | Filter-domain props (filter selection, labels/actions, open/button display/padding). | Preserve domain semantics; remove any Chakra `Pick` types such as OpenFilterButton display/padding and use named variants. | ❌ Correct feature category, but `dialogFilter`/`popoverFilter` recipes are feature-owned smells and style props leak through subcomponents. |
| **FocusedHeader** | `brand: Brand`. | Keep brand only. | ⚠️ Brand-bound feature; no recipe, composition is internal. |
| **FormControlSection** | `id`, `errorMessage?`, `label?`, `hint?`, `tooltip?`, children. | This is generic and should be a **component** (not feature): same semantic API/native field attrs. | ⚠️ Reclassify; no recipe but inline visual styling needs a slot recipe. |
| **FullHeight** | children only. | A layout primitive would need exact Chakra API; otherwise a generic semantic component with children only. | ⚠️ Ambiguous layout helper; current closed API/implementation makes it a component, no recipe. |
| **GalleryHeader** | HOC-wrapped `GalleryHeaderProps` plus children (brand/gallery content fields in Content). | Domain/gallery semantics only. | ❌ Feature correct, but registered slot recipe is a feature-owned styling smell. |
| **Layout: BaseGridLayout, BaseLayout, PageLayout, SingleColumnCenteredLayout, TwoColumnsLayout, LayoutWithVehicleReference** | Layout/content slots; container widths; Page header/footer/ads; two-column `left/right` content+column counts; Vehicle layout takes `vehicle: VehicleReferenceProps`. `BaseGridLayout = GridProps`. | Generic layouts should be components with semantic slots/widths; Vehicle layout is feature. `BaseGridLayout` must lose GridProps/style surface or become a primitive composition internal only. | ❌ Mixed export group: raw Grid leak, feature reuses `VehicleReferenceProps`, and layout assigns style internally; split/reclassify. |
| **MissingImage** | `Pick<BoxProps,width,height,aspectRatio>`. | Semantic `size`/aspect ratio if needed, otherwise no props. | ❌ Generic component, not feature; Box style-type leak; no recipe. |
| **MobileOnlyAccordion** | Accordion recipe variants + Pick root `multiple/collapsible/value/onValueChange`, children. | It is a generic component/composition with same controlled API; name promises responsive behaviour but implementation only delegates. | ❌ Chakra Pick leak and misleading/incomplete behaviour; no own recipe needed. |
| **Navigation: Header, Footer and descendants** | Domain/tenant/user/language/navigation configuration, callbacks and React content slots. | Preserve navigation/domain config; no Chakra types/style props at public boundary. | ⚠️ Feature correct; review individual config exports separately before publishing them as public API. |
| **RangeFilterInput** | Generic `from/to{name,value?,placeholder?,ariaLabel?}`, `handleChange/onBlur`, unit, and `Pick<NumberInput.Root,min,max,disabled,onFocus>`. | Feature filter props plus `value/onChange`; remove Chakra Pick and use canonical callback name. | ❌ Correct filter feature, but Chakra prop sourcing and `handleChange` inconsistency. |
| **RangeFilterInputWithSlider** | from/to, `onChange/onBlur`, unit, min/max/disabled/focus; discriminated facets/chartHeight vs scale configuration. | Same domain model, semantic slider configuration; remove inherited Chakra props. | ❌ Correct feature, but Chakra Pick leak; no own recipe. |
| **RangeSliderWithScale / RangeSliderWithChart** | Slider selection, callbacks and scale/facet/chart props. | These are generic components (not filter feature) with controlled selection/onChange. | ⚠️ Reclassify generic components; recipe/closed surface should be assessed with base RangeSlider. |
| **Section** | recipe variants + `title,text?,image?,maxImgW?: BoxProps['maxWidth']`. | Generic component (content composition), semantic image-size variant only. | ❌ Reclassify; registered slot recipe plus Box type/style leak. |
| **SimpleHeader** | `title`, `url`. | Generic header component or app feature; use semantic close/back action rather than bare URL. | ❌ Registered slot recipe but a feature-like composition owns it; no native link attrs. |
| **TenantSelection** | `user: EnrichedSessionUser|null`, `isLoading?`, `selectTenant(sellerId)`, `language`. | Keep domain props; rename `isLoading` to `loading` on next major. | ⚠️ Clear domain feature; no Chakra leakage in public type, but banned v2 loading spelling. |
| **TopListingBadge** | children, `aspectRatio?`. | Generic overlay component (not domain feature): children/aspect ratio semantic API. | ⚠️ Reclassify component; inline Chakra styling but no own recipe. |
| **TopVehicleSharedBadge** | children, `aspectRatio?`, `brand`. | Brand/domain feature semantics; avoid direct Chakra styling in favour of reused component/recipe. | ❌ Correct feature category but hardcodes visual language inline. |
| **VehicleReference** | recipe variants + `image?`, `vehicleTitle`, `price?`, `sellerName?`, `sellerAddress?`, `callToAction?`, `templateColumns?: ComponentProps<Grid>['templateColumns']`. | Keep vehicle fields/action; replace `templateColumns` with a semantic layout variant or internal layout. | ❌ Clear feature; registered feature recipe is a smell and Grid style-type leaks. |
| **DevOverlay** | development variables and display/debug configuration. | Out-of-production diagnostic feature; no Chakra surface. | ⚠️ Treat as feature/dev tooling, not design-system API. |

## C. Cross-cutting findings

1. **Prop-type leakage is pervasive.** `Accordion`, `Alert`, `Button`, `ColorPicker`,
`Input`, `Link`, `RangeSlider`, `Select`, `Tab`, and many feature/layout helpers use a
Chakra type directly or through `Pick`/`Omit`/`Exclude`. `Omit` is not a safe boundary: the
remaining type still carries unblocked Chakra style props.
2. **Style props are public on components/features.** Concrete examples: `Avatar.color`,
`Checkbox.paddingY`, `Pagination.marginTop/marginBottom`, `ArticleTeaser.maxImgW`,
`Section.maxImgW`, `VehicleReference.templateColumns`, `MissingImage.width/height`, and
`Textarea.padding/minHeight/maxHeight`.
3. **Vocabulary is inconsistent.** `Collapse.in`, `TenantSelection.isLoading`,
`DiscreteSlider.onValueChanged`, `RangeFilterInput.handleChange`, `Pagination.currentPage`,
and `Count.ariaLabel` diverge from the stipulated shared vocabulary. Checkbox/Switch use
`onCheckedChange` while Radio reconstructs an HTML `ChangeEvent`; choose controlled
`value/onChange` or explicitly retain Chakra detail callbacks consistently.
4. **Recipe registration is mostly comprehensive.** All listed slot recipes are registered;
the important exception is **Button**: its recipe is not in `recipes/index.ts` and is applied
manually. Several registered recipes are nevertheless manually applied, and feature-owned
recipes are likely wrong: `articleTeaser`, `carousel`, `energyLabel`, `galleryHeader`,
`section`, `simpleHeader`, `vehicleReference`, and the filter patterns.
5. **Archetypes are mixed inside folders.** Card/List expose Chakra compound namespaces,
Dialog/Popover are curated, Button/Input are behaviour wrappers, and Badge/Skeleton are thin
branded elements. Make the chosen archetype explicit before changing each public type.

Notable classification calls: **Text and Heading are components**, per the locked typography
decision, despite Chakra-shaped APIs. **Carousel, DiscreteSlider, EnergyLabel,
FormControlSection, MissingImage, RangeSliderWithChart/Scale, Section, and
TopListingBadge are generic components**, not features: they accept generic content/scalar
configuration rather than a business entity. **VehicleReference, tenantSelection, navigation,
and filter inputs are features** because they encode vehicle/user/navigation/filter-domain
models.

