# Listings-web inventory

Generated from the public API of `@smg-automotive/components` v`25.24.0`.

The inventory covers 128 public JSX components across 1280 source files. Icons and non-JSX exports are excluded. Props explicitly declared by components-pkg are custom; all other accepted props are classified as Chakra surface. Explicit JSX props and statically resolved spread keys are counted once per component instance; `children` is counted when non-empty JSX children are supplied. Spreads are unresolved when their value cannot be traced to an object literal or initialized local object.

| Component | JSX uses | Custom props by usage | Chakra-surface props by usage | Resolved spreads | Unresolved spreads |
| --- | ---: | --- | --- | ---: | ---: |
| Accordion | 2 | — | `children` (2), `allowMultiple` (1), `allowToggle` (1), `onChange` (1), `variant` (1) | 0 | 0 |
| AccordionButton | 6 | `leftIcon` (6) | `children` (6), `color` (1), `paddingBottom` (1), `paddingTop` (1) | 0 | 0 |
| AccordionItem | 6 | — | `children` (6) | 0 | 0 |
| AccordionPanel | 6 | — | `children` (6) | 0 | 0 |
| Alert | 14 | `description` (14), `type` (10), `title` (7), `icon` (6), `link` (6), `dismissible` (1), `onDismiss` (1) | `children` (7) | 0 | 0 |
| AppLayout | 0 | — | — | 0 | 0 |
| AppLayoutContent | 0 | — | — | 0 | 0 |
| AppLayoutFooter | 0 | — | — | 0 | 0 |
| AppLayoutHeader | 0 | — | — | 0 | 0 |
| ArticleTeaser | 1 | `imageUrl` (1) | `key` (1) | 0 | 1 |
| AspectRatio | 18 | — | `children` (18), `ratio` (18), `width` (12), `overflow` (9), `borderRadius` (8), `height` (7), `cursor` (2), `maxH` (2), `w` (2), `bg` (1), `borderBottomRadius` (1), `data-testid` (1), `flexShrink` (1), `h` (1), `maxW` (1), `minW` (1), `onClick` (1) | 0 | 0 |
| AutoScout24AppLogo | 0 | — | — | 0 | 0 |
| Badge | 5 | `text` (5) | `children` (5) | 0 | 0 |
| Box | 284 | `onClick` (1) | `children` (266), `as` (58), `position` (41), `width` (39), `display` (35), `w` (33), `h` (32), `mt` (26), `mb` (23), `paddingX` (20), `top` (19), `borderRadius` (18), `color` (17), `alignItems` (16), `backgroundColor` (16), `height` (16), `bg` (15), `justifyContent` (15), `onClick` (14), `sx` (14), `href` (13), `key` (13), `left` (13), `paddingY` (13), `bottom` (12), `textStyle` (12), `borderColor` (11), `overflow` (10), `ref` (10), `id` (8), `marginTop` (8), `maxW` (8), `my` (8), `spacing` (8), `marginLeft` (7), `ml` (7), `gap` (6), `marginX` (6), `overflowY` (6), `pt` (6), `right` (6), `target` (6), `cursor` (5), `direction` (5), `flexDirection` (5), `flexShrink` (5), `maxWidth` (5), `mr` (5), `pointerEvents` (5), `py` (5), `rel` (5), `zIndex` (5), `borderTopWidth` (4), `borderWidth` (4), `flex` (4), `marginRight` (4), `order` (4), `textAlign` (4), `variant` (4), `_hover` (3), `alignSelf` (3), `aria-label` (3), `border` (3), `borderBottomWidth` (3), `borderTopColor` (3), `data-testid` (3), `flexWrap` (3), `marginBottom` (3), `minWidth` (3), `opacity` (3), `p` (3), `prefetch` (3), `px` (3), `transition` (3), `whiteSpace` (3), `aria-current` (2), `background` (2), `borderBottom` (2), `boxShadow` (2), `gridTemplateColumns` (2), `maxHeight` (2), `minH` (2), `minW` (2), `mx` (2), `paddingLeft` (2), `paddingTop` (2), `pl` (2), `role` (2), `shadow` (2), `src` (2), `tabIndex` (2), `title` (2), `transform` (2), `type` (2), `_active` (1), `_groupHover` (1), `align` (1), `allow` (1), `allowFullScreen` (1), `aspectRatio` (1), `bgGradient` (1), `borderBottomColor` (1), `borderBottomRadius` (1), `borderLeft` (1), `borderTopLeftRadius` (1), `borderTopRightRadius` (1), `borderY` (1), `columns` (1), `dangerouslySetInnerHTML` (1), `fontWeight` (1), `frameBorder` (1), `gridRow` (1), `gridTemplateRows` (1), `isolation` (1), `listStylePosition` (1), `listStyleType` (1), `marginY` (1), `maxH` (1), `minHeight` (1), `onMouseEnter` (1), `onMouseLeave` (1), `onScroll` (1), `overflowX` (1), `padding` (1), `pb` (1), `referrerPolicy` (1), `rounded` (1), `scrollBehavior` (1), `templateColumns` (1), `textOverflow` (1), `verticalAlign` (1) | 0 | 7 |
| BreadcrumbLink | 7 | — | `children` (7), `as` (4), `href` (4), `minWidth` (2) | 0 | 0 |
| Breadcrumbs | 3 | — | `children` (3), `mb` (1) | 0 | 0 |
| BreadcrumbsItem | 7 | — | `children` (7) | 0 | 0 |
| Button | 64 | `children` (54), `onClick` (52), `variant` (46), `as` (29), `href` (25), `size` (23), `leftIcon` (22), `ariaLabel` (15), `icon` (15), `type` (7), `rel` (5), `isDisabled` (3), `rightIcon` (3), `isExternal` (1) | `width` (27), `w` (18), `whiteSpace` (6), `h` (5), `mt` (4), `paddingX` (4), `display` (3), `p` (3), `px` (3), `wordBreak` (3), `_active` (2), `alignItems` (2), `aria-label` (2), `disabled` (2), `flexShrink` (2), `key` (2), `marginLeft` (2), `minWidth` (2), `ml` (2), `mr` (2), `my` (2), `zIndex` (2), `_hover` (1), `alignSelf` (1), `aria-busy` (1), `aria-controls` (1), `bottom` (1), `colorScheme` (1), `flex` (1), `fontWeight` (1), `height` (1), `justifyContent` (1), `maxW` (1), `mb` (1), `minW` (1), `mx` (1), `pointerEvents` (1), `position` (1) | 0 | 18 |
| Card | 0 | — | — | 0 | 0 |
| CardBody | 0 | — | — | 0 | 0 |
| CardFooter | 0 | — | — | 0 | 0 |
| CardHeader | 0 | — | — | 0 | 0 |
| Carousel | 8 | `children` (8), `slidesPerView` (5), `loop` (3), `onSlideSelect` (3), `slidesToScroll` (3), `startIndex` (3), `fullScreen` (1), `onSlideClick` (1) | — | 0 | 0 |
| Center | 1 | — | `children` (1) | 0 | 0 |
| Checkbox | 7 | `isChecked` (7), `label` (7), `name` (2), `onChange` (2), `isInvalid` (1) | `children` (6) | 0 | 5 |
| CheckboxFilter | 15 | `items` (15), `language` (15), `onApply` (15), `numberOfColumnsOnDesktop` (5) | — | 0 | 0 |
| CheckboxGroup | 0 | — | — | 0 | 0 |
| Chip | 1 | `isActive` (1), `onClick` (1) | `children` (1) | 0 | 0 |
| Collapse | 3 | — | `children` (3), `in` (3) | 0 | 0 |
| ColorPicker | 0 | — | — | 0 | 0 |
| Count | 6 | `count` (6), `ariaLabel` (3), `variant` (1) | `children` (2) | 0 | 0 |
| DatePicker | 1 | `min` (1), `size` (1) | `children` (1), `data-testid` (1) | 0 | 1 |
| DevOverlay | 1 | `activeTheme` (1), `displayTranslationKeys` (1), `hideDevOverlay` (1), `toggleTheme` (1), `toggleTranslation` (1), `variables` (1) | — | 0 | 0 |
| DiscreteSlider | 0 | — | — | 0 | 0 |
| Divider | 39 | — | `children` (19), `borderColor` (9), `orientation` (5), `mb` (4), `my` (4), `display` (3), `mx` (2), `height` (1), `marginLeft` (1), `marginRight` (1), `mt` (1), `position` (1), `right` (1), `top` (1), `w` (1) | 0 | 0 |
| Drawer | 0 | — | — | 0 | 0 |
| DrawerBody | 0 | — | — | 0 | 0 |
| DrawerContent | 0 | — | — | 0 | 0 |
| DrawerOverlay | 0 | — | — | 0 | 0 |
| EnergyLabel | 1 | `efficiency` (1) | — | 0 | 0 |
| ErrorPage | 3 | `language` (3), `statusCode` (3) | `children` (3) | 0 | 0 |
| FilterHeading | 2 | `isApplied` (1), `label` (1), `language` (1), `numberOfAppliedFilters` (1), `onResetFilter` (1) | `children` (2), `appliedLabel` (1), `displayValue` (1), `triggerDisplayType` (1), `triggerHeight` (1) | 0 | 2 |
| Flex | 138 | — | `children` (138), `alignItems` (57), `justifyContent` (48), `direction` (46), `width` (20), `flexDirection` (18), `w` (18), `position` (13), `mt` (12), `as` (10), `height` (10), `mb` (10), `marginTop` (8), `maxWidth` (8), `py` (8), `backgroundColor` (7), `borderColor` (7), `flexWrap` (7), `zIndex` (7), `color` (6), `h` (6), `key` (6), `top` (6), `flex` (5), `paddingY` (5), `left` (4), `marginBottom` (4), `paddingX` (4), `align` (3), `background` (3), `borderRadius` (3), `borderTopWidth` (3), `borderWidth` (3), `data-testid` (3), `flexShrink` (3), `listStyleType` (3), `marginX` (3), `p` (3), `padding` (3), `px` (3), `wrap` (3), `borderBottomWidth` (2), `borderTopColor` (2), `display` (2), `flexGrow` (2), `gridGap` (2), `maxW` (2), `right` (2), `textStyle` (2), `_groupHover` (1), `alignContent` (1), `aria-current` (1), `aria-label` (1), `backgroundImage` (1), `backgroundPosition` (1), `backgroundRepeat` (1), `backgroundSize` (1), `border` (1), `borderBottom` (1), `borderBottomColor` (1), `borderBottomRadius` (1), `bottom` (1), `inset` (1), `justify` (1), `marginRight` (1), `marginY` (1), `maxH` (1), `minHeight` (1), `minW` (1), `mx` (1), `my` (1), `onClick` (1), `opacity` (1), `order` (1), `overflow` (1), `paddingBottom` (1), `paddingLeft` (1), `paddingTop` (1), `pb` (1), `pl` (1), `pointerEvents` (1), `pr` (1), `sx` (1), `textAlign` (1), `transform` (1), `transition` (1) | 0 | 0 |
| FocusedHeader | 0 | — | — | 0 | 0 |
| Footer | 1 | `brand` (1), `environment` (1), `language` (1), `project` (1), `useAbsoluteUrls` (1) | — | 0 | 0 |
| FormControl | 30 | `errorMessage` (30), `id` (30), `label` (24), `isRequired` (9), `hint` (1) | `children` (30) | 0 | 0 |
| FormControlSection | 0 | — | — | 0 | 0 |
| FormLabel | 2 | — | `children` (2) | 0 | 0 |
| FullHeight | 0 | — | — | 0 | 0 |
| GalleryHeader | 1 | `currentSlide` (1), `language` (1), `onClose` (1), `slidesCount` (1) | `children` (1) | 0 | 0 |
| Grid | 121 | — | `children` (121), `templateColumns` (97), `gap` (69), `rowGap` (25), `alignItems` (16), `columnGap` (15), `templateAreas` (15), `py` (9), `gridTemplateColumns` (8), `mt` (8), `as` (6), `position` (6), `borderBottomColor` (5), `borderBottomWidth` (5), `marginTop` (5), `mb` (5), `templateRows` (5), `marginBottom` (4), `maxWidth` (4), `overflowX` (4), `ref` (4), `width` (4), `borderColor` (3), `borderTopColor` (3), `borderTopWidth` (3), `data-testid` (3), `my` (3), `top` (3), `backgroundColor` (2), `border` (2), `borderRadius` (2), `gridGap` (2), `height` (2), `mr` (2), `opacity` (2), `p` (2), `pr` (2), `px` (2), `sx` (2), `transform` (2), `transition` (2), `zIndex` (2), `alignContent` (1), `aria-hidden` (1), `aria-label` (1), `aria-labelledby` (1), `borderBottom` (1), `borderWidth` (1), `color` (1), `display` (1), `flexDirection` (1), `flexWrap` (1), `gridAutoRows` (1), `gridColumnGap` (1), `gridRowGap` (1), `gridTemplateAreas` (1), `id` (1), `justifyContent` (1), `justifyItems` (1), `key` (1), `ml` (1), `onSubmit` (1), `order` (1), `overflow` (1), `padding` (1), `paddingTop` (1), `paddingY` (1), `pb` (1), `pt` (1), `role` (1), `rounded` (1), `w` (1), `wordBreak` (1) | 0 | 4 |
| GridItem | 134 | — | `children` (131), `area` (59), `display` (30), `as` (22), `gridArea` (17), `alignItems` (16), `justifyContent` (14), `textStyle` (13), `colSpan` (11), `key` (9), `paddingRight` (9), `color` (8), `mb` (8), `borderColor` (7), `borderRightWidth` (6), `gap` (6), `gridColumn` (6), `order` (6), `w` (6), `backgroundColor` (5), `flexDirection` (5), `gridRow` (5), `mt` (5), `paddingX` (5), `position` (5), `h` (4), `onClick` (4), `aria-label` (3), `borderRadius` (3), `gridTemplateColumns` (3), `left` (3), `maxW` (3), `my` (3), `paddingY` (3), `wordBreak` (3), `zIndex` (3), `alignSelf` (2), `borderBottomWidth` (2), `height` (2), `marginBottom` (2), `marginLeft` (2), `paddingBottom` (2), `paddingTop` (2), `ref` (2), `spacing` (2), `_groupHover` (1), `alt` (1), `aria-labelledby` (1), `borderLeftWidth` (1), `borderTopWidth` (1), `borderWidth` (1), `colStart` (1), `data-testid` (1), `direction` (1), `flexWrap` (1), `gridAutoRows` (1), `gridColumnEnd` (1), `gridColumnStart` (1), `href` (1), `id` (1), `isExternal` (1), `justifyItems` (1), `justifySelf` (1), `margin` (1), `marginRight` (1), `marginTop` (1), `minH` (1), `minW` (1), `mx` (1), `noOfLines` (1), `overflow` (1), `p` (1), `padding` (1), `pr` (1), `rightIcon` (1), `rowSpan` (1), `src` (1), `sx` (1), `whiteSpace` (1) | 0 | 1 |
| H1 | 18 | — | `children` (18), `mb` (5), `textStyle` (3), `color` (2), `alignItems` (1), `display` (1), `flexWrap` (1), `marginRight` (1), `my` (1), `px` (1), `textAlign` (1), `w` (1), `wordBreak` (1) | 0 | 0 |
| H2 | 39 | — | `children` (39), `mb` (13), `textStyle` (12), `my` (10), `mt` (9), `marginBottom` (2), `color` (1), `left` (1), `marginRight` (1), `maxWidth` (1), `position` (1), `py` (1), `sx` (1), `textAlign` (1) | 0 | 1 |
| H3 | 7 | — | `children` (7), `textStyle` (2), `marginRight` (1), `marginY` (1), `order` (1), `wordBreak` (1) | 0 | 0 |
| H4 | 3 | — | `children` (3), `color` (3), `mb` (3) | 0 | 0 |
| H5 | 0 | — | — | 0 | 0 |
| H6 | 0 | — | — | 0 | 0 |
| HeaderNavigation | 1 | `brand` (1), `comparisonItemIds` (1), `entitlements` (1), `environment` (1), `hasNotification` (1), `language` (1), `onLogin` (1), `onLogout` (1), `project` (1), `selectTenant` (1), `showTenantSelection` (1), `trackEvent` (1), `useAbsoluteUrls` (1), `user` (1) | — | 0 | 0 |
| Hide | 33 | — | `children` (33), `above` (31), `display` (4), `mt` (3), `alignItems` (2), `backgroundColor` (2), `below` (2), `bottom` (2), `position` (2), `width` (2), `zIndex` (2), `as` (1), `height` (1), `justifyContent` (1), `left` (1), `minWidth` (1), `mr` (1), `mx` (1), `px` (1), `textStyle` (1), `w` (1) | 0 | 0 |
| HighlightedText | 0 | — | — | 0 | 0 |
| Input | 18 | `size` (15), `name` (4), `placeholder` (4), `value` (4), `isClearable` (3), `debounce` (2), `onBlur` (2), `onChange` (2), `onFocus` (2), `setInputValue` (2), `autoFocus` (1), `endElement` (1), `icon` (1), `isDisabled` (1), `onKeyDown` (1) | `children` (17), `ref` (1) | 0 | 14 |
| InsertionLayout | 0 | — | — | 0 | 0 |
| LayoutWithVehicleReference | 2 | `backLink` (2), `vehicle` (2) | `children` (2) | 0 | 0 |
| Link | 74 | `href` (58), `onClick` (48), `leftIcon` (28), `prefetch` (13), `rightIcon` (9), `disabled` (6), `replace` (1) | `children` (74), `as` (42), `rel` (16), `width` (14), `isExternal` (11), `target` (10), `key` (7), `textStyle` (7), `aria-label` (5), `flexShrink` (5), `fontSize` (4), `color` (3), `fontWeight` (3), `noOfLines` (3), `alignItems` (2), `display` (2), `marginLeft` (2), `ml` (2), `mx` (2), `textAlign` (2), `type` (2), `wordBreak` (2), `alignSelf` (1), `justifyContent` (1), `marginTop` (1), `minWidth` (1), `mt` (1), `order` (1), `overflowX` (1), `paddingX` (1), `paddingY` (1), `style` (1), `textColor` (1), `textDecoration` (1), `textOverflow` (1), `whiteSpace` (1), `zIndex` (1) | 0 | 13 |
| LinkBox | 0 | — | — | 0 | 0 |
| LinkOverlay | 1 | — | `as` (1), `children` (1), `href` (1), `onClick` (1), `prefetch` (1), `rel` (1), `target` (1) | 0 | 0 |
| List | 19 | `children` (19) | `aria-label` (2), `mt` (1), `my` (1), `onClick` (1), `order` (1), `pt` (1), `spacing` (1) | 0 | 0 |
| ListItem | 17 | — | `children` (17), `key` (7), `display` (2), `mb` (2), `sx` (1) | 0 | 0 |
| MarkedText | 6 | `variant` (6), `highlightColor` (4) | `children` (6), `as` (1), `fontWeight` (1), `maxHeight` (1) | 0 | 0 |
| Menu | 1 | `icon` (1), `iconSpacing` (1), `items` (1), `offset` (1), `placement` (1), `showChevron` (1), `title` (1) | — | 0 | 0 |
| MissingImage | 3 | — | `children` (1) | 0 | 0 |
| Modal | 12 | `size` (11), `title` (6), `primaryActionButton` (4), `secondaryActionButton` (2), `disableBodyPadding` (1), `variant` (1) | `children` (12), `isOpen` (12), `onClose` (12) | 0 | 0 |
| ModalCloseButton | 0 | — | — | 0 | 0 |
| ModalFilter | 5 | `actionButton` (5), `children` (5), `onModalClose` (5), `displayValue` (4), `isApplied` (4), `label` (4), `language` (4), `onResetFilter` (4), `onModalOpen` (3), `showResetButton` (3), `isDisabled` (2), `paddingX` (2) | `backgroundColor` (2) | 0 | 1 |
| MotoScout24AppLogo | 0 | — | — | 0 | 0 |
| OpenFilterButton | 5 | `displayValue` (5), `isApplied` (5), `label` (5), `onClick` (5), `onResetFilter` (5), `resetButtonAriaLabel` (5), `height` (3), `showResetButton` (2), `variant` (2), `isDisabled` (1) | `children` (5) | 0 | 0 |
| OrderedList | 0 | — | — | 0 | 0 |
| PageLayout | 19 | `header` (19), `maxContentWidth` (19), `skyScraperAd` (6), `footer` (1), `heroAd` (1) | `children` (19) | 0 | 0 |
| Pagination | 3 | `currentPage` (3), `onChange` (3), `totalPages` (3) | `marginTop` (3), `children` (1) | 0 | 0 |
| Popover | 0 | — | — | 0 | 0 |
| PopoverFilter | 2 | `actionButton` (2), `children` (2), `onPopoverClose` (2), `onPopoverOpen` (2), `displayValue` (1), `header` (1), `isApplied` (1), `label` (1), `language` (1), `onResetFilter` (1), `showCallToActionButton` (1), `triggerHeight` (1) | — | 0 | 1 |
| Progress | 0 | — | — | 0 | 0 |
| Radio | 11 | `label` (11), `value` (11) | `children` (10), `key` (1) | 0 | 0 |
| RadioGroup | 6 | — | `children` (6), `onChange` (6), `value` (6), `marginBottom` (2) | 0 | 0 |
| RadioList | 0 | — | — | 0 | 0 |
| RangeFilterInput | 0 | — | — | 0 | 0 |
| RangeFilterInputWithSlider | 18 | `rangeSliderScale` (18) | — | 0 | 18 |
| RangeSlider | 0 | — | — | 0 | 0 |
| Rating | 2 | `rating` (2), `size` (2) | `children` (2) | 0 | 0 |
| SearchableList | 3 | `listItems` (3), `searchFieldOptions` (3), `EmptyQueryPlaceholder` (2), `NoResults` (2), `listAriaLabel` (1), `listOptions` (1), `listRef` (1) | `key` (2) | 0 | 0 |
| SearchField | 1 | `name` (1), `onBlur` (1), `placeholder` (1), `searchQuery` (1), `setSearchQuery` (1) | — | 0 | 0 |
| Section | 1 | `image` (1), `maxImgW` (1), `text` (1), `title` (1), `variant` (1) | `children` (1) | 0 | 0 |
| Select | 2 | `options` (2), `size` (2) | `placeholder` (2), `children` (1) | 0 | 2 |
| SelectMenu | 1 | `leftIcon` (1), `menuColor` (1), `options` (1), `title` (1), `value` (1), `withIndicator` (1) | `children` (1) | 0 | 0 |
| Show | 46 | `showDisplay` (6) | `children` (46), `above` (44), `flexDirection` (3), `as` (2), `below` (2), `justifyContent` (2), `mt` (2), `alignItems` (1), `color` (1), `display` (1), `flex` (1), `fontSize` (1), `gap` (1), `gridTemplateAreas` (1), `gridTemplateColumns` (1), `left` (1), `marginRight` (1), `marginX` (1), `mb` (1), `ml` (1), `order` (1), `position` (1), `textAlign` (1), `textStyle` (1), `top` (1), `transform` (1), `width` (1) | 0 | 0 |
| SimpleGrid | 18 | — | `children` (18), `columns` (15), `spacing` (10), `spacingY` (7), `spacingX` (5), `alignItems` (1), `key` (1) | 0 | 0 |
| SimpleHeader | 0 | — | — | 0 | 0 |
| Skeleton | 3 | — | `children` (3), `h` (2) | 0 | 0 |
| Slide | 0 | — | — | 0 | 0 |
| Spinner | 10 | `size` (3) | `children` (4) | 0 | 0 |
| Stack | 47 | — | `children` (47), `spacing` (44), `direction` (21), `align` (6), `key` (5), `alignItems` (4), `data-testid` (1), `marginTop` (1) | 0 | 0 |
| Switch | 3 | `id` (3), `label` (3) | `isChecked` (3), `onChange` (3) | 0 | 0 |
| Tab | 1 | — | `children` (1), `key` (1) | 0 | 0 |
| Table | 0 | — | — | 0 | 0 |
| TableCaption | 0 | — | — | 0 | 0 |
| TableContainer | 0 | — | — | 0 | 0 |
| TabList | 1 | — | `borderBottom` (1), `children` (1) | 0 | 0 |
| TabPanel | 0 | — | — | 0 | 0 |
| TabPanels | 0 | — | — | 0 | 0 |
| Tabs | 1 | — | `children` (1), `index` (1), `onChange` (1) | 0 | 0 |
| Tbody | 0 | — | — | 0 | 0 |
| Td | 0 | — | — | 0 | 0 |
| TenantSelection | 0 | — | — | 0 | 0 |
| Text | 225 | — | `children` (225), `textStyle` (144), `color` (67), `as` (31), `fontWeight` (24), `display` (15), `mt` (12), `noOfLines` (12), `py` (10), `mb` (9), `wordBreak` (9), `ml` (8), `mr` (8), `textAlign` (8), `align` (6), `px` (6), `marginBottom` (5), `mx` (5), `marginTop` (4), `flex` (3), `fontSize` (3), `key` (3), `marginRight` (3), `marginX` (3), `my` (3), `overflow` (3), `paddingX` (3), `paddingY` (3), `position` (3), `pt` (3), `textOverflow` (3), `verticalAlign` (3), `whiteSpace` (3), `_hover` (2), `alignItems` (2), `backgroundColor` (2), `flexGrow` (2), `flexShrink` (2), `marginLeft` (2), `paddingBottom` (2), `textColor` (2), `width` (2), `border` (1), `borderColor` (1), `borderRadius` (1), `borderTopLeftRadius` (1), `data-testid` (1), `flexDirection` (1), `fontStyle` (1), `isTruncated` (1), `letterSpacing` (1), `lineHeight` (1), `marginY` (1), `maxW` (1), `maxWidth` (1), `minH` (1), `minWidth` (1), `paddingRight` (1), `pl` (1), `pr` (1), `sx` (1), `textDecoration` (1) | 0 | 0 |
| Textarea | 5 | `rows` (5), `placeholder` (1) | `children` (5) | 0 | 5 |
| Tfoot | 0 | — | — | 0 | 0 |
| Th | 0 | — | — | 0 | 0 |
| Thead | 0 | — | — | 0 | 0 |
| ThemeProvider | 3 | `theme` (3) | `children` (3) | 0 | 0 |
| TimePicker | 0 | — | — | 0 | 0 |
| Tooltip | 0 | — | — | 0 | 0 |
| TopListingBadge | 1 | — | `children` (1) | 0 | 0 |
| TopVehicleSharedBadge | 1 | `aspectRatio` (1) | `children` (1), `key` (1) | 0 | 0 |
| Tr | 0 | — | — | 0 | 0 |
| TwoColumnsLayout | 2 | `left` (2), `right` (2), `title` (1) | — | 0 | 0 |
| UnorderedList | 2 | — | `children` (2) | 0 | 0 |
| VehicleReference | 1 | `image` (1), `templateColumns` (1) | `children` (1) | 0 | 1 |

Regenerate with `npm run components:inventory`.
