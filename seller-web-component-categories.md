# Component categories

This classifies every public JSX export in the seller-web inventory exactly once.
It is an organisational view of the current public API, not a recommendation or a
statement about the component's implementation quality.

| Category | Components |
|---|---:|
| Layout primitives | 11 |
| UI primitives | 94 |
| Shared product components | 25 |
| Total | 130 |

## Layout primitives

Generic structural and responsive-layout building blocks.

- `AspectRatio`, `Box`, `Center`, `Flex`, `FullHeight`, `Grid`, `GridItem`,
  `Hide`, `Show`, `SimpleGrid`, `Stack`

## UI primitives

Generic, reusable UI building blocks, including bespoke components-pkg wrappers.

- `Accordion`, `AccordionButton`, `AccordionItem`, `AccordionPanel`, `Alert`,
  `AutoScout24AppLogo`, `Avatar`, `Badge`, `BreadcrumbLink`, `Breadcrumbs`,
  `BreadcrumbsItem`, `Button`, `Card`, `CardBody`, `CardFooter`, `CardHeader`,
  `Carousel`, `Checkbox`, `CheckboxFilter`, `CheckboxGroup`, `Chip`, `Collapse`,
  `ColorPicker`, `Count`, `DatePicker`, `Dialog`, `DialogFilter`, `DiscreteSlider`,
  `Divider`, `Drawer`, `DrawerBody`, `DrawerContent`, `DrawerOverlay`, `EnergyLabel`,
  `FormControl`, `FormControlSection`, `FormLabel`, `H1`, `H2`, `H3`, `H4`, `H5`,
  `H6`, `HighlightedText`, `HoverCard`, `Input`, `Link`, `LinkBox`, `LinkOverlay`,
  `List`, `ListItem`, `MarkedText`, `Menu`, `MissingImage`, `MobileOnlyAccordion`,
  `Modal`, `ModalCloseButton`, `MotoScout24AppLogo`, `OrderedList`, `Pagination`,
  `Popover`, `Progress`, `Radio`, `RadioList`, `RangeSlider`, `Rating`,
  `SearchableList`, `SearchField`, `Section`, `Select`, `SelectMenu`, `Skeleton`,
  `Spinner`, `Switch`, `Tab`, `Table`, `TabList`, `TabPanel`, `TabPanels`, `Tabs`,
  `Tbody`, `Td`, `Text`, `Textarea`, `Tfoot`, `Th`, `Thead`, `TimePicker`, `Tooltip`,
  `TopListingBadge`, `TopRightToast`, `TopToast`, `Tr`, `UnorderedList`

## Shared product components

Product/domain composition, app-shell conventions, or shared product infrastructure.

- `AppLayout`, `AppLayoutContent`, `AppLayoutFooter`, `AppLayoutHeader`,
  `ArticleTeaser`, `DevOverlay`, `ErrorPage`, `FilterHeading`, `FocusedHeader`,
  `Footer`, `GalleryHeader`, `HeaderNavigation`, `InsertionLayout`,
  `LayoutWithVehicleReference`, `OpenFilterButton`, `PageLayout`, `PopoverFilter`,
  `RangeFilterInput`, `RangeFilterInputWithSlider`, `SingleColumnCenteredLayout`,
  `TenantSelection`, `ThemeProvider`, `TopVehicleSharedBadge`, `TwoColumnsLayout`,
  `VehicleReference`

## Source links

Links target the implementation on this PR branch.

| Component | Source |
| --- | --- |
| `Accordion` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/accordion/index.tsx) |
| `AccordionButton` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/accordion/AccordionButton.tsx) |
| `AccordionItem` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/accordion/AccordionItem.tsx) |
| `AccordionPanel` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/accordion/AccordionPanel.tsx) |
| `Alert` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/alert/index.tsx) |
| `AppLayout` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/layout/app/AppLayout.tsx) |
| `AppLayoutContent` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/layout/app/Content.tsx) |
| `AppLayoutFooter` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/layout/app/Footer.tsx) |
| `AppLayoutHeader` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/layout/app/Header.tsx) |
| `ArticleTeaser` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/articleTeaser/index.tsx) |
| `AspectRatio` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/aspectRatio/index.tsx) |
| `AutoScout24AppLogo` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/icons/AutoScout24AppLogo.tsx) |
| `Avatar` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/avatar/index.tsx) |
| `Badge` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/badge/index.tsx) |
| `Box` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Box/index.tsx) |
| `BreadcrumbLink` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/breadcrumbs/Link.tsx) |
| `Breadcrumbs` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/breadcrumbs/index.tsx) |
| `BreadcrumbsItem` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/breadcrumbs/Item.tsx) |
| `Button` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Button/index.tsx) |
| `Card` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/card/index.tsx) |
| `CardBody` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/card/index.tsx) |
| `CardFooter` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/card/index.tsx) |
| `CardHeader` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/card/index.tsx) |
| `Carousel` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/carousel/index.tsx) |
| `Center` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/center/index.tsx) |
| `Checkbox` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Checkbox/index.tsx) |
| `CheckboxFilter` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/checkboxFilter/index.tsx) |
| `CheckboxGroup` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/checkboxGroup/index.tsx) |
| `Chip` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Chip/index.tsx) |
| `Collapse` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/collapse/index.tsx) |
| `ColorPicker` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/colorPicker/index.tsx) |
| `Count` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/count/index.tsx) |
| `DatePicker` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/datePicker/index.tsx) |
| `DevOverlay` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/devOverlay/index.tsx) |
| `Dialog` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/dialog/index.tsx) |
| `DialogFilter` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/filterPatterns/dialog/index.tsx) |
| `DiscreteSlider` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/discreteSlider/index.tsx) |
| `Divider` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Divider/index.tsx) |
| `Drawer` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/drawer/index.tsx) |
| `DrawerBody` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/drawer/DrawerBody.tsx) |
| `DrawerContent` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/drawer/DrawerContent.tsx) |
| `DrawerOverlay` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/drawer/DrawerOverlay.tsx) |
| `EnergyLabel` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/energyLabel/index.tsx) |
| `ErrorPage` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/errorPage/index.tsx) |
| `FilterHeading` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/filterPatterns/Heading.tsx) |
| `Flex` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/flex/index.tsx) |
| `FocusedHeader` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/focusedHeader/index.tsx) |
| `Footer` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/navigation/footer/index.tsx) |
| `FormControl` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/FormControl/index.tsx) |
| `FormControlSection` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/formControlSection/index.tsx) |
| `FormLabel` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/FormLabel/index.tsx) |
| `FullHeight` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/fullHeight/index.tsx) |
| `GalleryHeader` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/galleryHeader/index.tsx) |
| `Grid` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/grid/index.tsx) |
| `GridItem` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/grid/index.tsx) |
| `H1` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/heading/index.tsx) |
| `H2` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/heading/index.tsx) |
| `H3` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/heading/index.tsx) |
| `H4` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/heading/index.tsx) |
| `H5` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/heading/index.tsx) |
| `H6` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/heading/index.tsx) |
| `HeaderNavigation` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/navigation/header/index.tsx) |
| `Hide` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Hide/index.tsx) |
| `HighlightedText` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/text/HighlightedText.tsx) |
| `HoverCard` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/hoverCard/index.tsx) |
| `Input` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Input/index.tsx) |
| `InsertionLayout` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/InsertionLayout/index.tsx) |
| `LayoutWithVehicleReference` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/layout/WithVehicleReference.tsx) |
| `Link` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Link/index.tsx) |
| `LinkBox` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/linkOverlay/index.tsx) |
| `LinkOverlay` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/LinkOverlay/index.tsx) |
| `List` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/List/index.tsx) |
| `ListItem` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/List/index.tsx) |
| `MarkedText` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/text/marked/index.tsx) |
| `Menu` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/menu/index.tsx) |
| `MissingImage` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/missingImage/index.tsx) |
| `MobileOnlyAccordion` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/mobileOnlyAccordion/index.tsx) |
| `Modal` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Modal/index.tsx) |
| `ModalCloseButton` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Modal/index.tsx) |
| `MotoScout24AppLogo` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/icons/MotoScout24AppLogo.tsx) |
| `OpenFilterButton` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/filterPatterns/dialog/OpenFilterButton.tsx) |
| `OrderedList` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/List/index.tsx) |
| `PageLayout` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/layout/Page.tsx) |
| `Pagination` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/pagination/index.tsx) |
| `Popover` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Popover/index.tsx) |
| `PopoverFilter` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/PopoverFilter/index.tsx) |
| `Progress` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/progress/index.tsx) |
| `Radio` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/radio/index.tsx) |
| `RadioList` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/radio/RadioList.tsx) |
| `RangeFilterInput` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/rangeFilterInput/index.tsx) |
| `RangeFilterInputWithSlider` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/RangeFilterInputWithSlider/index.tsx) |
| `RangeSlider` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/rangeSlider/index.tsx) |
| `Rating` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/rating/index.tsx) |
| `SearchableList` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/list/SearchableList.tsx) |
| `SearchField` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/input/SearchField.tsx) |
| `Section` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/section/index.tsx) |
| `Select` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Select/index.tsx) |
| `SelectMenu` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/SelectMenu/index.tsx) |
| `Show` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Show/index.tsx) |
| `SimpleGrid` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/SimpleGrid/index.tsx) |
| `SingleColumnCenteredLayout` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/layout/SingleColumnCentered.tsx) |
| `Skeleton` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Skeleton/index.tsx) |
| `Spinner` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/spinner/index.tsx) |
| `Stack` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Stack/index.tsx) |
| `Switch` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Switch/index.tsx) |
| `Tab` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Tab/Tab.tsx) |
| `Table` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Table/index.tsx) |
| `TabList` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/tab/TabList.tsx) |
| `TabPanel` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/tab/TabPanel.tsx) |
| `TabPanels` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/tab/TabPanels.tsx) |
| `Tabs` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Tab/Tabs.tsx) |
| `Tbody` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Table/index.tsx) |
| `Td` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Table/index.tsx) |
| `TenantSelection` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/tenantSelection/index.tsx) |
| `Text` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Text/index.tsx) |
| `Textarea` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/textarea/index.tsx) |
| `Tfoot` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Table/index.tsx) |
| `Th` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Table/index.tsx) |
| `Thead` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Table/index.tsx) |
| `ThemeProvider` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/themeProvider/index.tsx) |
| `TimePicker` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/timePicker/index.tsx) |
| `Tooltip` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/tooltip/index.tsx) |
| `TopListingBadge` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/topListingBadge/index.tsx) |
| `TopRightToast` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/toast/index.tsx) |
| `TopToast` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/toast/index.tsx) |
| `TopVehicleSharedBadge` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/topVehicleSharedBadge/index.tsx) |
| `Tr` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/Table/index.tsx) |
| `TwoColumnsLayout` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/layout/TwoColumnsLayout.tsx) |
| `UnorderedList` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/adapter/List/index.tsx) |
| `VehicleReference` | [source](https://github.com/masone/components-pkg/blob/claude/components-structure-rfc-6e1337/src/components/vehicleReference/index.tsx) |
