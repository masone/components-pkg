# Component library future

This RFC makes a few foundational statements that will help us improving the DX of our component library. These are necessary to understand and agree upon before we plan further steps. No technical solutions at this point, just alignment.

## Problem statement

> Today, the component library has no predictable public contract, blending custom and Chakra props in unpredictable ways, which makes Chakra-based components difficult to use consistently.

components-pkg was intended to be an abstraction over Chakra, providing the product-facing component interface. Over time, that boundary gradually blurred and we strayed from the initial intent of providing a deliberate abstraction. Especially with the Chakra migration, there was a pull towards the exposed components becoming more Chakra-centric. 

Embracing the way Chakra works, our interfaces expose Chakra-internal props. The problem is that this happens without deliberate concern. Some components pick, some omit, some pass through. There is no clear pattern that can be applied. The result is an inconsistent interface: neither pure Chakra, nor purely custom, but an implicit blend of both.

What makes this worse, is that the available set of props is not visible in Storybook. We intentionally filter out all Chakra props. A consumer can't look at Storybook and understand what the available interface looks like. They have to fill the gap by infering the full picture from Chakra docs and types. 

## Options & Analysis

If we want to harmonize the confusing hybrid interfaces, we have two options:

1. We can inch closer to raw Chakra, exposing the full set of props directly and reducing custom props. This means moving away from the component library being an abstraction, towards it being a Chakra configuration layer. The most extreme version of this is to import components directly from Chakra, not through the components-pkg.
2. Or we can take control of the interfaces, deliberately selecting necessary Chakra props case by case and consider them as part of our custom interface design.

The latter is in line with our original intent and with the state of our codebase too. The interface of components-pkg

### Analysis

Only a few primitives are practically a Chakra re-export (where we may or may not `Omit` or `Pick` a subset). The bulk of our components have custom props:

- Yes, some are practically just aliases (passing a `text` prop instead of passing `children`), but that is a fair design decision.
- In contrast, some of the custom props carry significant weight. Our `Button` is arguably complex, but can take an `iconLeft` or even have a `href`. Some of these capabilities would be solved differently in the corresponding Chakra component and some are simply non-existent.
- There's a good share of components that have only custom props because they're composite components that don't exist in Chakra. These are all fair abstractions.
- And then there is a large chunk of components that have a mix of custom props and their underlying Chakra props exposed too. We sometimes `Omit` or `Pick`, but often expose the full set of Chakra props - even when we often don't need it.

We currently heavily lean into building abstractions on top of Chakra components (much closer to Option 2 above). 

- We are not exposing all components Chakra has to offer.
- The majority of the components have bespoke interfaces and are an opinionated layer on top of Chakra.
- When we expose underlying Chakra props, it often does not look like an intentional choice, but a potentially non-warranted default.
- Components-pkg is clearly closer to an opinionated abstraction, than to a raw passthrough of selected Chakra components

## Proposal 

Proposed components library philosophy in a nutshell:

- Components-pkg is an opinionated set of components with bespoke props interfaces, built with Chakra
- The primary purpose of this abstraction layer is to enforce the design system. eg. not every Chakra component is available, buttons should not accept arbitrary text colors, etc.
- The primary purpose of the abstraction is not to hide Chakra from consumers entirely. We have to bea realistic that Chakra is an implementation detail we can't fully abstract away. Our components libary is heavily depending on Chakra and its concepts inadvertedly leak to the consumer. You don't get around understanding Chakra concepts, for example you have to know what tokens are, you have to know how Chakra handles responsiveness, etc.
- Chakra supplies raw components, accessible behaviour, composition mechanisms, recipes, style props, tokens, and implementation patterns. We do not intend to mirror Chakra. The component library decides which of those capabilities it promises to application developers and in which way.
- We decide what the best surface is component by component. We don't have to mirror the underlying Chakra component behavior and props exactly. For example, just because Chakra offers great composability on a component, doesn't mean we can't expose the composability as a closed component. 
- We choose to expose Chakra props deliberately when they are warranted (pick, not omit). A prop that is forwarded from Chakra is considered as part of the public stable interface, just like custom props are. We commit to treating all props as an equal abstraction. Just because we leverage a Chakra prop, does not mean we're leaking Chakra internals.
- Interfaces are consistent. You roughly know what to expect for each component type. Props are strongly typed and well documented in Storybook.
- Consumers of components-pkg use the Chakra docs only to understand the concepts. They should not be using the Chakra docs for deriving capabilities. Only the storybook, docs and interfaces of the library itself is authoritative.
- Chakra documentation is primarily implementation material for component-library maintainers. This remains true when a library component deliberately uses Chakra naming or patterns.

### Framework choice

We continue committing to Chakra-ui

The position of this RFC is that problem is not with Chakra, but in inconsistency around components-pkg interfaces. While the idea of replacing Chakra pops up here and there, we never invested the time demonstrating an alternative path with tradeoffs, consequences and a migration plan clearly laid out - and that is telling in itself. The default position is that the existing framework remains the right choice. The burden of proof is on proposals that seek to replace it, not on repeatedly re-justifying the original decision. 

### Props vocabulary

The exposed interfaces should have consistent naming. We want to prevent some components exposing `disabled` while others expose `isDisabled` or even `bg` versus `background`. This is part of building a predictable interface.

By exposing Chakra props on our interfaces, we inherently adopted a v2-inspired starting point. Since the components-pkg is an abstraction by definition, there is no need to adopt v3 naming conventions. The adapters give us exactly the type of abstraction the component-pkg is set up to provide. Since we never considered the passed-through Chakra props as part of _our API_, changing Chakra internals leak to the consumer and create inconsistencies.

Proposal: We make a decision for adopting Chakra v3 style props (and removing the adapters) vs. keeping v2 abstractions. We decide based on actual usage what the path of least resistance is. 


### Expose all available props in Storybook

In Storybook, we currently [intentionally filter out all Chakra props](.storybook/main.ts) and `as`, `asChild` and `recipe` are currently hidden globally. Consumers of components-pkg must be able to fully rely on Storybook as the authoritative source of truth.

Proposed change: Expose all available props in Storybook to offer a complete documentation. Noise will be reduced by deliberately exposing only the necessary props, see below.

### Types of components & their interfaces

We should acknowledge that we have components of varying types. They each have different needs for the props they expose. Chakra is exporting layout primitives and ui components on a flat export and we're putting our larger composite components on the same level. This is making it hard to tell them apart. 

At least internally, we should organize components by type in separate folders. By categorizing components, we can establish and enforce more consistent interfaces.

#### Layout primitives

Examples: `Box`, `Stack`, `Grid`, `Flex`.

These are the fundamental, lowest level of building blocks we have. This is how we apply low-level styling to low-level html elements. Their role is composition and layout, so they may expose a broad Chakra-like layout and styling surface. Projects should be able to pretty much achieve any customization needed with these. 

These will likely be implemented as transparent Chakra exports. It's still important that we commit to owning the exposed interface for these. Just because we heavily lean into Chakra, doesn't mean we are not responsible for deliberately designing the interface. A close mapping to Chakra is an intentional choice, not an obligation. 

Proposed change: People currently often find the set of props we pick on these restricting. We should decide to widen it.

#### Design system primitives

Examples: `Button`, `Input`, `Accordion`, `Dialog`, `Tabs`.

One level higher than layout primitives, these are actual visible components we use regularly to compose UIs. They're strongly tied to the design system and match what the designers are building with too. 

These expose a bespoke API. We can use Chakra components under the hood, or we can implement our own. They're small and flexible. Likely implemented with Chakra recipes, exposing variants for styling and custom behavioral props. We design the interface in a way the design system can't be violated. These components are often used directly in layouts and might expose a defined set of style props. If we're exposing Chakra props, it doesn't mean we are not responsible for deliberately designing the interface.

Proposed change: Decide if these components should allow to control their outer layout. Define a set of Chakra styling props that should be exposed for all of these to increase consistency.

#### Shared product components

Examples: `ArticleTeaser`, `VehicleCard`, `SearchResultItem`, `PriceSummary`.

These represent reusable product patterns. They are product-specific shared components. They're farthest away from being primitives. While they're reusable shared components, they're used selectively in specific contexts and are not foundational building blocks. They're not exposing Chakra styling props and all interfaces are bespoke. The sole reason they made it to the component library is probably to make them shareable, not because they're considered part of the core design system. 

Proposed change: Now that we're in the monorepo, we can consider splitting these out of the component library and just put them in a "shared components" folder.

### Ownership

Enablement team volounteered to driving this RFC, owning the defintions and putting enforcements and guardrails for human and agent in place.

The components-pkg continues to be owned by the whole Frontend team. It falls on us collectively to keep this library sane. This includes designing good interfaces, writing healthy components and maintaining them, navigating Chakra updates and managing breaking changes. Please make sure to invest in good interface design for new components and keep boy/girl-scouting as part of your regular product work. Larger refactors like the one we're up against naturally fall into Tech20 time bucket.


## Summary

We set out for components-pkg to be an abstraction layer, but over time we exposed more and more Chakra internals on the public interface without careful deliberation. The majority of our library is exposing custom bespoke interfaces. The components are adding valuable abstractions of top of raw Chakra components. The tension exists only because we treat any Chakra props we expose as passthroughs.

The library can deliberately borrow Chakra vocabulary, types, behaviours, recipes, and low-level building blocks. It can also intentionally provide transparent, Chakra-shaped primitives where that is useful. That doesn't mean we're in a conflict with mirroring Chakra 1-1 to the consumer. Every exported component remains a components-pkg contract: its supported props, composition, and documentation are defined by the library, not inferred from Chakra.

### Path forward

The primary goal is for us to agree on the direction this RFC sets. Only once we all understand the state of our current and philosophy of our future components library, we can move forward. 

I suggest to keep breaking changes and refactorings low and go the path of least resistance. Once our interfaces are transparent and consistent, it will be much easier to make deliberate breaking changes. 

- we freeze the interfaces for each components to exactly the set of props that are currently used (Pick). this gives us a limited stable set we can optimize down the line
- we decide what style we want for shorthand / longhand / object aliases and deduplicate accordingly
- we reflect all available props for each component in Storybook
- we classify components into
  - layout primitives (broad Chakra props exposed)
  - design system primitives (bespoke interfaces, selective Chakra props)
  - shared product components (bespoke interfaces, no Chakra props)
- we define what Chakra props every layout primitive offers consistently (eg. all)
- we define a set of Chakra props every design system primitive offers consistently (if any by default)
- we define a vocabulary for naming props (v2 vs v3 syntax), which also defines what to do with the adapters: keep or remove
- these practices get documented for agents and humans alike
- only later, we decide on `as` usage, restricting custom tokens, etc.

### Decisions

- components-pkg is an abstraction layer
- components are implemented With Chakra v3
- nobody should have to look at Chakra docs when using components-pkg
- every Chakra prop we expose is considered part of the abstraction, even when they're a wrapper/passthrough. the interfaces are designed deliberately, Chakra props are exposed selectively and with purpose, not by default (this is the important midset shift)
- we optimize for consistency over perfection for the short term. we build on our current state, go the path of least resistance and prevent unnecessary breaking changes

## Please debate this proposal

Feel free to reach out to me for a synchronous discussion. 