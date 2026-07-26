# Seller-web component categories

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
