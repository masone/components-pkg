# RFC: Separate Chakra v3 from shared component abstractions

This RFC makes a few foundational statements that will help us improving the DX of our component library. These are necessary to understand and agree upon before we plan further steps. No technical solutions at this point, just alignment.

## Problem statement

> Today’s component library has no predictable public contract.

Consumers cannot tell whether an export is raw Chakra with our branding, a bespoke shared component, or even a legacy compatibility wrapper. Chakra props may be fully supported, partially blocked, renamed, or mixed with custom behaviour; Chakra composition may be preserved or silently replaced behind an abstraction.

As a result, developers cannot safely rely on documentation or type shapes alone. They inspect source, trial props, add local workarounds, or create more wrappers. This increases implementation and review overhead, preserves v2 patterns, and makes Chakra v3 adoption inconsistent.

It is equally difficult for agents: there is no reliable feedforward about which API or composition model applies. Basic component use requires implementation-level investigation, making generated changes slower, less consistent, and more error-prone.

Using a component should not require reading its implementation first. A developer should be
able to encounter an export and know what documentation applies, what props are available, and
who owns the component’s behaviour.

## Two valid models to build a component library with Chakra

A lot of teams are using raw Chakra. When customization is required, there are two coherent models. Neither is inherently better; the mistake is blending them in a single API without telling consumers which model applies.

[TODO: two extremes]

### Model A: Chakra customization layer

The library exposes Chakra itself, configured for our brands.

- Consumers use Chakra documentation, props, compound composition, and event/detail models.
- Tokens, recipes, slot recipes, and providers create the SMG visual language.
- A recipe changes the look; it does not require a wrapper or create a second API.

This model is efficient and honest when Chakra already supplies the required behaviour.

### Model B: Owned abstraction layer

The library exposes a semantic API that it owns.

- Consumers use our documentation and our semantic props.
- Chakra is an internal implementation detail.
- The library owns the composition, behaviour, accessibility decisions, and migration burden.

This model is valuable when we provide a genuinely better reusable abstraction than Chakra,
but it is expensive. We must design, document, and maintain every bespoke API we expose.

### Where we are today: Unfortunately, an implicit hybrid

In reality, our current library mixes both models. Some exports are Chakra or nearly
Chakra; some are bespoke shared components. That is not itself a problem.

The problem is that the two models are not separated or named. They sit behind one flat public
surface, and individual exports often blend them. 

A component can look like Chakra but accept only some Chakra props. It
can expose Chakra props while rendering a different component tree. Or it can look like one of
our components but silently depend on Chakra-specific behaviour. The result is slow, uncertain
work: people read source, try props until TypeScript accepts them, or add workarounds.

Consumers can then follow neither Chakra documentation nor a small owned API with confidence.
This is an **implicit hybrid**: both valid models exist, but the public surface does not tell a
consumer which one applies and partial abstractions fill the gap.

## Design decisions

Our component library started out with a loose intent, then grew organically as delivery moved quickly. Along the way, we weakened some of the original implicit boundaries without making new decisions explicit. Today, it is difficult to tell what is intentional, what is legacy, and what “correct” looks like. We need to re-establish this foundation. 

The design decisions below are the principles I want us to challenge, discuss, and recommit to as a team. They are intended to restore clarity and guide future component work consistently. This is the moment to debate them. Once we commit and begin migrating toward them, these principles should become stable constraints, not questions we reopen component by component.

## Decision 1: Framework choice

> We recommit to Chakra v3 and its documented patterns.

Our projects are already heavily Chakra-aware. They use Chakra layout and style props,
responsive values, compound component composition, state/detail callback models, and
Chakra-specific types. The current package also exposes many of those concepts through its
public APIs. Our pain points do not come from Chakra. They come from how our library has wrapped, reshaped, and inconsistently exposed Chakra over time.

Moving to another UI framework would therefore be a full application and component rewrite,
not a dependency substitution. That kind of migration could only be insulated if this package
already provided a complete, stable, framework-neutral abstraction API. It does not.

We should not pretend the current leaky API buys us framework portability. Instead, we will:

- Embrace Chakra as our UI platform, including its concepts, APIs, composition patterns, and theming model.
- Build on Chakra v3 and remove the remaining v2-style dialect and compatibility mindset from the long-term API.
- Revisit Chakra only when foundational blockers leave no reasonable alternative. A strategic framework replacement will not be discussed based on DX or personal preferences.
- Make agents effective at using our component library, reducing the practical influence of individual framework preferences on day-to-day implementation choices.

## Decision 2: Component library model

> We embrace an explicit hybrid approach

We choose a mix of both valid models, but clearly separate their public contracts. This is an explicit
split hybrid—not a flat surface that asks consumers to infer intent from implementation details.
The concrete mechanism for making the separation visible is deliberately deferred.

| Contract | Consumer promise |
|---|---|
| **Design-system contract** | “This is Chakra v3, configured for us. Chakra documentation and composition apply.” |
| **Shared-components contract** | “This is ours. Use its documented semantic API; Chakra is internal.” |

The public boundary must carry the promise. A consumer must be able to choose a contract before
reading implementation.

Domain/product features are outside this RFC. Their ownership and placement should be decided
separately, and must not blur either shared contract.

### Contract 1: the design-system layer exposes Chakra v3

[TODO: primitives not design system]

The design-system layer answers: “How do I use our configured Chakra v3 system?”

- Its public API is the exact Chakra **v3** API.
- Its public compound composition, state model, and callback/detail shapes are Chakra v3’s.
- It preserves Chakra without transformation: no `Pick`, `Omit`, prop renames, default
  behaviour, wrapper logic, or prop filtering.
- Recipes and slot recipes are registered in the Chakra system and provide branding. A recipe
  does not need a wrapper and must not become a reason to invent a parallel API.
- Raw Chakra style props and responsive values remain available. Transparency is the contract;
  token discipline must not be implemented as an arbitrary prop whitelist.
- V2-style names and bridges do not belong in the long-term API: no `isDisabled`, `isLoading`,
  `spacing`, renamed Chakra component APIs, or synthetic v2 event models merely to make v3 look
  like v2.

Examples include Box, Flex, Stack, Grid, SimpleGrid, Center, AspectRatio, and—where we choose
the Chakra contract—Badge, Accordion, Skeleton, Table, and List.

### Contract 2: the shared-components layer exposes owned semantic APIs

The shared-components layer answers: “What reusable interaction or UI pattern do we own
beyond Chakra?”

- Public props come from native-element attributes, shared vocabulary, owned semantic props,
  and intentional recipe variants.
- Chakra prop types are never a source of public props: no Chakra type extension,
  intersection, `Pick`, or `Omit`.
- Raw Chakra style props are not public API. No `bg`, `p`, `m`, `css`, `className`, `style`,
  `colorPalette`, or `asChild` escape hatch.
- Custom components are not concerned with external layout. External layout uses the design system.  
- Valid native accessibility attributes, ids, names, and event handlers remain available.
- The component owns its composition and behaviour. Consumers do not need to understand Chakra
  slots to use it.
- A recipe/slot recipe owns the visual language where needed; the wrapper owns only the
  component’s actual behaviour and composition. 
  
  [TODO]
  
- `as` is semantic correction only, never a behavioural replacement.

Shared vocabulary is consistent: `disabled`, `loading`/`loadingText`, `size`, `variant`,
`fullWidth`, `leftIcon`/`rightIcon`, `value`/`onChange`, `open`/`onOpenChange`, and `onClick`.

Examples likely include Button, Input, Alert, Dialog, Pagination, and domain-neutral form
controls, subject to later per-component decisions.

### What this means for recipes

Recipes are visual configuration, not a third component contract.

- In the design system, a registered recipe brands the matching Chakra component. Consumers
  retain the Chakra API and composition.
- In shared components, a recipe supports an owned semantic API and internal composition.
- Recipe variants are intentional visual API. They are not a place for external layout or
  context-specific positioning.
- A manually applied recipe is justified only when a shared component genuinely owns custom
  composition. It is not a way to create a partial Chakra wrapper.

### The current symptoms this decision addresses

The split gives a direct answer to the problems seen in the inventory:

- **Partial prop surfaces:** a Chakra export cannot whitelist Chakra props; a shared component
  cannot source its props from Chakra.
- **Changed composition with Chakra props:** changing composition means the export belongs in
  shared components and must close its Chakra surface.
- **Two styling mechanisms:** registered recipes are the normal design-system mechanism;
  manual slot styling belongs only to genuinely owned composition.
- **V2 dialect persistence:** the design-system layer is explicitly Chakra v3, not a v2
  compatibility layer.
- **Unclear ownership:** the public contract says whether Chakra or the library owns the API.

## Worked examples

The following are illustrations of the two contracts. They do not prescribe the migration
sequence or final export names.

### Accordion: when Chakra composition is the API

Today a caller can write package-specific compound JSX while still passing Chakra root props:

```tsx
<Accordion defaultValue={['details']} colorPalette="blue" variant="light">
  <AccordionItem value="details">
    <AccordionButton leftIcon={<InfoIcon />}>Details</AccordionButton>
    <AccordionPanel>Content</AccordionPanel>
  </AccordionItem>
</Accordion>
```

This is confusing because `defaultValue` and `colorPalette` suggest that Chakra documentation
applies, but Chakra does not document `AccordionItem`, `AccordionButton`, or
`AccordionPanel`. The wrapper also inserts the indicator and panel slots for the caller.

Under the design-system contract, Chakra’s documented composition is the API:

```tsx
<Accordion.Root defaultValue={['details']} colorPalette="blue" variant="light">
  <Accordion.Item value="details">
    <Accordion.ItemTrigger>
      <HStack flex="1" gap="sm">
        <InfoIcon />
        <span>Details</span>
      </HStack>
      <Accordion.ItemIndicator />
    </Accordion.ItemTrigger>
    <Accordion.ItemContent>
      <Accordion.ItemBody>Content</Accordion.ItemBody>
    </Accordion.ItemContent>
  </Accordion.Item>
</Accordion.Root>
```

The registered recipe gives the visual result. Chakra owns the props, state, slots, and
accessibility; the caller composes the icon and indicator explicitly.

### Alert: when the library owns the message API

Today Alert owns the entire message structure but still accepts Chakra configuration and style
props through an omitted Chakra root type:

```tsx
<Alert
  type="warning"
  title="Your browser is outdated"
  description="Update to continue."
  dismissible
  colorPalette="orange"
  bg="orange.50"
/>
```

The first four props describe an owned message component. The last two are Chakra styling
escapes. Mixing them means neither contract is clear.

Under the shared-components contract, the message API is all that callers see:

```tsx
<Alert
  status="warning"
  title="Your browser is outdated"
  description="Update to continue."
  dismissible
/>
```

Alert owns its title, description, icon, action, close behaviour, recipe, and internal Chakra
composition. A caller who needs external spacing, background, or placement uses the
design-system layer around it.

### Badge: when a recipe is enough

Today the wrapper adds only a `text` prop on top of a Chakra component:

```tsx
<Badge text="New" variant="navigationLinkBadge" />
```

It has no custom behaviour or composition. The recipe already supplies the visual variants.

Under the design-system contract, the caller uses Chakra children directly:

```tsx
<Badge variant="navigationLinkBadge">New</Badge>
```

There is no need for a second Badge API merely to turn children into `text`.

## Scope of this RFC

### Goals

- Make component API expectations predictable before source inspection.
- Recommit the design-system layer to Chakra v3.
- Preserve Chakra where Chakra is the intended developer experience.
- Make shared components genuinely owned, semantic, and documentable.
- Stop new partial abstractions, prop whitelists, and composition/API mismatches.
- Give humans and agents a reliable decision procedure for new exports.

### Not decided here

- Which existing exports move to design system versus shared components.
- How current APIs are migrated, deprecated, codemodded, or removed.
- Lint rules, CI checks, and other enforcement mechanics.
- Feature/domain ownership.
- Detailed controlled-state, adapter-removal, and react-hook-form decisions.

## Alternatives considered

### Use only Chakra customization

This is consistent and cheap, but discards or relocates useful owned abstractions such as Alert.

### Hide Chakra behind a full abstraction layer

This is also consistent, but it commits us to inventing, documenting, migrating, and
maintaining a bespoke API for every Chakra interaction and compound component. It is a valid
strategic choice, but much larger than the current package implies.

### Keep the current flat, implicit model

This retains local convenience but preserves documentation mismatch, type leakage, and API
surprises. It is rejected.

### Clearly separated hybrid — proposed

Clearly separating the valid Chakra and owned-abstraction models makes the intended contract
discoverable before source inspection. This preserves Chakra where it is reasonable and supports
genuine shared abstractions where they add value.

## Consequences

- A Chakra component in the design-system layer means Chakra v3 rules apply exactly.
- A shared-component name promises a closed, owned API; consumers use the design system for
  layout and Chakra-level composition.
- Existing ambiguous exports are migration work, not precedent for future APIs.
- Making the separation concrete and migrating existing exports will have cost, but it replaces
  recurring uncertainty with a durable, understandable contract.
