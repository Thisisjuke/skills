---
name: design-system-review
description: Review cross-cutting design-system changes across token, component, documentation, governance, compatibility, migration, generated-artifact, and consumer layers.
license: MIT
---

# Design system review

Provide a design-system-specific review lens. It does not own generic review procedure, implementation, full accessibility evaluation or fresh visual inspection.

## Establish authority and consumers

Identify authoritative design and code sources, generated outputs, affected packages, supported platforms, maturity, owners, release policy and real consumers. Trace the change through tokens, components, docs and artifacts rather than reviewing one file in isolation.

Record the installed toolchain and declared token-format version. Use version-matched specifications for interchange semantics and real consumer evidence for compatibility. External design systems can show mature alternatives, but they do not override local product meaning.

## Inspect

- whether the change represents a reusable need rather than one product exception;
- duplication or contradiction with existing tokens, primitives, components or patterns;
- semantic token names, types, aliases, modes and platform transformations;
- component anatomy, composition depth, variants, states, controlled/uncontrolled behavior, event/ref semantics, content rules and invalid combinations;
- public API compatibility, theming, overrides and escape hatches;
- documentation that matches the actual runtime setup and supported behavior;
- generated artifacts aligned with their source, deterministic, free of unexplained churn and importable from the packed public surface;
- accessibility responsibilities assigned to system versus consumer;
- ownership, maturity, evidence, deprecation and migration path;
- blast radius across brands, themes, platforms and downstream versions;
- dependency placement, duplicate framework runtimes, global style side effects, client/server boundaries and unintended deep imports;
- token or asset inputs that can inject unsafe CSS, URLs, markup, SVG behavior, secrets or private content into consumer artifacts;
- whether an apparent system addition is really a local product exception that should remain outside the shared contract.

Require a concrete inconsistent contract, consumer failure, governance gap or migration risk. Do not block a valid local exception merely because it is not reusable; keep it outside the shared system instead.

Use `design-system-component-api` when the primary outcome is to define, reshape or migrate one component's public API. This review skill retains the cross-layer impact lens and should not duplicate that API design workflow.

For breaking or deprecating changes, require an affected-consumer inventory, replacement semantics, compatibility window, migration path and removal signal. A codemod proves only syntax migration; behavioral and visual changes need their own evidence.

## Report

Give location, system layer, trigger, affected consumers, consequence and smallest safe direction. Distinguish authored cause from generated symptom, format conformance from product correctness, and required fix from optional system evolution. State whether consumer/runtime evidence was executed or inferred. When combined with `review`, let it own severity and final disposition.

## Composition boundaries

`design-system-component-api` owns focused public component contracts. `design-system-tokens` owns token semantics and resolution. `design-system-visual-review` specializes rendered consistency. `design-system-accessibility` owns reusable accessibility behavior. `design-system-testing` owns executable system invariants. `design-system-build` owns transformation and packaging mechanics.
