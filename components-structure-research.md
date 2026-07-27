# Components structure — supporting research

> Working notes behind the structure RFC. Detailed evidence and one worked
> exemplar (Button) live here so the RFC itself can stay high-level (structure +
> categorization). Nothing here is a committed API change — it's the material we
> reasoned from.

---

## 1. Current-state findings (evidence)

### 1.1 The same primitive exists in three layers

For `Box`:

| Layer | File | Behavior |
|---|---|---|
| Real Chakra | `@chakra-ui/react` | Chakra v3 `Box` |
| Transparent re-export | `src/components/box/index.tsx` | `export { Box } from '@chakra-ui/react'` |
| **Compatibility adapter** | `src/components/adapter/Box/index.tsx` | Renames `color`→`textColor`, `gap`→`spacing`, reimplements `as`/`asChild`, drops `sx` |

The **adapter is the public export**: `src/components/index.ts` has **28**
`export * from './adapter/*'` lines. So `import { Box } from '@smg-automotive/components'`
returns the v2-flavored shim, not Chakra. Adapters translate the old Chakra v2 API
(`isDisabled`, `isLoading`, `isTruncated`, `noOfLines`, `spacing`, `textColor`,
`align`, `sx`) into v3 (`disabled`, `loading`, `truncate`, `lineClamp`, `gap`,
`color`, `textAlign`, `css`). They are leftovers from the v2→v3 migration.

### 1.2 Primitives follow three contradictory philosophies

| Primitive | File | Contract today |
|---|---|---|
| `Box` | `src/components/box/index.tsx` | Transparent — full Chakra API |
| `Flex` | `src/components/flex/index.tsx` | `Omit<FlexProps, 'gap' \| 'columnGap' \| 'rowGap'>` |
| `Stack` | `src/components/stack/index.tsx` | `Pick<…>` of ~20 hand-listed props |

Plus `adapter/Stack` adds `spacing` on top of `Stack`. No inferable rule.

### 1.3 Whitelisting-for-no-value

`src/components/textarea/index.tsx` re-declares a hand-picked prop interface and
forwards straight to `ChakraTextarea`. Blocks nothing meaningful; guarantees the day
someone needs `maxLength`/`aria-*` they edit the package or reach for `as`.

### 1.4 `as` as a workaround

- **64** `as=` occurrences in `src/components`.
- **9** are `as={SomeComponent}` (e.g. `<Box as={Stack} spacing="md">`, `<Link as={Button}>`).
- Also **9** ad-hoc CSS-variable declarations in component TSX.

### 1.5 What is already correct (templates to copy)

- `src/components/carousel/index.tsx` — Category-3 done right: semantic props, Chakra
  internal, nothing leaked.
- `src/components/breadcrumbs/index.tsx` — Category-2 extension via `Breadcrumb.Root` +
  slot recipe.
- `src/themes/shared/recipes/button.ts` — recipe-owned visual identity, typed variants.
- Recipe coverage is broad already: **14 recipes + ~20 slot recipes**.

### 1.6 Enforcement gap

`eslint.config.mjs` has **no** `no-restricted-imports` for Chakra and no per-category
rules. Nothing prevents direct Chakra imports of branded components or new adapters.

### 1.7 Dependency shape (decides the primitive strategy)

`@chakra-ui/react` + Emotion are **bundled `dependencies`** (pinned `3.36.0`), not
peers. → transparent re-export keeps a single Chakra instance controlled by the
package; direct app imports would risk dual-instance / broken-context theming unless
Chakra is flipped to a peer dep.

---

## 2. Worked exemplar — Button (Category 2)

> Captured to pressure-test the design-system contract. **Not** a committed change;
> the RFC references this as an illustration only.

### 2.1 Current API vs contract

Current `UnifiedButtonProps` (`src/components/button/index.tsx`):

- ✅ Recipe-backed, typed `variant` (`primary`/`secondary`/`success`/`transparent`/`scouty`) + `size` (`md`/`lg`); `splitVariantProps` used.
- ❌ **Leaks raw style props** — API is a *blocklist* (`Omit<ChakraButtonProps, 'background'|'color'|'border'|'textStyle'|…>`), so `padding`/`margin`/`width`/`display`/`boxShadow` all still land.
- ❌ **v2 icon dialect** — `leftIcon`/`rightIcon` (v3 uses children composition).
- ❌ **IconButton merged in** via `icon`+`ariaLabel` union → most of the TS gymnastics.
- ⚠️ **`loading` untyped** — only sneaks through `...rest`.
- ⚠️ `colorPalette` unused — variants hardcode brand colors.

### 2.2 Evidence: which style props are actually passed to `<Button>`

19 internal usage sites. Style props seen:

| Prop | Where | Verdict |
|---|---|---|
| `variant`, `size` | everywhere | ✅ approved |
| `width={{ base:'full', sm:'fit' }}` | dialog (×2) | **Real recurring need** (responsive full-width) |
| `marginTop`, `marginX` | DrawerLoginToggle | ❌ leak → parent layout owns spacing |
| `paddingY="sm"` | SearchableListItem | ❌ leak → belongs to a `size` variant |
| `display="inline-flex"` | navigationTenantMenu | ❌ redundant → recipe base already sets it |

### 2.3 Decisions taken for the exemplar

| Question | Decision |
|---|---|
| IconButton | **Keep unified** (text + icon-only in one component) |
| Icon API | **Keep `leftIcon`/`rightIcon`** as design-system sugar |
| `colorPalette` | **Do not introduce** — keep fixed variants |
| Style props | **Strip all raw style props**; expose one semantic `fullWidth?: ResponsiveValue<boolean>` for the only real recurring need |
| `loading` | **Make first-class** (`loading`, `loadingText`) |
| Recipe location | **Keep in `src/themes/shared/recipes/`** (no co-location churn) |

### 2.4 Target sketch (illustrative)

```tsx
type ButtonVariants = RecipeVariantProps<typeof buttonRecipe>;   // variant | size

type ButtonBase = ButtonVariants & {
  children: ReactNode;
  disabled?: boolean;
  loading?: boolean;
  loadingText?: string;
  fullWidth?: ResponsiveValue<boolean>;   // only semantic layout prop
  leftIcon?: ReactElement;
  rightIcon?: ReactElement;
  onClick?: React.MouseEventHandler<HTMLButtonElement>;
};
// + existing button | submit | link(as='a') | icon-only(ariaLabel+icon) unions
// NO bg / color / p / m / w / display / boxShadow.
```

Internal migration cost if this shipped: ~5 sites (one is a straight deletion). External
consumers passing style props would break — acceptable only under a coordinated major;
`fullWidth` absorbs the main legitimate case.

---

## 3. Decisions log (scope, agreed)

| # | Decision |
|---|---|
| D1 | Three categories: **primitives / design-system / features** |
| D2 | Category names on disk: `src/primitives/`, `src/components/` (design-system), `src/features/` |
| D3 | **Internal-only reorg** — keep exporting everything from the **root**; no `/primitives` or `/features` subpaths (avoids breaking consumers) |
| D4 | Consumers are guided by the **TypeScript API shape**, not the import path; maintainers by the folder + per-folder lint |
| D5 | Primitives = **transparent re-export**, lint-guarded (barrel may only re-export from Chakra) |
| D6 | Design-system = curated API, look owned by a recipe, **no raw style props** |
| D7 | Features = **domain props only**, Chakra internal |
| D8 | **Freeze adapters** now (no new ones); remove later via codemod + major |
| D9 | **Strict tokens-only → backlog** |
| D10 | Coordinated major is acceptable *eventually*, but this RFC targets **minimal breaking changes** |
| D11 | RFC objective = **align on structure + categorize upcoming work**, not per-component API redesign |

---

## 4. Open questions (deferred, tracked)

- `IconButton` vs `Button` (currently: keep unified).
- react-hook-form + Chakra synthetic events (v3 moves away; RHF still expects them).
- Recipe co-location vs centralized (currently: centralized).
- Monorepo split timing — the eventual `primitives`/`features` package split *will* be
  a breaking path change; this RFC only sequences toward it.
