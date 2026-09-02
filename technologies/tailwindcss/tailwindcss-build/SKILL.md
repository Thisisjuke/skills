---
name: tailwindcss-build
description: Configure, migrate, build, or diagnose Tailwind CSS installation, CSS entrypoints, Vite, PostCSS or CLI integration, source detection, monorepo inputs, browser compatibility, and emitted production CSS for the installed major.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation"
---

# Tailwind CSS build

Make the Tailwind pipeline explicit from source files to the CSS delivered to a browser. This skill owns dependency and plugin alignment, CSS entrypoints, source discovery, migration mechanics and emitted artifacts. It does not own utility design, product tokens, framework behavior, deployment or generic mutation safety.

## Inventory before changing the pipeline

Read the manifest and lockfile, package-manager declaration, installed Tailwind and plugin versions, framework and bundler configuration, CSS entrypoints and import order, workspace boundaries, production scripts, browser targets and generated asset evidence. Locate every Tailwind stylesheet and identify which route or application consumes it.

Use documentation matching the installed major. In v4, the official Vite plugin is `@tailwindcss/vite`, the PostCSS plugin is `@tailwindcss/postcss`, the CLI is `@tailwindcss/cli`, and CSS normally imports `tailwindcss`. In v3, configuration and integration differ. Do not mix these families or infer the active path from a stray config file.

Tailwind v4 is itself the CSS build step and is not designed to run through Sass, Less or Stylus. Confirm the [compatibility contract](https://tailwindcss.com/docs/compatibility) and the framework's current [installation guide](https://tailwindcss.com/docs/installation/framework-guides) before selecting an integration.

## Choose one supported integration

- Prefer the official Vite plugin in a Vite pipeline when the framework supports it.
- Use the dedicated PostCSS plugin when PostCSS is the established integration boundary.
- Use the dedicated CLI for a direct input-to-output workflow without a suitable bundler integration.
- Preserve a framework-specific integration when its current docs own the setup.
- Treat the Play CDN as a development experiment, not a production build path.

Keep a single owner for compiling each CSS entrypoint. Duplicate plugins or nested compilation can produce repeated Preflight, confusing import order, slow rebuilds or divergent development and production output.

## Make source detection intentional

Trace every class-producing path: application templates, shared component workspaces, installed source packages, generated files and registry-copied code. Tailwind's detector sees text, not the language AST. Prove that each required complete candidate is reachable from the stylesheet's scan boundary.

For v4, set `source()` when the build working directory is not the correct base, add external sources with `@source`, exclude known irrelevant trees with `@source not`, and use `source(none)` only when every source will be declared explicitly. Use `@source inline()` for a bounded external candidate set, not to compensate for arbitrary runtime interpolation. Avoid scanning caches, generated output, dependency trees or secrets merely to make a missing class appear.

In a monorepo, distinguish the command working directory, stylesheet location, package exports and application consumption. Test at least one class used only by a shared workspace package; a successful local app class does not prove the shared source is scanned.

## Migrate majors as a product change

Before v3-to-v4 migration, confirm the browser contract and Node runtime. The official upgrade tool requires Node.js 20 and can change dependencies, configuration, CSS and templates. Run it only when migration and dependency writes are authorized, from a recoverable branch or checkpoint, then inspect the complete diff.

Review at least PostCSS/CLI package changes, CSS imports, removed directives, theme configuration, source paths, default border and ring behavior, prefix and important syntax, variant stacking, arbitrary-variable syntax and any compatibility CSS. Preserve v3.4 when v4's browser baseline is not acceptable. Do not combine a major migration with unrelated redesign.

## Diagnose from artifacts

Classify failure by seam:

1. plugin absent, duplicated or loaded by the wrong config;
2. CSS entrypoint not imported by the affected application;
3. source path or complete candidate not detected;
4. theme variable or custom utility not registered;
5. cascade, Preflight, specificity or third-party CSS wins;
6. development and production commands build different graphs;
7. asset caching or deployment serves an older artifact.

Use the smallest reproduction and inspect the generated CSS for the exact selector and declarations. A successful command does not prove that the browser receives the artifact. Compare development and production builds when minification, source detection or route-level CSS loading differs.

## Delegate mutation safely

When authorized manifests, lockfiles, CSS entrypoints, build configs, scripts or source declarations must change, call `implementation` after capturing the resolved versions, current command, failure seam, intended pipeline, browser constraints and verification command. This skill retains Tailwind build decisions; `implementation` exclusively owns mutation safety and diff accounting. If unavailable, preserve the diagnosis and stop the mutation portion.

Calls do not authorize downloads, package installation, migration, lockfile rewrites, deployment or cache invalidation beyond the user's scope. Do not call `implementation` if it is active in the invocation chain or an ancestor; report the cycle and retain evidence.

## Verify and report

Run the repository's development and production CSS paths as risk requires. Confirm the affected route imports the expected stylesheet, shared-package classes compile, variants and theme values appear, browser targets are supported, and no duplicate global reset or major unexplained size increase was introduced.

Report the Tailwind and plugin versions, selected integration, CSS entrypoints, source roots, browser baseline, commands, generated artifact evidence, migration decisions and any environment or deployment path not exercised.

## Composition boundaries

- `tailwindcss` owns utilities, variants, themes and rendered styling semantics.
- Framework build skills own their wider compiler, routing and deployment contract.
- `design-system-build` owns multi-platform token and component package delivery.
- `implementation` owns authorized file mutation, not the Tailwind pipeline decision.
