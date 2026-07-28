# RFC components-pkg design decisions

## Freeze interfaces

https://github.com/masone/components-pkg/blob/agent/extract-recent-exploration-results/seller-web-prop-contract-usage.md

https://github.com/masone/components-pkg/blob/agent/extract-recent-exploration-results/listings-web-prop-contract-usage.md



- we freeze the interfaces for each components to exactly the set of props that are currently used (Pick). this gives us a limited stable set we can optimize down the lineExpand commentComment on line R128Resolved
- we decide what style we want for shorthand / longhand / object aliases and deduplicate accordingly
- we reflect all available props for each component in StorybookExpand commentComment on line R130Resolved
- we classify components into
  - layout primitives (broad Chakra props exposed)
  - design system primitives (bespoke interfaces, selective Chakra props)
  - shared product components (bespoke interfaces, no Chakra props)
- we define what Chakra props every layout primitive offers consistently (eg. all)
- we define a set of Chakra props every design system primitive offers consistently (if any by default)Expand commentComment on line R136Resolved
- we define a vocabulary for naming props (v2 vs v3 syntax), which also defines what to do with the adapters: keep or remove
- these practices get documented for agents and humans alike
- only later, we decide on `as` usage, restricting custom tokens, etc.