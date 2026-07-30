# Seller-web inventory

Generated from the public API of `@smg-automotive/components` v`25.30.0-chakra-v3.1`.

The inventory covers 130 public JSX components across 2978 source files. Icons and non-JSX exports are excluded. Props explicitly declared by components-pkg are custom; all other accepted props are classified as Chakra surface. Explicit JSX props and statically resolved spread keys are counted once per component instance; `children` is counted when non-empty JSX children are supplied. Spreads are unresolved when their value cannot be traced to an object literal or initialized local object.

| Component | JSX uses | Custom props by usage | Chakra-surface props by usage | Resolved spreads | Unresolved spreads |
| --- | ---: | --- | --- | ---: | ---: |
| Accordion | 27 | `variant` (2) | `children` (27), `multiple` (9), `borderRadius` (4), `defaultValue` (4), `onValueChange` (4), `value` (4), `background` (3), `boxShadow` (3), `collapsible` (3), `bg` (2), `borderColor` (1), `borderWidth` (1), `overflow` (1) | 0 | 0 |
| AccordionButton | 11 | `leftIcon` (3) | `children` (11), `_hover` (3), `_expanded` (2), `alignItems` (2), `display` (2), `maxW` (2), `onClick` (2), `paddingX` (2), `py` (2), `w` (2), `backgroundColor` (1), `color` (1), `fontWeight` (1), `px` (1), `textStyle` (1) | 0 | 0 |
| AccordionItem | 11 | `variant` (1) | `children` (11), `value` (11), `_last` (8), `borderColor` (2), `key` (2), `backgroundColor` (1), `border` (1), `borderBottom` (1), `borderTopColor` (1), `borderTopStyle` (1), `borderTopWidth` (1) | 0 | 1 |
| AccordionPanel | 11 | — | `children` (11), `border` (7), `backgroundColor` (2), `padding` (2), `paddingInline` (2), `paddingTop` (2), `pb` (2), `px` (2), `_expanded` (1), `paddingX` (1), `pt` (1), `textStyle` (1) | 0 | 0 |
| Alert | 51 | `description` (51), `type` (51), `icon` (46), `title` (14), `dismissible` (9), `link` (7), `onDismiss` (5) | `children` (32) | 0 | 0 |
| AppLayout | 1 | `children` (1) | — | 0 | 0 |
| AppLayoutContent | 1 | — | `children` (1) | 0 | 0 |
| AppLayoutFooter | 1 | — | `children` (1) | 0 | 0 |
| AppLayoutHeader | 1 | — | `children` (1) | 0 | 0 |
| ArticleTeaser | 0 | — | — | 0 | 0 |
| AspectRatio | 30 | — | `children` (30), `ratio` (30), `width` (18), `overflow` (14), `height` (8), `borderRadius` (5), `minW` (3), `opacity` (3), `cursor` (2), `flexShrink` (2), `key` (2), `maxW` (2), `position` (2), `margin` (1), `marginTop` (1), `mb` (1), `mr` (1), `mx` (1), `w` (1) | 0 | 0 |
| AutoScout24AppLogo | 0 | — | — | 0 | 0 |
| Avatar | 0 | — | — | 0 | 0 |
| Badge | 8 | `text` (8) | `children` (5) | 0 | 0 |
| Box | 922 | `as` (123), `ref` (19), `spacing` (17), `href` (1), `textColor` (1) | `children` (901), `width` (147), `display` (145), `borderRadius` (126), `padding` (88), `alignItems` (79), `backgroundColor` (78), `borderColor` (70), `mb` (70), `position` (70), `background` (69), `height` (67), `bg` (58), `justifyContent` (58), `fontWeight` (49), `gap` (49), `boxShadow` (46), `flexDirection` (45), `paddingX` (45), `paddingY` (41), `mt` (40), `marginBottom` (39), `w` (38), `textAlign` (32), `maxWidth` (28), `borderWidth` (27), `margin` (26), `color` (25), `flex` (25), `border` (24), `h` (23), `ml` (23), `overflow` (23), `cursor` (22), `p` (22), `paddingBottom` (20), `top` (20), `key` (18), `marginRight` (18), `py` (18), `maxW` (17), `onClick` (17), `left` (15), `flexShrink` (14), `mr` (14), `opacity` (14), `px` (14), `borderBottom` (13), `borderStyle` (13), `textStyle` (13), `zIndex` (13), `marginX` (12), `my` (12), `paddingTop` (12), `data-testid` (11), `minW` (11), `paddingLeft` (11), `right` (11), `marginTop` (10), `pb` (10), `alignSelf` (9), `bottom` (9), `_groupHover` (8), `aria-label` (8), `m` (8), `pt` (8), `role` (8), `borderBottomWidth` (7), `borderTop` (7), `direction` (7), `inset` (7), `listStyleType` (7), `minWidth` (7), `overflowY` (7), `_last` (6), `borderBottomColor` (6), `borderRight` (6), `flexBasis` (6), `rounded` (6), `whiteSpace` (6), `fontSize` (5), `maxH` (5), `mx` (5), `pl` (5), `pointerEvents` (5), `type` (5), `visibility` (5), `borderRightColor` (4), `className` (4), `marginY` (4), `minH` (4), `minHeight` (4), `order` (4), `transform` (4), `wordBreak` (4), `_hover` (3), `alt` (3), `aria-current` (3), `bgColor` (3), `borderBottomStyle` (3), `borderLeft` (3), `boxSize` (3), `flexWrap` (3), `justify` (3), `outline` (3), `outlineColor` (3), `paddingInline` (3), `pr` (3), `src` (3), `textOverflow` (3), `transition` (3), `align` (2), `aria-labelledby` (2), `aria-pressed` (2), `borderTopRadius` (2), `columns` (2), `fill` (2), `gridTemplateColumns` (2), `id` (2), `onChange` (2), `onPointerCancel` (2), `onPointerDown` (2), `onPointerMove` (2), `onPointerUp` (2), `overflowX` (2), `paddingRight` (2), `shadow` (2), `textTransform` (2), `userSelect` (2), `viewBox` (2), `xmlns` (2), `_focusVisible` (1), `aria-controls` (1), `aria-expanded` (1), `aria-hidden` (1), `aria-valuemax` (1), `aria-valuemin` (1), `aria-valuenow` (1), `aria-valuetext` (1), `aspectRatio` (1), `backdropFilter` (1), `backgroundImage` (1), `backgroundRepeat` (1), `backgroundSize` (1), `borderLeftColor` (1), `borderTopWidth` (1), `columnGap` (1), `css` (1), `dangerouslySetInnerHTML` (1), `flexGrow` (1), `gridArea` (1), `gridAutoFlow` (1), `gridTemplateRows` (1), `hideBelow` (1), `hideFrom` (1), `listStylePosition` (1), `marginInline` (1), `maxHeight` (1), `objectFit` (1), `onKeyDown` (1), `onMouseEnter` (1), `onMouseLeave` (1), `overflowWrap` (1), `rowGap` (1), `style` (1), `tabIndex` (1), `target` (1), `textDecoration` (1) | 0 | 26 |
| BreadcrumbLink | 0 | — | — | 0 | 0 |
| Breadcrumbs | 3 | — | `children` (3) | 0 | 0 |
| BreadcrumbsItem | 17 | — | `children` (17) | 0 | 0 |
| Button | 143 | `children` (143), `variant` (111), `size` (108), `onClick` (74), `as` (58), `leftIcon` (53), `href` (42), `type` (36), `disabled` (21), `isDisabled` (11), `isLoading` (9), `rightIcon` (8), `ariaLabel` (3), `icon` (3), `isExternal` (1) | `width` (50), `height` (14), `w` (13), `mt` (11), `marginBottom` (6), `fontWeight` (4), `mb` (4), `alignSelf` (3), `mr` (3), `paddingX` (3), `bgColor` (2), `colorPalette` (2), `cursor` (2), `flexShrink` (2), `marginLeft` (2), `minWidth` (2), `whiteSpace` (2), `display` (1), `flex` (1), `gap` (1), `marginRight` (1), `marginTop` (1), `maxW` (1), `minW` (1), `ml` (1), `order` (1), `position` (1), `right` (1), `style` (1), `top` (1) | 0 | 4 |
| Card | 5 | — | `children` (5), `outline` (1), `outlineColor` (1) | 0 | 1 |
| CardBody | 2 | — | `children` (2), `margin` (1), `padding` (1) | 0 | 0 |
| CardFooter | 1 | — | `bgColor` (1), `children` (1), `display` (1), `flexDirection` (1), `justifyContent` (1) | 0 | 0 |
| CardHeader | 1 | — | `children` (1), `height` (1), `marginBottom` (1), `marginTop` (1), `paddingX` (1), `textAlign` (1) | 0 | 0 |
| Carousel | 4 | `children` (4), `onSlideSelect` (3), `startIndex` (3), `paginationType` (2), `fullScreen` (1) | `key` (1) | 0 | 0 |
| Center | 54 | — | `children` (54), `padding` (36) | 0 | 0 |
| Checkbox | 23 | `isChecked` (23), `label` (17), `name` (15), `onChange` (15), `variant` (5), `isDisabled` (3), `readOnly` (2), `indeterminate` (1), `paddingY` (1) | `children` (13), `key` (3) | 0 | 9 |
| CheckboxFilter | 5 | `items` (5), `language` (5), `onApply` (5) | `children` (3) | 0 | 0 |
| CheckboxGroup | 0 | — | — | 0 | 0 |
| Chip | 0 | — | — | 0 | 0 |
| Collapse | 2 | `in` (2), `animateOpacity` (1) | `children` (2) | 0 | 0 |
| ColorPicker | 1 | — | `border` (1), `borderColor` (1), `borderRadius` (1), `children` (1), `name` (1), `onChange` (1), `value` (1) | 0 | 0 |
| Count | 7 | `count` (7), `ariaLabel` (1), `variant` (1) | `children` (7) | 0 | 0 |
| DatePicker | 3 | `min` (3), `size` (2) | `children` (3) | 0 | 3 |
| DevOverlay | 1 | `activeTheme` (1), `displayTranslationKeys` (1), `hideDevOverlay` (1), `toggleTheme` (1), `toggleTranslation` (1), `variables` (1) | — | 0 | 0 |
| Dialog | 0 | — | — | 0 | 0 |
| DialogFilter | 0 | — | — | 0 | 0 |
| DiscreteSlider | 1 | `applyIndentation` (1), `marks` (1), `onValueChanged` (1), `value` (1) | `children` (1) | 0 | 0 |
| Divider | 56 | — | `children` (44), `orientation` (10), `my` (9), `marginBottom` (7), `mt` (5), `width` (4), `borderColor` (3), `marginX` (3), `mb` (3), `display` (2), `marginY` (2), `height` (1), `marginTop` (1), `pt` (1) | 0 | 0 |
| Drawer | 1 | `isOpen` (1), `onClose` (1) | `children` (1), `placement` (1), `size` (1) | 0 | 0 |
| DrawerBody | 1 | — | `children` (1), `maxWidth` (1), `paddingX` (1), `paddingY` (1) | 0 | 0 |
| DrawerContent | 1 | `borderRadius` (1), `p` (1), `withCloseButton` (1) | `children` (1), `data-testid` (1) | 0 | 0 |
| DrawerOverlay | 1 | — | `children` (1) | 0 | 0 |
| EnergyLabel | 0 | — | — | 0 | 0 |
| ErrorPage | 4 | `language` (4), `statusCode` (4) | `children` (3) | 0 | 0 |
| FilterHeading | 0 | — | — | 0 | 0 |
| Flex | 328 | — | `children` (327), `alignItems` (138), `justifyContent` (115), `direction` (50), `gridGap` (36), `flexDirection` (35), `mb` (32), `width` (29), `flexWrap` (19), `mt` (19), `w` (19), `height` (18), `h` (17), `padding` (16), `justify` (15), `key` (15), `borderRadius` (12), `wrap` (12), `align` (11), `color` (10), `marginBottom` (10), `mr` (9), `backgroundColor` (8), `borderColor` (8), `flexShrink` (7), `paddingX` (7), `paddingY` (7), `position` (7), `background` (6), `bg` (6), `cursor` (5), `flex` (5), `margin` (5), `maxWidth` (5), `_last` (4), `border` (4), `boxShadow` (4), `display` (4), `fontWeight` (4), `gridColumnGap` (4), `minH` (4), `ml` (4), `mx` (4), `px` (4), `py` (4), `borderBottom` (3), `marginTop` (3), `maxW` (3), `onClick` (3), `overflow` (3), `textAlign` (3), `borderBottomColor` (2), `borderWidth` (2), `fontSize` (2), `gridRowGap` (2), `left` (2), `marginRight` (2), `marginX` (2), `marginY` (2), `minHeight` (2), `minW` (2), `rounded` (2), `top` (2), `_groupHover` (1), `_hover` (1), `align-self` (1), `alignContent` (1), `aria-hidden` (1), `as` (1), `borderBottomWidth` (1), `borderStyle` (1), `borderTop` (1), `borderTopColor` (1), `borderTopWidth` (1), `css` (1), `data-testid` (1), `flexBasis` (1), `flexDir` (1), `gapX` (1), `gapY` (1), `gridArea` (1), `justifyItems` (1), `lineHeight` (1), `maxH` (1), `minWidth` (1), `my` (1), `overflowY` (1), `p` (1), `paddingBottom` (1), `paddingRight` (1), `paddingTop` (1), `pb` (1), `pt` (1), `textOverflow` (1) | 0 | 1 |
| FocusedHeader | 1 | `brand` (1) | — | 0 | 0 |
| Footer | 1 | `brand` (1), `environment` (1), `language` (1), `project` (1), `useAbsoluteUrls` (1) | — | 0 | 0 |
| FormControl | 133 | `id` (133), `label` (96), `errorMessage` (94), `size` (83), `isRequired` (60), `isDisabled` (11), `tooltip` (11), `hint` (1), `labelButtonOnClick` (1), `labelButtonText` (1) | `children` (133), `key` (2) | 0 | 0 |
| FormControlSection | 3 | `errorMessage` (3), `id` (3), `label` (3), `hint` (2), `tooltip` (2) | `children` (3) | 0 | 0 |
| FormLabel | 0 | — | — | 0 | 0 |
| FullHeight | 0 | — | — | 0 | 0 |
| GalleryHeader | 1 | `currentSlide` (1), `language` (1), `onClose` (1), `slidesCount` (1) | `children` (1) | 0 | 0 |
| Grid | 87 | — | `children` (87), `templateColumns` (74), `gap` (40), `templateAreas` (38), `columnGap` (21), `rowGap` (21), `alignItems` (9), `mb` (7), `templateRows` (6), `backgroundColor` (5), `maxW` (5), `gridTemplateRows` (4), `padding` (4), `w` (4), `borderBottom` (3), `borderColor` (3), `borderRadius` (3), `data-testid` (3), `gridTemplateColumns` (3), `justifyItems` (3), `mx` (3), `paddingY` (3), `bg` (2), `gridColumnGap` (2), `justifyContent` (2), `margin` (2), `marginBottom` (2), `marginTop` (2), `my` (2), `p` (2), `paddingLeft` (2), `paddingRight` (2), `ref` (2), `width` (2), `_hover` (1), `_last` (1), `alignContent` (1), `autoColumns` (1), `autoFlow` (1), `background` (1), `border` (1), `borderBottomColor` (1), `borderLeft` (1), `borderLeftColor` (1), `borderTop` (1), `borderTopLeftRadius` (1), `borderTopRightRadius` (1), `boxShadow` (1), `color` (1), `flex` (1), `fontWeight` (1), `gridAutoRows` (1), `gridTemplateAreas` (1), `key` (1), `marginX` (1), `maxWidth` (1), `minW` (1), `opacity` (1), `paddingInline` (1), `paddingX` (1), `px` (1), `py` (1) | 0 | 5 |
| GridItem | 232 | — | `children` (231), `area` (131), `gridArea` (60), `display` (36), `textAlign` (35), `alignSelf` (27), `justifyContent` (14), `as` (13), `alignItems` (10), `colSpan` (10), `fontSize` (6), `fontWeight` (5), `height` (5), `marginLeft` (5), `alignContent` (4), `color` (4), `gap` (4), `gridRow` (4), `mr` (4), `overflow` (4), `mt` (3), `pt` (3), `w` (3), `backgroundColor` (2), `flexDirection` (2), `gridColumn` (2), `h` (2), `justifySelf` (2), `key` (2), `marginBottom` (2), `marginRight` (2), `marginTop` (2), `maxW` (2), `minW` (2), `mx` (2), `aria-label` (1), `backgroundImage` (1), `backgroundPosition` (1), `backgroundSize` (1), `borderColor` (1), `borderTop` (1), `data-testid` (1), `gridAutoRows` (1), `margin` (1), `marginX` (1), `mb` (1), `minHeight` (1), `minWidth` (1), `onClick` (1), `opacity` (1), `order` (1), `padding` (1), `paddingLeft` (1), `paddingRight` (1), `pb` (1), `px` (1), `rounded` (1), `style` (1), `textStyle` (1) | 0 | 10 |
| H1 | 32 | — | `children` (32), `marginBottom` (8), `textStyle` (7), `mb` (5), `marginY` (2), `color` (1), `fontSize` (1), `mt` (1), `textAlign` (1) | 0 | 0 |
| H2 | 49 | — | `children` (49), `textAlign` (7), `marginBottom` (5), `mb` (5), `textStyle` (3), `fontSize` (2), `whiteSpace` (2), `color` (1), `display` (1), `fontWeight` (1), `ml` (1), `pb` (1), `w` (1) | 0 | 0 |
| H3 | 50 | — | `children` (50), `mb` (13), `color` (10), `marginBottom` (5), `mt` (4), `textAlign` (4), `fontWeight` (2), `textStyle` (2), `alignItems` (1), `display` (1), `fontSize` (1), `lineHeight` (1), `marginY` (1), `maxWidth` (1), `overflow` (1), `paddingY` (1), `textOverflow` (1), `w` (1), `whiteSpace` (1), `width` (1) | 0 | 1 |
| H4 | 79 | — | `children` (79), `mb` (13), `ml` (6), `fontWeight` (4), `textAlign` (4), `marginBottom` (3), `mt` (3), `pb` (2), `alignItems` (1), `borderBottomColor` (1), `borderBottomWidth` (1), `color` (1), `display` (1), `gap` (1), `justifyContent` (1), `margin` (1) | 0 | 0 |
| H5 | 8 | — | `children` (8), `fontWeight` (4), `textStyle` (2), `mb` (1), `overflow` (1), `textOverflow` (1), `whiteSpace` (1) | 0 | 0 |
| H6 | 0 | — | — | 0 | 0 |
| HeaderNavigation | 1 | `brand` (1), `entitlements` (1), `environment` (1), `hasNotification` (1), `language` (1), `onLogin` (1), `onLogout` (1), `project` (1), `selectTenant` (1), `trackEvent` (1), `useAbsoluteUrls` (1), `user` (1) | — | 0 | 0 |
| Hide | 26 | `above` (13), `below` (13) | `children` (26), `display` (1) | 0 | 0 |
| HighlightedText | 0 | — | — | 0 | 0 |
| HoverCard | 0 | — | — | 0 | 0 |
| Input | 48 | `type` (21), `size` (17), `placeholder` (14), `value` (10), `name` (9), `debounce` (7), `setInputValue` (7), `onChange` (6), `isDisabled` (4), `icon` (3), `isClearable` (3), `onBlur` (3), `leftAddonElement` (1), `onFocus` (1) | `children` (47), `data-testid` (2), `ref` (1) | 0 | 39 |
| InsertionLayout | 30 | `maxContentWidth` (17) | `children` (30) | 0 | 0 |
| LayoutWithVehicleReference | 2 | `leftColumnSize` (2), `maxContentWidth` (2), `rightColumnSize` (2), `title` (2), `vehicle` (2) | `children` (2) | 0 | 0 |
| Link | 207 | `href` (136), `onClick` (90), `disabled` (42), `leftIcon` (22), `isExternal` (16), `rightIcon` (10), `prefetch` (7), `isTruncated` (1), `noOfLines` (1), `replace` (1) | `children` (207), `as` (124), `color` (37), `target` (31), `type` (15), `padding` (13), `width` (10), `mt` (9), `display` (7), `flexShrink` (7), `rel` (7), `ml` (5), `textDecoration` (5), `textStyle` (4), `alignItems` (3), `fontWeight` (3), `textAlign` (3), `w` (3), `_hover` (2), `aria-expanded` (2), `aria-label` (2), `data-testid` (2), `justifySelf` (2), `maxW` (2), `overflow` (2), `whiteSpace` (2), `aria-controls` (1), `bg` (1), `border` (1), `borderColor` (1), `borderRadius` (1), `cursor` (1), `gridArea` (1), `key` (1), `left` (1), `marginTop` (1), `paddingX` (1), `paddingY` (1), `pointerEvents` (1), `position` (1), `role` (1), `style` (1), `textTransform` (1), `top` (1), `wordBreak` (1) | 0 | 0 |
| LinkBox | 0 | — | — | 0 | 0 |
| LinkOverlay | 0 | — | — | 0 | 0 |
| List | 12 | `size` (2), `spacing` (1) | `children` (12), `display` (4), `flexDirection` (4), `justifyContent` (3), `alignItems` (2), `aria-label` (2), `gap` (2), `textAlign` (2), `onClick` (1), `paddingLeft` (1), `pt` (1), `width` (1) | 0 | 0 |
| ListItem | 54 | — | `children` (54), `key` (12), `display` (2), `alignItems` (1), `mb` (1), `my` (1), `paddingLeft` (1), `textStyle` (1) | 0 | 0 |
| MarkedText | 28 | `variant` (26), `highlightColor` (6) | `children` (28), `as` (2), `fontSize` (2), `mb` (2), `mr` (2), `textStyle` (2), `fontWeight` (1) | 0 | 0 |
| Menu | 3 | `items` (3), `menuColor` (3), `title` (3), `fontWeightTitle` (2), `showChevron` (1), `value` (1) | `children` (2) | 0 | 0 |
| MissingImage | 10 | — | `children` (2) | 0 | 0 |
| MobileOnlyAccordion | 0 | — | — | 0 | 0 |
| Modal | 29 | `isOpen` (29), `onClose` (29), `size` (28), `title` (22), `disableBodyPadding` (9), `motionPreset` (5), `primaryActionButton` (5), `secondaryActionButton` (5), `variant` (3) | `children` (29) | 0 | 1 |
| ModalCloseButton | 3 | — | `children` (3), `color` (3), `fontSize` (3), `height` (2), `width` (2), `_hover` (1), `alignSelf` (1), `bg` (1), `ml` (1), `mr` (1), `onMouseEnter` (1), `padding` (1), `rounded` (1), `transform` (1), `transitionDelay` (1), `transitionDuration` (1), `transitionProperty` (1), `transitionTimingFunction` (1), `visibility` (1) | 0 | 0 |
| MotoScout24AppLogo | 0 | — | — | 0 | 0 |
| OpenFilterButton | 2 | `displayValue` (2), `isApplied` (2), `label` (2), `onClick` (2), `onResetFilter` (2), `resetButtonAriaLabel` (2), `variant` (2), `isDisabled` (1), `showResetButton` (1) | `children` (2) | 0 | 0 |
| OrderedList | 1 | — | `children` (1) | 0 | 0 |
| PageLayout | 3 | `header` (3), `maxContentWidth` (3) | `children` (3) | 0 | 0 |
| Pagination | 8 | `currentPage` (8), `onChange` (8), `totalPages` (8) | `marginTop` (7), `marginBottom` (4), `children` (2) | 0 | 0 |
| Popover | 20 | `content` (20), `placement` (18), `trigger` (16), `contentPosition` (10), `onClose` (6), `onOpen` (6), `closeOnInteractOutside` (3), `contentPadding` (3), `gutter` (3), `isOpen` (3), `showArrow` (3), `closeOnBlur` (2), `maxWidth` (2), `size` (2), `autoFocus` (1), `open` (1) | `children` (20) | 0 | 0 |
| PopoverFilter | 6 | `actionButton` (6), `children` (6), `displayValue` (6), `isApplied` (6), `label` (6), `language` (6), `numberOfAppliedFilters` (6), `onPopoverClose` (6), `onPopoverOpen` (6), `onResetFilter` (6), `hasFlip` (1), `header` (1), `initialPopoverState` (1), `isDisabled` (1), `showCallToActionButton` (1), `triggerHeight` (1), `zIndex` (1) | — | 0 | 0 |
| Progress | 1 | `current` (1), `label` (1), `max` (1) | `children` (1) | 0 | 0 |
| Radio | 9 | `items` (9), `onChange` (9), `value` (9), `children` (6), `name` (6), `orientation` (5), `size` (1), `variant` (1) | `ref` (3) | 0 | 0 |
| RadioList | 2 | `name` (2), `onChange` (2), `options` (2), `defaultValue` (1) | — | 0 | 0 |
| RangeFilterInput | 0 | — | — | 0 | 0 |
| RangeFilterInputWithSlider | 1 | `from` (1), `onBlur` (1), `onChange` (1), `rangeSliderScale` (1), `to` (1), `unit` (1) | `children` (1), `max` (1), `min` (1) | 0 | 0 |
| RangeSlider | 0 | — | — | 0 | 0 |
| Rating | 1 | `rating` (1), `size` (1) | `children` (1) | 0 | 0 |
| SearchableList | 5 | `listItems` (5), `searchFieldOptions` (5), `listOptions` (3), `NoResults` (1) | `children` (4) | 0 | 0 |
| SearchField | 0 | — | — | 0 | 0 |
| Section | 0 | — | — | 0 | 0 |
| Select | 55 | `options` (55), `size` (55), `name` (17), `value` (15), `isDisabled` (12), `isInvalid` (6) | `children` (53), `placeholder` (40), `onChange` (16), `borderRightRadius` (1) | 0 | 38 |
| SelectMenu | 1 | `leftIcon` (1), `options` (1), `showChevron` (1), `title` (1), `value` (1), `withIndicator` (1) | — | 0 | 0 |
| Show | 20 | `above` (13), `below` (7) | `children` (20), `background` (1), `borderRadius` (1), `data-testid` (1), `fontSize` (1), `marginTop` (1), `mt` (1), `padding` (1) | 0 | 0 |
| SimpleGrid | 57 | `spacing` (43), `columns` (30), `spacingX` (8), `spacingY` (7) | `children` (57), `alignItems` (2), `key` (1), `rowGap` (1), `width` (1) | 0 | 0 |
| SingleColumnCenteredLayout | 0 | — | — | 0 | 0 |
| Skeleton | 13 | `isLoaded` (11) | `children` (13), `height` (3), `width` (3) | 0 | 0 |
| Spinner | 37 | `size` (18) | `children` (26) | 0 | 0 |
| Stack | 377 | `spacing` (297) | `children` (376), `direction` (135), `width` (61), `alignItems` (39), `justify` (30), `marginTop` (16), `key` (14), `align` (13), `marginBottom` (10), `paddingX` (9), `wrap` (6), `borderRadius` (5), `paddingY` (5), `marginX` (3), `bg` (2), `borderColor` (2), `borderWidth` (2), `boxShadow` (2), `maxWidth` (2), `w` (2), `background` (1), `backgroundColor` (1), `flexWrap` (1), `justifyContent` (1), `minH` (1), `padding` (1), `paddingBottom` (1), `paddingTop` (1), `separator` (1) | 0 | 1 |
| Switch | 7 | `id` (7), `isChecked` (7), `onChange` (7), `label` (6), `isDisabled` (2) | `children` (5) | 0 | 0 |
| Tab | 17 | `value` (17), `isDisabled` (2), `marginX` (2) | `children` (17), `key` (3) | 0 | 1 |
| Table | 4 | — | `children` (4), `css` (2), `h` (2), `mb` (2), `w` (2), `colorScheme` (1), `size` (1), `striped` (1) | 0 | 0 |
| TabList | 11 | — | `children` (11), `borderBottom` (1), `px` (1) | 0 | 0 |
| TabPanel | 17 | — | `children` (17), `value` (17), `key` (3) | 0 | 0 |
| TabPanels | 10 | — | `children` (10) | 0 | 0 |
| Tabs | 11 | `onChange` (7), `variant` (4) | `children` (11), `defaultValue` (11), `lazyMount` (4), `value` (1) | 0 | 0 |
| Tbody | 4 | — | `children` (4) | 0 | 0 |
| Td | 19 | `isNumeric` (6) | `children` (19), `colSpan` (10), `borderBottom` (8), `py` (6), `data-label` (5), `fontWeight` (2), `pb` (2), `pt` (2), `w` (1), `width` (1) | 0 | 0 |
| TenantSelection | 1 | `isLoading` (1), `language` (1), `selectTenant` (1), `user` (1) | — | 0 | 0 |
| Text | 666 | `isTruncated` (8), `textColor` (3), `noOfLines` (1) | `children` (665), `textStyle` (157), `fontWeight` (114), `color` (113), `mb` (59), `as` (53), `textAlign` (49), `fontSize` (28), `mr` (24), `mt` (22), `marginBottom` (20), `ml` (18), `display` (17), `lineHeight` (15), `textTransform` (9), `paddingY` (6), `whiteSpace` (6), `minHeight` (5), `wordBreak` (5), `alignItems` (4), `backgroundColor` (4), `cursor` (4), `paddingX` (4), `pr` (4), `borderColor` (3), `borderRight` (3), `flexGrow` (3), `key` (3), `marginLeft` (3), `paddingRight` (3), `py` (3), `alignSelf` (2), `flexShrink` (2), `id` (2), `marginRight` (2), `marginTop` (2), `maxW` (2), `onClick` (2), `position` (2), `pt` (2), `textDecoration` (2), `textDecorationColor` (2), `textDecorationThickness` (2), `width` (2), `zIndex` (2), `aria-label` (1), `bottom` (1), `height` (1), `left` (1), `margin` (1), `maxWidth` (1), `my` (1), `overflow` (1), `overflowWrap` (1), `padding` (1), `pos` (1), `right` (1), `textOverflow` (1), `title` (1), `top` (1), `transform` (1), `visibility` (1), `w` (1) | 0 | 0 |
| Textarea | 13 | `rows` (11), `placeholder` (6), `textStyle` (3) | `children` (13) | 0 | 13 |
| Tfoot | 1 | — | `children` (1) | 0 | 0 |
| Th | 11 | `isNumeric` (1) | `children` (9) | 0 | 0 |
| Thead | 4 | — | `children` (4) | 0 | 0 |
| ThemeProvider | 6 | `theme` (6) | `children` (6) | 0 | 0 |
| TimePicker | 1 | — | `children` (1) | 0 | 1 |
| Tooltip | 26 | `label` (26), `placement` (18) | `children` (26) | 0 | 0 |
| TopListingBadge | 5 | `aspectRatio` (1) | `children` (5) | 0 | 0 |
| TopRightToast | 2 | — | — | 0 | 0 |
| TopToast | 2 | — | — | 0 | 0 |
| TopVehicleSharedBadge | 1 | `aspectRatio` (1), `brand` (1) | `children` (1) | 0 | 0 |
| Tr | 13 | — | `children` (13), `key` (1) | 0 | 0 |
| TwoColumnsLayout | 0 | — | — | 0 | 0 |
| UnorderedList | 13 | `variant` (6) | `children` (13), `listStylePosition` (3), `paddingLeft` (3) | 0 | 0 |
| VehicleReference | 0 | — | — | 0 | 0 |

Regenerate with `npm run components:inventory`.
