---
name: design-system
description: Define or evolve cross-technology design-system structure, reusable component and pattern contracts, runtime alignment, and governance; use when work spans system layers or decides what belongs in the shared system.
license: MIT
---

# Design system

Treat a design system as a governed product contract connecting design decisions, implementation and consumer guidance. This skill owns system structure, semantic consistency and evolution. It does not own product visual direction, generic implementation, a particular UI framework, build pipelines, testing methodology or accessibility conformance.

## Establish the system contract

Locate the authoritative token sources, component packages, documentation, design assets, release policy, supported platforms and consumer examples. Identify owners and consumers, current versions, generated versus authored artifacts, themes or brands, contribution states and deprecation policy. Surface contradictions instead of choosing whichever source is easiest to edit.

Rank evidence explicitly: adopted repository contracts and rendered consumers establish local truth; version-matched platform and format specifications define external semantics; mature public systems provide comparison patterns, not authority over the product. Record the version behind any compatibility claim. The [Design Tokens Community Group 2025.10 format report](https://www.w3.org/community/reports/design-tokens/CG-FINAL-format-20251028/) is a stable interchange reference, but a tool implements it only when its documented version and round-trip evidence say so.

## Model decisions and tokens

- Name tokens by semantic role when consumers should survive value changes; keep raw scales or primitives distinct from contextual aliases.
- Use a deliberate layer model—primitive values, semantic decisions and narrowly justified component tokens—without forcing every project to expose every layer publicly.
- Preserve type, unit, color space, mode, alias and fallback semantics across tools.
- Avoid circular aliases, orphaned tokens and platform transformations that silently change meaning.
- Distinguish global decisions, component tokens and consumer overrides.
- Do not create a token for every isolated value or bypass an existing semantic token for local convenience.
- Do not infer semantic meaning from a file path or group name when the adopted token format does not define that inference.

When the project adopts a token exchange format, follow its declared version and extensions. Preserve unknown extensions unless the transformation contract explicitly rejects them. A token file is not by itself the complete design system, and format conformance does not prove that token names or values are good product decisions.

Use `design-system-tokens` when token modeling, DTCG semantics, aliases, resolver contexts, theming or token migration is the primary outcome. Keep the semantic layer rules here because component and governance work must still respect them.

## Design component and pattern contracts

Define purpose, anatomy, content, supported variants, interactive states, responsive behavior, theming, accessibility responsibilities and escape hatches. Prefer primitives and composition where consumer needs vary; add a variant only when it represents a stable supported contract. Exclude combinations that cannot be made coherent.

Keep APIs shallow enough for ordinary use and deep enough for real composition. Expose semantic slots, parts or child composition when consumers must arrange content; avoid multiplying booleans whose combinations encode an undocumented state machine. Specify controlled and uncontrolled behavior, event ordering, ref or handle semantics, form participation, loading and async behavior, and which DOM/native structure is contractual. Do not expose internal implementation details merely to make a test or one product exception convenient.

Define the full state and content contract where applicable: default, hover, focus-visible, active, selected, disabled, readonly, loading, success, warning, invalid, error, empty, overflow, localization, bidirectionality and reduced-motion or forced-color behavior. Only supported states belong in the matrix; omissions must be intentional rather than accidental.

Separate components from higher-level patterns. A component exposes reusable mechanics; a pattern explains how components and content solve a recurring user need.

Use `design-system-component-api` when a component's anatomy, slots, public variants, state ownership, events, refs, polymorphism, styling surface or migration is the primary outcome. Keep the cross-layer contract here because system governance still decides whether the component belongs in the shared system.

## Keep design and runtime aligned

Trace a decision from source through transforms, packages, providers or styles to rendered output. Verify that documented imports, required styles, fonts, icons, contexts and themes actually activate the system. Do not declare conformance merely because code references approved names.

Prefer safe defaults and explicit escape hatches. Do not let theme input, icons, rich content, URLs or generated CSS bypass the consumer application's security boundary. Keep runtime global effects, reset styles and provider nesting documented and bounded. A component package must not silently replace the application's framework runtime or accessibility semantics.

## Govern evolution

Require evidence of broad usefulness and non-duplication before expanding the public system. Record maturity, owner, supported contexts and known gaps. For breaking changes, identify affected consumers, replacement path, compatibility window, codemods or migration evidence as applicable. Deprecation is a lifecycle with communication and removal criteria, not a label alone.

Use contribution and maturity states that change what consumers may rely on: proposal, experimental, supported, deprecated and retired, or the repository's equivalents. Measure adoption and unresolved exceptions; a catalogue entry with no maintained consumer is not automatically a successful system primitive.

## Verify and report

Use the repository's actual source, build and rendered evidence. Exercise at least one representative external consumer when public imports, runtime setup or compatibility changes. Report authoritative inputs and versions, consumer scope, system decision, affected tokens/components/patterns, compatibility and migration impact, evidence gathered and unresolved divergence.

## Composition boundaries

- `product-design` owns product intent and visual direction.
- `design-system-tokens`, `design-system-component-api`, `design-system-build`, `design-system-testing`, `design-system-accessibility`, `design-system-review` and `design-system-visual-review` own focused mechanics.
- Framework, shadcn/ui and Tailwind CSS skills realize system contracts in
  their technology layers without owning product-wide governance.
- Storybook may document or exercise a system but is not a hidden dependency.
