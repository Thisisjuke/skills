---
name: design-system-component-api
description: Define, review, or evolve reusable design-system component APIs; use for anatomy, composition, semantic slots, variants and states, controlled or uncontrolled behavior, events, refs, polymorphism, styling escape hatches, accessibility responsibilities, or breaking migrations.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation"
---

# Design-system component API

Make a reusable component's public contract explicit before choosing its implementation shape. This skill owns cross-consumer component API decisions and their evolution. It does not own product visual direction, token semantics, framework-specific rendering, accessibility conformance, generic implementation safety or release approval.

## Establish the consumer contract

Inspect the component package, supported frameworks and runtimes, current public exports, implementation, rendered markup, documentation, tests and representative consumers. Search real usage before removing, renaming or generalizing a prop. Record which behavior is guaranteed by the component, supplied by a primitive dependency, or delegated to the consumer.

Use mature systems as comparison evidence, not automatic product authority. [Radix composition](https://www.radix-ui.com/primitives/docs/guides/composition) demonstrates the obligations created by an `asChild` seam; [React Aria Components](https://react-aria.adobe.com/) demonstrates anatomy-oriented composition; the [WAI-ARIA Authoring Practices patterns](https://www.w3.org/WAI/ARIA/apg/patterns/) describe widget semantics and interaction expectations. Match every implementation-specific claim to the installed dependency version.

## Write the API contract

For the component under change, define:

1. purpose, non-goals and supported consumer contexts;
2. anatomy, semantic parts, content model and required relationships;
3. variants, sizes, states and deliberately invalid combinations;
4. controlled and uncontrolled state, defaults, change callbacks and reset behavior;
5. events, cancellation, ref or imperative handle, focus and form participation;
6. styling surface, tokens, data attributes, slots and escape hatches;
7. accessibility guarantees and consumer responsibilities;
8. compatibility, deprecation and migration consequences.

Prefer a small contract that makes supported behavior easy and unsupported combinations difficult to express. Public types, runtime behavior, documentation and rendered semantics must describe the same API.

## Compose without leaking internals

Prefer children, named parts or semantic slots when consumers need to arrange meaningful content. Prefer explicit variants when a stable visual or behavioral choice belongs to the system. Avoid boolean-prop combinations that encode an undocumented state machine; use mutually exclusive shapes or separate components where combinations cannot be coherent.

Parts are public only when consumers have a real composition, styling or accessibility need. Name them by role rather than current DOM depth. Do not make descendant selectors, generated class names, provider internals or incidental wrappers contractual merely to support one test or override.

Offer polymorphism such as `as`, `component` or `asChild` only when consumers need to change the rendered element and the system can preserve:

- valid native semantics and keyboard behavior;
- required props and event-handler composition;
- ref attachment and imperative behavior;
- accessible names, relationships and focus management;
- framework typing and server/client constraints.

A polymorphic escape hatch transfers some obligations to the consumer. Document those obligations and reject semantically invalid substitutions. Do not add polymorphism to every leaf as a default measure of flexibility.

## Define state and interaction ownership

Controlled state comes from the controlling value and reports requested changes through its callback. Uncontrolled state begins from a default and evolves internally. Define the precedence of paired props, prohibit mode switching during one mounted lifetime where the framework requires stable ownership, and specify how form reset, remount and external changes affect state.

For compound components, define whether parts require a root, how they identify one another, and whether nesting or multiple roots are valid. For overlays and composites, specify trigger, portal, focus entry, focus return, dismissal, escape key, outside interaction, selection and disabled-item behavior. A callback must state when it fires, with what value or event, whether cancellation is supported, and whether internal behavior runs before or after consumer code.

Expose a ref to the object consumers genuinely need, not to an incidental wrapper. Prefer declarative state over broad imperative handles; when an imperative method is required, define lifetime, failure and concurrency behavior.

## Bound variants, styling and content

Create a state matrix from supported behavior rather than enumerating every possible prop combination. Consider default, hover, focus-visible, active, selected, disabled, readonly, loading, invalid, empty, overflow, long or translated content, bidirectionality, responsive layout, reduced motion and forced colors only where they apply.

Use semantic tokens and stable state or part hooks. Define precedence among base styles, variants, consumer classes or style props, and theme overrides. Escape hatches must not bypass required layout, focus visibility or security constraints silently. Treat icons, URLs, render callbacks and rich content as consumer input with an explicit sanitization and disclosure boundary where relevant.

Separate component guarantees from usage obligations. A dialog component can provide focus containment and relationships, while the consumer still supplies a meaningful title, safe initial focus and task-appropriate actions. Do not claim accessibility from ARIA attributes or a primitive dependency alone; verify the actual composed output and interaction.

## Evolve the contract deliberately

Classify a change as additive, behavior-changing, deprecated or breaking for each supported consumer and runtime. Identify deep imports, wrappers, tests and documentation that rely on incidental behavior. Provide a replacement path, compatibility window and removal condition; add a codemod only when a deterministic rewrite is possible and independently verifiable.

Do not preserve an unsafe or incoherent behavior indefinitely for compatibility. When a correction must break consumers, state the risk, migration evidence and release decision needed rather than hiding it behind a nominally compatible prop.

## Delegate implementation

When authorized source, types, stories, tests or documentation must change, call `implementation` after the public contract and affected consumers are known. Pass the API matrix, supported runtimes, compatibility decision, affected paths and verification commands. This skill retains component API and migration decisions; `implementation` exclusively owns mutation safety and diff accounting. If it is unavailable, return the contract and impact analysis and stop before mutation. The call grants no dependency installation, network, credential, publication, release or destructive authority. Do not call it when it is already active in the invocation chain; report the cycle.

## Verify and report

Exercise the public package surface, not only internal examples. Type-check valid and invalid API shapes, run behavioral and accessibility-relevant interactions, inspect rendered semantics, and test at least one representative consumer from the packed artifact when exports or runtime behavior change. Cover controlled and uncontrolled paths, ref and event composition, portals, supported state combinations and migration compatibility as applicable.

Report the component purpose and anatomy, final API matrix, guarantees versus consumer duties, supported frameworks and versions, compatibility classification, consumer evidence, commands run and unresolved platform or accessibility gaps.

## Composition boundaries

- `design-system` owns cross-layer structure, shared-system scope and governance.
- `design-system-tokens` owns token models, aliases, themes and format semantics.
- Framework skills own renderer-specific component implementation rules.
- `accessibility`, `testing`, `review` and `product-design` retain their technology-neutral methods and product decisions.
