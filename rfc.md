# Component library future

This RFC makes a few foundational statements that will help us improving the DX of our component library. These are necessary to understand and agree upon before we plan further steps. No technical solutions at this point, just alignment.

## Problem statement

> Today, the component library has no predictable public contract, blending custom and Chakra props in unpredictable ways, which makes Chakra-based components difficult to use consistently.

components-pkg was intended to be an abstraction over Chakra, providing the product-facing component interface. Over time, that boundary gradually blurred and we strayed from the initial intent of providing an abstraction. 

Embracing the way Chakra works, our interfaces expose Chakra-internal props. The problem is that this happens without deliberate concern. Some components pick, some omit, some pass through. There is no clear pattern that can be applied. The result is an inconsistent interface. They're neither pure Chakra, nor purely custom, they're all implicit hybrids.

## Options & Analysis

If we want to harmonize the mixed interfaces that are confusing, we have two options:

1. we can inch closer to raw Chakra, exposing the full set of props directly and reducing custom props. This means moving away from the component library being an abstraction, towards it being a Chakra configuration layer
2. or we can take control of the interfaces, deliberately selecting necessary Chakra props case by case and consider them as part of our custom interface design

### Analysis

Only a few primitives are practically a Chakra re-export (where we may or may not `Omit` or `Pick` a subset). The bulk of our components have custom props:

- Yes, some are practically just aliases (passing a `text` prop instead of passing `children`), but that is a fair design decision.
- In contrast, some of the custom props carry significant weight. Our `Button` is arguably complex, but can take an `iconLeft` or even have a `href`. Some of these capabilities would be solved differently in the corresponding Chakra component and some are simply non-existent.
- There's a good share of components that have only custom props because they're composite components that don't exist in Chakra. These are all fair abstractions.
- And then there is a large chunk of components that have a mix of custom props and their underlying Chakra props exposed too. We sometimes `Omit` or `Pick`, but often expose the full set of Chakra props - even when we often don't need it.

This shows that we're much closer to Option 2 (see above):

- We currently heavily lean into building abstractions on top of Chakra components. 
- We are not exposing all components Chakra has to offer.
- The majority of the components have bespoke interfaces and are an opinionated layer on top of Chakra.
- When we expose underlying Chakra props, it often does not look like an intentional choice, but a side effect.
- Components-pkg is clearly closer to an opinionated abstraction, than to a raw passthrough of selected Chakra components

## Components library Philosophy

- components-pkg is an opinionated set of components with bespoke props interfaces, built with Chakra
- Chakra supplies raw components, accessible behaviour, composition mechanisms, recipes, style props, tokens, and implementation patterns. we do not intend to mirror Chakra. The component library decides which of those capabilities it promises to application developers and in which way.
- the primary purpose of this layer is to enforce the design system. eg. not every Chakra component is available, buttons should not accept arbitrary text colors, etc.
- the props we expose as interfaces are within our control. we can deliberately choose to expose props from Chakra (pick, don't omit) when they are warranted. don't expose full Chakra prop sets because we can.
- the abstraction has clear limits. Chakra is an implementation detail that we can't fully abstract away. our components libary is heavily depending on Chakra and its concepts inadvertedly leak to the consumer. you don't get around understanding Chakra concepts, eg. you have to know what tokens are, you have to know how Chakra handles responsiveness, etc.
- Consumers of components-pkg use the Chakra docs only to understand the concepts. they should not be using the Chakra docs for deriving capabilities. Only the storybook, docs and interfaces of the library itself is authoritative.
- Chakra documentation is primarily implementation material for component-library maintainers. This remains true when a library component deliberately uses Chakra naming or patterns.


### Types of components & their interfaces

leaning into Chakra props

layout primitives provide broad props
Their role is composition and layout, so they may expose a broad Chakra-like layout and styling surface. Even then, the exported library API remains the contract; a close mapping to Chakra is an intentional choice, not an obligation.

design system primitives
These expose a curated API. They're often used in layouts and therefore  may deliberately retain selected Chakra behaviour — such as disabled state, loading state, events, accessibility, controlled values, or composition—but should not automatically expose all Chakra styling props.

Their visual identity belongs to recipes and named variants. Their supported behaviour and limited layout needs belong to their public API.

Their API should be semantic and content-driven, with styling owned by their recipe. Broad generic styling props are usually a sign that a variant, slot, or lower-level primitive should be exposed instead.



## Framework choice

> We continue committing to Chakra v3

The position of this RFC is that problem is not with Chakra, but in inconsistency around components-pkg interfaces. While the idea of replacing Chakra pops up here and there, we never invested the time demonstrating an alternative path with tradeoffs, consequences and a migration plan clearly laid out - and that is telling in itself. The default position is that the existing framework remains the right choice. The burden of proof is on proposals that seek to replace it, not on repeatedly re-justifying the original decision. 

If you feel strongly against this decision, talk to your Tech Lead and work out a proposal. Otherwise, with the acceptance of this RFC, we commit to only challenging Chakra when foundational blockers leave no alternative, not based on imperfect DX or personal preference. 

We will:

- Embrace Chakra as our UI platform, including its concepts, APIs, composition patterns, and theming model.
- Get back in control on the level of components-pkg abstracting Chakra to an extent where we do not care about the underlying framework when working on composing UIs. 
- The goal is that Chakra knowledge is only needed when working on components-pkg, not on UIs
- Make agents effective at using our component library, reducing the practical influence of individual framework preferences on day-to-day implementation choices.


## Components library Philosophy

### Making sense of confusion

[TODO: wrappers are OK, even intended]


## Rules

curated restrictive, not permissive and open by default

### Vocabulary

First and foremost, shared vocabulary is consistent. We failed when props read like `isDisabled`, `disabled` and `notEnabled`. By default, we lean into the naming conventions that Chakra offers. 


: `disabled`, `loading`/`loadingText`, `size`, `variant`,
`fullWidth`, `leftIcon`/`rightIcon`, `value`/`onChange`, `open`/`onOpenChange`, and `onClick`.



With the recent migration to v3, 

1. Most Chakra layout primitives, but also some UI primitives were exposed almost one-to-one—especially. Those transparent exports are useful, but they created a misleading package-level mix of bespoke abstracted interfaces and raw low level Chakra props exposed. We would sometimes restrict, sometimes transparently pass through with no apparent reasoning or pattern. That can lead to a perception that components-pkg is intended as a thin pass-through rather layer, rather than as the owner of its public contracts. 

2. the Chakra v2-to-v3 migration introduced compatibility wrappers. These wrappers intentionally translated v2 names and behaviours to v3 internals. Because they were flagged as temporary and undesirable, the corrective instinct became “return to Chakra compatibility”. Ironically, the wrappers are actually the abstraction we seek. Application developers should not be concerned about Chakra when working in the product UI, but rely only on components-pkg documentation, Storybook examples, and exported types as the source of truth—not infer supported behaviour from Chakra documentation.


### Component set

We curate the set of components. For each component we expose, we make a decision to use an underlying Chakra compoent or compose primitives. 

### Component categories

We categorize components. Each category is clearly defined, so we can define clear rules of engagement and set expectations on the shape of the interfaces they expose.
[describe purpose]

layout primitives - design system primitives - shared product components
<------------------------------------------------------------------------->
(raw, bare metal, full styling capabilities) - (building blocks, limited styling capabilities) - (no styling capabilities)

#### Layout primitives

Examples: `Box`, `Stack`, `Grid`, `Flex`.

These are the fundamental, lowest level of building blocks we have. They're close to bare metal css, thus also closest to raw Chakra capabilities. These are meant to be gerneic, raw, unopinionated and flexible. For most leverage, these adopt a generous API surface.
[Describe this category of components]

#### Design system primitives

Examples: `Button`, `Input`, `Accordion`, `Dialog`, `Tabs`.

One level higher than layout primitives, these are actual visible components that are assembling blocks for our UIs. These are concrete components we use regularly to assemble UI components. 

only select styling capabilities. heavy use of recipes (ie. variants, sizes). deliberately restricted set of styling props ([TODO: decision] eg. props for outer layout)

[Describe this category of components]


#### Shared product components

Examples: `ArticleTeaser`, `VehicleCard`, `SearchResultItem`, `PriceSummary`.

These represent reusable product patterns. They are product-specific shared components. They're far away from being primitives. While they're reused, they're not primitives. 

[Describe this category of components]

### usecases

- you create a new component
- you are lacking a style prop
- 



## Changes

- layout primitives expose all props by default, previously limited
- pick deliberately, don't omit



naming props

per category

## ownership

Enablement is owning the defintion and guardrails
everyone can edit components. if you're violating rules bring it up with Enablement
component library is open to everyone
put yourself in component lib mode, chakra mode, maintainer mode, api designer mode
you add components and change props, fix breaking changes

### path to liberty

no breaking changes
custom tokens


### Q&A

no 1-1 parity

Is this an abstraction?
Yes but a leaky one. Intentionally.

chakra in comp pkg, comp pkg in projects


Wrappers are OK!


The intended direction is different:
components-pkg owns the API used by product teams. Chakra is the toolkit it is built on.

The library can deliberately borrow Chakra vocabulary, types, behaviours, recipes, and low-level building blocks. It can also intentionally provide transparent, Chakra-shaped primitives where that is useful. But every exported component remains a components-pkg contract: its supported props, composition, and documentation are defined by the library, not inferred from Chakra.


As a result, consumers cannot reliably tell whether a component is a transparent Chakra primitive or a curated product API. They inspect source code, trial props, add local workarounds, or build further wrappers. This increases overhead.

It is equally difficult for agents: there is no reliable feedforward about which API or composition model applies. Basic component use requires implementation-level investigation, making generated changes slower, less consistent, and more error-prone.
