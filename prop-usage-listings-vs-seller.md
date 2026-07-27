# Prop usage comparison: listings-web vs seller-web

A prop is listed when either report records it as used, including values reported outside named `ComponentProps` (for example `key` and `data-testid`). Counts are intentionally omitted.

| Component | Used in listings only | Used in both | Used in seller-web only |
| --- | --- | --- | --- |
| Accordion | `allowMultiple`, `allowToggle`, `onChange` | `children`, `variant` | `background`, `bg`, `borderColor`, `borderRadius`, `borderWidth`, `boxShadow`, `collapsible`, `defaultValue`, `multiple`, `onValueChange`, `overflow`, `value` |
| AccordionButton | `paddingBottom`, `paddingTop` | `children`, `color`, `leftIcon` | `_expanded`, `_hover`, `alignItems`, `backgroundColor`, `display`, `fontWeight`, `maxW`, `onClick`, `paddingX`, `px`, `py`, `textStyle`, `w` |
| AccordionItem | — | `children` | `_last`, `backgroundColor`, `border`, `borderBottom`, `borderColor`, `borderTopColor`, `borderTopStyle`, `borderTopWidth`, `key`, `value`, `variant` |
| AccordionPanel | — | `children` | `_expanded`, `backgroundColor`, `border`, `padding`, `paddingInline`, `paddingTop`, `paddingX`, `pb`, `pt`, `px`, `textStyle` |
| Alert | — | `children`, `description`, `dismissible`, `icon`, `link`, `onDismiss`, `title`, `type` | — |
| AutoScout24AppLogo | — | — | — |
| Avatar | — | — | — |
| Badge | — | `children`, `text` | — |
| BreadcrumbLink | `as`, `children`, `href`, `minWidth` | — | — |
| Breadcrumbs | `mb` | `children` | — |
| BreadcrumbsItem | — | `children` | — |
| Button | `_active`, `_hover`, `alignItems`, `aria-busy`, `aria-controls`, `aria-label`, `bottom`, `colorScheme`, `h`, `justifyContent`, `key`, `mx`, `my`, `p`, `pointerEvents`, `px`, `rel`, `wordBreak`, `zIndex` | `alignSelf`, `ariaLabel`, `as`, `children`, `disabled`, `display`, `flex`, `flexShrink`, `fontWeight`, `height`, `href`, `icon`, `isDisabled`, `isExternal`, `leftIcon`, `marginLeft`, `maxW`, `mb`, `minW`, `minWidth`, `ml`, `mr`, `mt`, `onClick`, `paddingX`, `position`, `rightIcon`, `size`, `type`, `variant`, `w`, `whiteSpace`, `width` | `bgColor`, `colorPalette`, `cursor`, `gap`, `isLoading`, `marginBottom`, `marginRight`, `marginTop`, `order`, `right`, `style`, `top` |
| Card | — | — | `children`, `outline`, `outlineColor` |
| CardBody | — | — | `children`, `margin`, `padding` |
| CardFooter | — | — | `bgColor`, `children`, `display`, `flexDirection`, `justifyContent` |
| CardHeader | — | — | `children`, `height`, `marginBottom`, `marginTop`, `paddingX`, `textAlign` |
| Carousel | `loop`, `onSlideClick`, `slidesPerView`, `slidesToScroll` | `children`, `fullScreen`, `onSlideSelect`, `startIndex` | `key`, `paginationType` |
| Checkbox | `isInvalid` | `children`, `isChecked`, `label`, `name`, `onChange` | `indeterminate`, `isDisabled`, `key`, `paddingY`, `readOnly`, `variant` |
| CheckboxFilter | `numberOfColumnsOnDesktop` | `items`, `language`, `onApply` | `children` |
| CheckboxGroup | — | — | — |
| Chip | `children`, `isActive`, `onClick` | — | — |
| Collapse | — | `children`, `in` | `animateOpacity` |
| ColorPicker | — | — | `border`, `borderColor`, `borderRadius`, `children`, `name`, `onChange`, `value` |
| Count | — | `ariaLabel`, `children`, `count`, `variant` | — |
| DatePicker | `data-testid` | `children`, `min`, `size` | — |
| Dialog | — | — | — |
| DialogFilter | — | — | — |
| DiscreteSlider | — | — | `applyIndentation`, `children`, `marks`, `onValueChanged`, `value` |
| Divider | `marginLeft`, `marginRight`, `mx`, `position`, `right`, `top`, `w` | `borderColor`, `children`, `display`, `height`, `mb`, `mt`, `my`, `orientation` | `marginBottom`, `marginTop`, `marginX`, `marginY`, `pt`, `width` |
| Drawer | — | — | `children`, `isOpen`, `onClose`, `placement`, `size` |
| DrawerBody | — | — | `children`, `maxWidth`, `paddingX`, `paddingY` |
| DrawerContent | — | — | `borderRadius`, `children`, `data-testid`, `p`, `withCloseButton` |
| DrawerOverlay | — | — | `children` |
| EnergyLabel | `efficiency` | — | — |
| FormControl | — | `children`, `errorMessage`, `hint`, `id`, `isRequired`, `label` | `isDisabled`, `key`, `labelButtonOnClick`, `labelButtonText`, `size`, `tooltip` |
| FormControlSection | — | — | `children`, `errorMessage`, `hint`, `id`, `label`, `tooltip` |
| FormLabel | `children` | — | — |
| H1 | `alignItems`, `display`, `flexWrap`, `marginRight`, `my`, `px`, `w`, `wordBreak` | `children`, `color`, `mb`, `textAlign`, `textStyle` | `fontSize`, `marginBottom`, `marginY`, `mt` |
| H2 | `left`, `marginRight`, `maxWidth`, `mt`, `my`, `position`, `py`, `sx` | `children`, `color`, `marginBottom`, `mb`, `textAlign`, `textStyle` | `display`, `fontSize`, `fontWeight`, `ml`, `pb`, `w`, `whiteSpace` |
| H3 | `marginRight`, `order`, `wordBreak` | `children`, `marginY`, `textStyle` | `alignItems`, `color`, `display`, `fontSize`, `fontWeight`, `lineHeight`, `marginBottom`, `maxWidth`, `mb`, `mt`, `overflow`, `paddingY`, `textAlign`, `textOverflow`, `w`, `whiteSpace`, `width` |
| H4 | — | `children`, `color`, `mb` | `alignItems`, `borderBottomColor`, `borderBottomWidth`, `display`, `fontWeight`, `gap`, `justifyContent`, `margin`, `marginBottom`, `ml`, `mt`, `pb`, `textAlign` |
| H5 | — | — | `children`, `fontWeight`, `mb`, `overflow`, `textOverflow`, `textStyle`, `whiteSpace` |
| H6 | — | — | — |
| HighlightedText | — | — | — |
| HoverCard | — | — | — |
| Input | `autoFocus`, `endElement`, `onKeyDown` | `children`, `debounce`, `icon`, `isClearable`, `isDisabled`, `name`, `onBlur`, `onChange`, `onFocus`, `placeholder`, `ref`, `setInputValue`, `size`, `value` | `data-testid`, `leftAddonElement`, `type` |
| Link | `alignSelf`, `fontSize`, `justifyContent`, `marginLeft`, `minWidth`, `mx`, `order`, `overflowX`, `textColor`, `textOverflow`, `zIndex` | `alignItems`, `aria-label`, `as`, `children`, `color`, `disabled`, `display`, `flexShrink`, `fontWeight`, `href`, `isExternal`, `key`, `leftIcon`, `marginTop`, `ml`, `mt`, `noOfLines`, `onClick`, `paddingX`, `paddingY`, `prefetch`, `rel`, `replace`, `rightIcon`, `style`, `target`, `textAlign`, `textDecoration`, `textStyle`, `type`, `whiteSpace`, `width`, `wordBreak` | `_hover`, `aria-controls`, `aria-expanded`, `bg`, `border`, `borderColor`, `borderRadius`, `cursor`, `data-testid`, `gridArea`, `isTruncated`, `justifySelf`, `left`, `maxW`, `overflow`, `padding`, `pointerEvents`, `position`, `role`, `textTransform`, `top`, `w` |
| LinkBox | — | — | — |
| LinkOverlay | `as`, `children`, `href`, `onClick`, `prefetch`, `rel`, `target` | — | — |
| List | `mt`, `my`, `order` | `aria-label`, `children`, `onClick`, `pt`, `spacing` | `alignItems`, `display`, `flexDirection`, `gap`, `justifyContent`, `paddingLeft`, `size`, `textAlign`, `width` |
| ListItem | `sx` | `children`, `display`, `key`, `mb` | `alignItems`, `my`, `paddingLeft`, `textStyle` |
| MarkedText | `maxHeight` | `as`, `children`, `fontWeight`, `highlightColor`, `variant` | `fontSize`, `mb`, `mr`, `textStyle` |
| Menu | `icon`, `iconSpacing`, `offset`, `placement` | `items`, `showChevron`, `title` | `children`, `fontWeightTitle`, `menuColor`, `value` |
| MissingImage | — | `children` | — |
| MobileOnlyAccordion | — | — | — |
| Modal | — | `children`, `disableBodyPadding`, `isOpen`, `onClose`, `primaryActionButton`, `secondaryActionButton`, `size`, `title`, `variant` | `motionPreset` |
| ModalCloseButton | — | — | `_hover`, `alignSelf`, `bg`, `children`, `color`, `fontSize`, `height`, `ml`, `mr`, `onMouseEnter`, `padding`, `rounded`, `transform`, `transitionDelay`, `transitionDuration`, `transitionProperty`, `transitionTimingFunction`, `visibility`, `width` |
| MotoScout24AppLogo | — | — | — |
| OrderedList | — | — | `children` |
| Pagination | — | `children`, `currentPage`, `marginTop`, `onChange`, `totalPages` | `marginBottom` |
| Popover | — | — | `autoFocus`, `children`, `closeOnBlur`, `closeOnInteractOutside`, `content`, `contentPadding`, `contentPosition`, `gutter`, `isOpen`, `maxWidth`, `onClose`, `onOpen`, `open`, `placement`, `showArrow`, `size`, `trigger` |
| Progress | — | — | `children`, `current`, `label`, `max` |
| Radio | `key`, `label` | `children`, `value` | `items`, `name`, `onChange`, `orientation`, `ref`, `size`, `variant` |
| RadioList | — | — | `defaultValue`, `name`, `onChange`, `options` |
| RangeFilterInput | — | — | — |
| RangeFilterInputWithSlider | — | `rangeSliderScale` | `children`, `from`, `max`, `min`, `onBlur`, `onChange`, `to`, `unit` |
| RangeSlider | — | — | — |
| Rating | — | `children`, `rating`, `size` | — |
| SearchableList | `EmptyQueryPlaceholder`, `key`, `listAriaLabel`, `listRef` | `NoResults`, `listItems`, `listOptions`, `searchFieldOptions` | `children` |
| SearchField | `name`, `onBlur`, `placeholder`, `searchQuery`, `setSearchQuery` | — | — |
| Section | `children`, `image`, `maxImgW`, `text`, `title`, `variant` | — | — |
| Select | — | `children`, `options`, `placeholder`, `size` | `borderRightRadius`, `isDisabled`, `isInvalid`, `name`, `onChange`, `value` |
| SelectMenu | `children`, `menuColor` | `leftIcon`, `options`, `title`, `value`, `withIndicator` | `showChevron` |
| Skeleton | `h` | `children` | `height`, `isLoaded`, `width` |
| Spinner | — | `children`, `size` | — |
| Switch | — | `id`, `isChecked`, `label`, `onChange` | `children`, `isDisabled` |
| Tab | — | `children`, `key` | `isDisabled`, `marginX`, `value` |
| Table | — | — | `children`, `colorScheme`, `css`, `h`, `mb`, `size`, `striped`, `w` |
| TabList | — | `borderBottom`, `children` | `px` |
| TabPanel | — | — | `children`, `key`, `value` |
| TabPanels | — | — | `children` |
| Tabs | `index` | `children`, `onChange` | `defaultValue`, `lazyMount`, `value`, `variant` |
| Tbody | — | — | `children` |
| Td | — | — | `borderBottom`, `children`, `colSpan`, `data-label`, `fontWeight`, `isNumeric`, `pb`, `pt`, `py`, `w`, `width` |
| Text | `_hover`, `align`, `border`, `borderRadius`, `borderTopLeftRadius`, `data-testid`, `flex`, `flexDirection`, `fontStyle`, `letterSpacing`, `marginX`, `marginY`, `minH`, `minWidth`, `mx`, `paddingBottom`, `pl`, `px`, `sx`, `verticalAlign` | `alignItems`, `as`, `backgroundColor`, `borderColor`, `children`, `color`, `display`, `flexGrow`, `flexShrink`, `fontSize`, `fontWeight`, `isTruncated`, `key`, `lineHeight`, `marginBottom`, `marginLeft`, `marginRight`, `marginTop`, `maxW`, `maxWidth`, `mb`, `ml`, `mr`, `mt`, `my`, `noOfLines`, `overflow`, `paddingRight`, `paddingX`, `paddingY`, `position`, `pr`, `pt`, `py`, `textAlign`, `textColor`, `textDecoration`, `textOverflow`, `textStyle`, `whiteSpace`, `width`, `wordBreak` | `alignSelf`, `aria-label`, `borderRight`, `bottom`, `cursor`, `height`, `id`, `left`, `margin`, `minHeight`, `onClick`, `overflowWrap`, `padding`, `pos`, `right`, `textDecorationColor`, `textDecorationThickness`, `textTransform`, `title`, `top`, `transform`, `visibility`, `w`, `zIndex` |
| Textarea | — | `children`, `placeholder`, `rows` | `textStyle` |
| Tfoot | — | — | `children` |
| Th | — | — | `children`, `isNumeric` |
| Thead | — | — | `children` |
| TimePicker | — | — | `children` |
| Tooltip | — | — | `children`, `label`, `placement` |
| TopListingBadge | — | `children` | `aspectRatio` |
| TopRightToast | — | — | — |
| TopToast | — | — | — |
| Tr | — | — | `children`, `key` |
| UnorderedList | — | `children` | `listStylePosition`, `paddingLeft`, `variant` |


