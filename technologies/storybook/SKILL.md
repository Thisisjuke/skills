---
name: storybook
description: Author or analyze portable Storybook component stories; use for Component Story Format, args, decorators, loaders, play interactions, deterministic mocks, state coverage, and renderer-neutral development workflows against the installed version.
license: MIT
---

# Storybook

Use stories as reproducible component examples through the project's installed Storybook framework and version. This skill owns story semantics and authoring constraints. It does not own component implementation, generic testing, build configuration, visual design or a particular renderer such as React.

## Establish the Storybook contract

Inspect package and lock files, `.storybook` configuration, framework and builder, story globs, addons, preview annotations, TypeScript settings, existing stories and repository commands. Determine whether the task targets local development, documentation, tests, static output or downstream story reuse. Never assume current documentation matches an older installation.

Use the installed version's official [Storybook documentation](https://storybook.js.org/docs) and migration notes as primary authority. Use renderer documentation for component behavior and addon documentation only for the installed addon version. Treat CSF Next, experimental annotations, agent/MCP features and community recipes as conditional until the repository enables a supported form. Do not migrate stable CSF merely to adopt a preview API.

Storybook 10.x documentation is ESM-first and its current testing path is the Vitest browser integration, but older supported repositories can have different valid seams. Preserve their working integration unless an upgrade is in scope.

As of the [10.4 release line](https://storybook.js.org/blog/storybook-10-4/), CSF Next is still a preview, React Component Meta for MCP is experimental, and agent-facing component documentation remains renderer-sensitive. Re-check the release and framework documentation before making any of those a project contract.

## Write deterministic stories

- Prefer the installed version's supported Component Story Format and typed story APIs.
- In TypeScript, preserve literal inference with the version-supported `Meta` and story-object types; do not use casts to conceal invalid args.
- Represent one meaningful state per named story; use clear args rather than duplicating render code.
- Keep component metadata, args, arg types, parameters, decorators, loaders and render functions at the narrowest shared scope.
- Supply required providers, themes, routing and localization through reusable annotations without hiding story-specific preconditions.
- Prefer args for ordinary data. Use loaders only for asynchronous pre-render data that cannot be expressed as args, remembering that loaders at the same level may run in parallel rather than in declaration order.
- Mock external modules, time, randomness and network boundaries deterministically. Use the installed Storybook mock API at stable module boundaries and reset mutable effects in the supported per-story lifecycle. A story must not depend on execution order, mutable production state or another story having run first.
- Use `play` functions for user-observable interactions and assertions through the story context's canvas and test APIs; group meaningful actions with the supported step API. Do not use `play` as an opaque setup script or reach into component internals.
- Cover empty, loading, error, long-content, responsive and interactive states that are part of the component contract.
- Make controls truthful: exclude unsafe or meaningless inputs, document constraints, and never imply that an impossible prop combination is supported.

Stories document examples, not every product permutation. Prefer a discriminating state matrix over a cartesian explosion of props. Preserve portable stories when consumers reuse the story outside Storybook.

## Keep documentation truthful

Explain purpose, supported variants, constraints and usage rather than restating prop names. Ensure source examples compile and decorators do not make impossible usage appear valid. Separate docs-only pages from executable component states when their contracts differ.

Treat every preview and static Storybook as client-readable. Never put credentials, private fixtures or unrestricted environment values in stories, parameters, source snippets or preview bundles. Documentation indexes, story manifests and agent-facing endpoints expose only what the hosting policy permits. If current MCP documentation tools are renderer-limited, report that boundary instead of claiming equivalent support elsewhere.

Use `storybook-docs` when Autodocs, MDX, Doc Blocks, docgen, documentation builds, manifests or agent-facing component context are the primary outcome. Keep the disclosure guard here because ordinary stories also feed those surfaces.

## Verify

Load affected stories with the actual renderer, inspect console and accessibility output, run supported interactions, and verify isolation by visiting or testing stories independently and in a different order. Exercise direct story URLs when portability or hosted review matters. Report Storybook version, framework, builder, addons, test integration and any behavior inferred rather than executed.

## Composition boundaries

- `storybook-build`, `storybook-testing`, `storybook-review`, `storybook-docs` and `storybook-visual-testing` own their specialized mechanics.
- Renderer skills such as `react` own component semantics.
- Design-system skills own token, component API and governance decisions.
- Generic `testing`, `review`, `accessibility` and visual-review skills own their cross-technology methods.
