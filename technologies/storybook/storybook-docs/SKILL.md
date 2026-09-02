---
name: storybook-docs
description: Author, configure, review, or diagnose Storybook documentation; use for Autodocs, MDX, Doc Blocks, docgen, source examples, component usage guidance, documentation builds, story manifests, or Storybook MCP context.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED storybook-build"
---

# Storybook docs

Produce executable, truthful component documentation for humans and supported agent consumers. This skill owns Storybook documentation structure, Autodocs and MDX choices, docgen accuracy, source examples and agent-facing documentation meaning. It does not own component behavior, story semantics, generic build mechanics, publication or product content strategy.

## Establish the documentation contract

Inspect the installed Storybook version, framework and builder, story and MDX globs, docs addon, tags, docgen configuration, preview annotations, existing hierarchy, static-docs commands and any story manifest or MCP integration. Identify the intended readers, supported component package version and whether the output is local, CI-built, published or consumed by agents.

Use version-matched official documentation for [Autodocs](https://storybook.js.org/docs/writing-docs/autodocs), [MDX](https://storybook.js.org/docs/writing-docs/mdx), Doc Blocks and configured AI integrations. Respect Storybook's Experimental, Preview, Stable and Deprecated lifecycle labels. Preserve stable CSF and docs APIs unless migration is requested.

## Choose executable documentation forms

Use Autodocs when component metadata, stories and args can generate an accurate standard reference. Use MDX for narrative guidance, onboarding, cross-component patterns or a deliberately customized documentation page. Do not duplicate every story in prose or replace an executable state with a screenshot.

Keep stories as the source for rendered examples. Associate MDX with supported CSF exports and use version-correct Doc Blocks. Scope project, component and story parameters at the narrowest level that expresses the shared contract. A global template must not make every component claim anatomy or controls it does not support.

## Document the public component contract

Explain purpose, supported usage, anatomy, composition, variants, states, content constraints, responsive behavior, accessibility responsibilities, required providers or styles, escape hatches and deprecations. Distinguish guarantees supplied by the component from values or context the consumer must provide.

Treat extracted prop metadata as evidence to verify, not unquestioned truth. Confirm exported names, optionality, defaults, unions, generic types, event semantics and descriptions against the public package and rendered stories. Fix the authoritative type or docgen boundary when possible instead of maintaining a contradictory manual `argTypes` shadow API.

Make examples copyable through public imports and supported setup. Ensure they typecheck or build in the documented consumer context. Do not let internal aliases, workspace-only deep imports, hidden decorators or undocumented global CSS make an impossible example appear valid.

Use controls for safe, meaningful exploration. Constrain or disable values that cannot be serialized, would trigger unsafe behavior, or imply unsupported prop combinations. Source snippets must match the example actually rendered and must not expose implementation-only or private data.

## Keep human and agent documentation aligned

When a Storybook manifest or [MCP server](https://storybook.js.org/docs/ai/mcp/overview) is configured, verify its exact version, renderer support, component identifiers, stories, args, descriptions and available test tools. Experimental docgen or agent APIs remain opt-in and cannot become a compatibility promise without project evidence.

Human pages and agent tools should derive from the same public contracts. Do not create an agent-only description that grants unsupported capabilities or bypasses consumer setup. Test representative component discovery and retrieval through the configured interface instead of assuming a generated index is useful.

Treat the preview, documentation build, story index, manifest, source snippets, static assets and MCP responses as disclosure surfaces. Exclude secrets, credentials, private fixtures, internal URLs and unnecessary source details. Authentication controls who can download an artifact; it does not make bundled values secret.

## Verify documentation as a product surface

Build or load the affected documentation with the actual renderer. Check direct links, hierarchy, navigation, headings, generated tables, controls, source examples, themes, responsive content and console output. Render every referenced story independently. If agent tooling is configured, query representative components and compare returned usage with the human page and public package.

Report Storybook/framework versions, docs mode, docgen path, pages and stories exercised, consumer examples checked, disclosure review, agent features actually queried and any output inferred rather than built.

## Delegate bounded changes

When authorized stories, MDX or documentation text must change, call `implementation` after establishing the public contract and failing evidence. Pass the affected pages, source components, supported examples and verification command; this skill retains documentation meaning while `implementation` exclusively owns mutation safety and diff accounting. When addons, docgen, story globs, MDX processing, static-docs commands, manifests or MCP configuration must change, call `storybook-build` with installed versions, configuration evidence and expected documentation artifact; it exclusively owns Storybook build mechanics and may delegate its mutation. If a target is unavailable, preserve the documentation diagnosis and stop only the affected mutation or configuration portion. Calls grant no dependency installation, external publication, credential or hosted-service authority. Do not call a target already active in the invocation chain; report the cycle.

## Composition boundaries

- `storybook` owns story format, args, decorators, loaders and portable story semantics.
- `storybook-testing` owns interaction and accessibility-test execution.
- Design-system skills own component API, tokens and system governance.
- Generic documentation skills own broader repository documentation lifecycle.
