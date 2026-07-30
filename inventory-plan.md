## Plan: components-pkg usage inventory

1. Establish the canonical component list from the public API of `@smg-automotive/components`.

   - Use the installed package entry point for version `25.30.0-chakra-v3.1`.
   - Keep only exports that are JSX-renderable components.
   - Exclude icons, hooks, types, constants, functions, themes, and other non-component exports.
   - Resolve re-exports and adapters so a public component is represented exactly once—no duplicate rows caused by internal implementation paths or export chains.

2. Build an AST-based usage extractor for seller-web.

   - Scan TypeScript/TSX source files.
   - Resolve imports from `@smg-automotive/components`, including aliases.
   - Count only JSX-rendered usage such as `<Button />`; imports alone do not count.
   - Treat public export name as the canonical component identity, even when locally aliased.

3. Collect prop usage per component.

   - Create one unique row per public component, including components with zero seller-web usages.
   - Count each explicitly written JSX prop occurrence.
   - Sort each component’s props by descending count.
   - Record JSX spread attributes separately as `spreadProps` counts; their contents cannot be reliably attributed without a second, deeper analysis.

4. Generate the lookup artifacts.

   - Machine-readable source data, including package version, component count, component usage count, prop counts, and source locations.
   - A readable Markdown/HTML lookup table for daily use:
     `Component | JSX uses | Props by usage count | Spread-prop uses`
   - One row per public component, with no duplicated adapter/re-export rows.

5. Validate and document.

   - Assert every canonical public JSX component has exactly one row.
   - Spot-check aliases, adapters, nested/public composite components, zero-use components, and spread props.
   - Add a documented command to regenerate the inventory when `components-pkg` changes.

Explicitly out of scope: thresholds, refactoring recommendations, consistency analysis, and findings. Those can consume this inventory later without changing its counting rules.