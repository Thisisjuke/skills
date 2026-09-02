---
name: storybook-build
description: Configure, build, upgrade, or diagnose Storybook development and static outputs while preserving framework, builder, addon, asset, environment, and deployment-artifact contracts.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation"
---

# Storybook build

Produce a runnable development or static Storybook from the repository's actual configuration. This skill owns Storybook framework, builder, addon and artifact mechanics. It does not own component behavior, story quality, generic mutation safety or deployment.

## Discover configuration

Inspect `.storybook/main.*`, preview and manager annotations, manifests, lockfiles, framework integrations, builder config, story globs, static directories, environment variables, scripts, CI and hosting base paths. Keep Storybook packages on a compatible version line and use the project's package manager.

Distinguish configuration evaluated by Node from code bundled for the preview. Treat the generated Storybook, source snippets, story index, source maps and copied static files as publishable client artifacts. Do not expose server secrets through preview variables or assume a development server proves a static build. Authentication around a deployment controls access; it does not remove secrets already bundled into downloaded files.

Read official release and migration notes for every crossed major. The [Storybook 10 migration guide](https://storybook.js.org/docs/releases/migration-guide) requires ESM configuration and gives a Node baseline of 20.19+ or 22.12+. When starting below 9, migrate through 9 before 10 and run the official automigrations plus repository tests rather than hand-editing only the manifest. Keep all core packages and compatible addons on the intended version line.

## Preserve build meaning

- Keep framework and builder settings supported by the installed version.
- Include only required addons and inspect addon code as executable build input.
- Preserve aliases, plugins, CSS processing and asset rules needed to render components consistently with their application.
- Keep story discovery deterministic and avoid scanning generated, vendor or secret-bearing trees.
- Treat upgrades as migrations across config, addons, stories, test APIs and CI rather than a package-number edit.
- Verify base URLs, asset paths and direct deep links in the static artifact.
- Audit the artifact for unexpected environment values, credentials, private fixtures, absolute machine paths and source maps before any publication. Storybook's [2025 environment-variable disclosure advisory](https://storybook.js.org/blog/security-advisory/) affected released 7.x through 10.x lines; require a patched release on the selected major and re-check current advisories instead of relying on a historical minimum forever.
- Keep production and Storybook transforms close enough to exercise real components, while making Storybook-only aliases and mocks explicit so they cannot leak into the application build.
- Use test-oriented build optimizations such as `--test` only when the artifact is exclusively for automation; do not publish an intentionally reduced documentation build as the canonical Storybook.
- Generate optional story manifests or agent/MCP surfaces only when a named consumer requires them, and verify renderer/version support plus the information they expose.

## Change safely

When configuration, manifests, lockfiles or scripts must change, call `implementation` with the diagnosed build boundary, installed versions, expected artifact and verification command. This skill retains Storybook build decisions; `implementation` exclusively owns mutation safety and diff accounting. If unavailable, return the build diagnosis and stop before mutation. No call grants install, network, credential, publication or deployment authority. Do not call a target already active in the invocation chain; report the cycle.

## Verify and report

Run the narrow native command, then inspect development and/or static output as required. For an upgrade, run migration health checks and search for deprecated configuration or imports. Report Node and Storybook versions, framework, builder, addons, commands, artifact path, security/advisory check, warnings, direct-load behavior and unverified hosting assumptions.
