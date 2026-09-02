---
name: shadcn
description: Use, add, compose, customize, update, review, or diagnose shadcn/ui components and projects from their local source, detected framework, style, base library, aliases, Tailwind major, registry provenance, and installed component APIs.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED tailwindcss"
---

# shadcn/ui

Treat shadcn/ui as open component source and a code-distribution system, not as an opaque component package. This skill owns use and evolution of components copied into a project, their local composition contract and safe CLI-assisted component workflows. It does not own ecosystem discovery, registry production, Tailwind internals, large third-party feature engines or generic mutation safety.

## Resolve the project before choosing an API

Work in the exact application or UI workspace. Read its `components.json` when present, manifest and lockfile, installed component source, global CSS, aliases, framework and renderer conventions. Determine:

1. the shadcn CLI version or invocation policy;
2. the style or preset and icon library;
3. the component base—Base UI, React Aria or Radix—and its resolved versions;
4. Tailwind major, CSS entrypoint and theme strategy;
5. server/client boundary and package export path;
6. local changes to copied components and their consumers.

`components.json` is required for CLI addition but optional for copy-and-paste projects. In a monorepo, the repository root may not identify a unique target; select the workspace explicitly before using `info`, `docs`, `diff` or `add`. Never invent aliases or infer the base from an old example.

The 2026 shadcn line supports Base UI, React Aria and Radix, with Base UI the default for new projects and existing projects remaining on their selected base. Re-check the official [changelog](https://ui.shadcn.com/docs/changelog), [project configuration](https://ui.shadcn.com/docs/components-json), component docs and resolved local source. Base-specific composition, events and state selectors are not interchangeable.

## Use local source as the runtime truth

Once installed, the project's component file is the implementation consumers execute. Read it before changing a call site. Official docs establish upstream intent; local code establishes supported props, variants, slots, imports and divergences. Do not replace a customized component with a remembered upstream version.

Prefer an installed primitive or documented composition before adding a duplicate. Use built-in variants and semantic tokens where they express the product contract. Compose feature-specific structure at the feature boundary; change the shared primitive only when the new behavior should become part of its supported system contract.

Preserve native semantics, accessible names, keyboard and focus behavior supplied by the selected base. When using slot, child-rendering or trigger composition APIs, verify the base-specific mechanism and final DOM. Do not nest interactive elements, swallow refs or handlers, or assume an accessible primitive makes feature content accessible.

## Inspect before adding or updating

The official [CLI](https://ui.shadcn.com/docs/cli) provides project info, documentation lookup, search, registry viewing, dry runs and diffs. Prefer read-only evidence first. If CLI execution and network access are authorized, use the package runner declared by the project and record the resolved CLI version. Do not silently execute a moving `@latest` binary merely to inspect a repository.

Before an `add`, preset application, migration or reinstall:

- inspect the target item, files, ordinary and registry dependencies;
- preview the exact workspace and paths with supported view, dry-run or diff facilities;
- compare every existing target file and preserve local ownership decisions;
- review manifest, lockfile, global CSS and configuration effects;
- avoid non-interactive confirmation, overwrite and all-component flags unless their complete effect is intended;
- keep an existing clean checkpoint or otherwise recoverable diff.

Treat an upstream update as a three-way merge among local source, current consumers and new upstream behavior. Update one coherent slice at a time, then test its affected states. The irreversible `eject` command pins shared Tailwind CSS locally and ends automatic updates for that import; run it only when explicitly requested and after recording the maintenance consequence.

## Compose screens without creating a shadow system

Build complex UI from documented primitives and small feature components. Keep domain data, loading and error ownership outside low-level UI primitives. Model empty, pending, invalid, success, destructive confirmation, overflow, narrow viewport, localization and permission states explicitly.

Use semantic color roles and existing spacing, radius, typography and motion contracts. Do not sprinkle raw palette values or one-off overrides through feature markup when the appearance is system-owned. When a new visual variant is stable and reusable, add it to the local variant contract and exercise all affected consumers; when it is feature-only, keep it local.

For non-trivial Tailwind utility, theme, custom-variant or source-detection work, delegate to `tailwindcss`. shadcn retains the component and composition contract; Tailwind owns utility and theme semantics.

## Diagnose at the owning layer

Classify a problem before editing:

- wrong component API or imports: compare local source, selected base and current docs;
- missing styles: trace CSS entrypoint, Tailwind major, theme variables and source detection;
- broken composition: inspect rendered DOM, event/ref merging and selected base semantics;
- unexpected appearance: identify local variant, consumer class, cascade or token owner;
- CLI failure: confirm target workspace, aliases, registry configuration and network/auth boundary;
- update regression: diff the locally owned fork against the exact upstream item and consumer expectations.

Do not repair an alias or styling symptom by reinstalling every component. Do not migrate Base UI, React Aria or Radix as an incidental fix.

## Delegate bounded changes

When authorized component source, feature composition, tests or call sites must change, call `implementation` after gathering the workspace, resolved base and versions, local component contract, affected consumers, expected DOM/states and verification commands. This skill retains shadcn-specific decisions; `implementation` exclusively owns mutation safety and diff accounting. If unavailable, preserve the analysis and stop the mutation portion.

When Tailwind utilities, theme variables, custom CSS, variants or scan-safe class design materially change, call `tailwindcss` before mutation with the Tailwind major, CSS entrypoint, semantic intent, affected states and local shadcn conventions. It exclusively owns Tailwind semantics; this skill retains component behavior and integration. If unavailable, stop that styling portion and report the gap.

Calls do not authorize downloads, component installation, dependency writes, preset application, migration, overwrite, publication or deployment. Do not call a target active in the invocation chain or an ancestor; report the cycle and keep collected evidence.

## Verify and report

Run repository-native type, lint and behavioral checks and inspect representative rendered states. Verify keyboard and pointer interaction, focus restoration, accessible names, themes, narrow layouts, portals, disabled/invalid states and server/client boundaries where relevant. For an addition or update, account for every created or changed source, configuration, dependency and lockfile entry.

Report the target workspace, framework, component base, style/preset, Tailwind and CLI versions, components used or changed, local divergences preserved, commands and previews, verification evidence and any upstream or environment behavior not exercised.

## Composition boundaries

- `discover-shadcn` owns current community search, qualification and shortlisting.
- `shadcn-registry` owns authoring, serving and versioning registries.
- `shadcn-data-table` owns the integration of local Table primitives with TanStack Table.
- `design-system` owns product-wide component, token and governance decisions.
- Renderer and framework skills own their runtime, routing and server/client semantics.
