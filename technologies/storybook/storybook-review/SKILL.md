---
name: storybook-review
description: Review Storybook stories and configuration for state coverage, isolation, determinism, args, decorators, documentation, interactions, portability, and maintainability against the installed version.
license: MIT
---

# Storybook review

Provide a Storybook-specific review lens. Generic review framing, component correctness and visual judgment belong to their selected owners.

## Establish scope

Inspect installed Storybook, framework, builder, addons, configuration, affected components and existing story conventions. Review rendered implications as well as the story file; version-sensitive APIs must match the project.

Use official documentation for the installed line and renderer as the API authority. Identify whether an apparent convention is stable CSF, an enabled experimental feature, generated output or legacy compatibility before recommending a rewrite.

## Inspect

- meaningful coverage of variants, states, content extremes and recovery paths;
- deterministic loaders, mocks, time, randomness, storage and network behavior, including cleanup between independently executed stories;
- args that represent public component inputs rather than internal shortcuts;
- decorators and providers scoped without hiding impossible consumer setup;
- play functions that interact and await within the correct canvas, assert user-visible outcomes and do not serve as opaque setup;
- accessible queries, names, focus and keyboard-visible behavior;
- portable annotations and absence of Storybook-global coupling where reuse is intended;
- docs and source examples that match executable usage;
- config, addon and story patterns supported by the installed version;
- explicit handling of loading, failure, retry, disabled, overflow and responsive behavior where part of the public contract;
- no secrets, unrestricted environment values, private fixtures or misleading source snippets in client-readable preview or static output;
- accessibility parameters whose `off`, non-blocking or error behavior matches the stated CI policy rather than silently reducing coverage;
- optional story-manifest or agent/MCP output that is version-supported, intentionally exposed and accurate for the renderer;
- redundant permutations, snapshots or generated boilerplate with no distinct evidence.

Report only a concrete failure, misleading example, maintenance hazard or missing high-value state. Do not require every prop combination or impose React conventions on another renderer.

For configuration or dependency changes, additionally check compatible package lines, ESM/Node requirements on Storybook 10 migrations, current security advisories and whether development success was incorrectly substituted for a static-build check.

Use `storybook-visual-testing` when the primary outcome is capture coverage, baseline policy, diff triage or flaky-pixel diagnosis. Use `storybook-docs` when documentation generation or output is the primary outcome. This skill keeps only the review evidence needed to detect those specialized risks in a broader change.

## Report

Give location, affected story or consumer, Storybook mechanism, trigger, consequence and smallest safe direction. State what was rendered, executed or inferred. When combined with `review`, let it own severity and disposition.
