---
name: react-build
description: Inspect, configure, build, or package React applications and component libraries while preserving their framework, bundler, JSX, module, dependency, asset, and consumer contracts.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation"
---

# React build

Produce React artifacts through the repository's actual framework and toolchain. This skill owns React-facing build topology, JSX/compiler transforms, renderer dependency placement, entrypoints, client/server graphs and consumable packages. It does not own component semantics, test design, generic mutation safety, release approval or deployment.

## Discover the build contract

Inspect manifests, lockfiles, workspace configuration, framework and bundler files, TypeScript/JSX settings, compiler plugins, exports, CI and publishing scripts. Identify:

- application, embeddable widget or library output;
- client-only, SSR, SSG, streaming or RSC execution;
- package manager, Node and browser requirements;
- client/server entrypoints and module conditions;
- CSS, font, image and other asset ownership;
- environment-variable exposure and source-map policy;
- public package consumers and supported bundlers.

Use the checked-in package manager, wrapper and scripts. Do not replace a framework or bundler, regenerate a lockfile with another tool, or select the newest version without an explicit upgrade request. [Create React App is deprecated](https://react.dev/blog/2025/02/14/sunsetting-create-react-app) for new apps, but that does not authorize migrating a working existing app. For a new app, choose a framework or build tool from its runtime, routing, data, rendering and deployment requirements rather than popularity alone.

## Keep the React stack coherent

- Resolve the versions actually selected by the lockfile. Keep `react`, renderers, RSC transport packages and framework integrations on compatible, patched lines.
- Check official React and framework security advisories before changing an RSC or Server Function build; the 2025–2026 advisories demonstrated that vulnerable transport packages can be present even when application code defines no Server Function.
- Preserve the automatic versus classic JSX runtime and development transforms. A TypeScript typecheck does not prove JSX was emitted or transformed correctly.
- Configure React Compiler only through a supported framework/bundler integration. Run compatibility diagnostics, gate rollout in existing apps, retain lint rules and compare behavior and build cost.
- Keep Fast Refresh development-only and avoid module side effects that invalidate its component-boundary assumptions.

## Preserve client/server graphs

Treat `'use client'` and `'use server'` as graph boundaries interpreted by a supporting framework, not general JavaScript directives. Verify that server-only dependencies, credentials and filesystem/database modules cannot enter a client bundle. Minimize data serialized to client components and preserve supported serializable types.

Do not implement RSC transport directly from generic React packages unless that integration is the project's explicit responsibility; React recommends frameworks pin RSC integrations to exact versions because their bundler APIs may change between minors.

Audit environment injection at the final client artifact. Prefix conventions indicate intentional exposure, not secrecy. Public source maps, inline configuration and static Storybook-like assets can disclose source or values even when runtime access is restricted.

## Package reusable libraries

- Keep React external and normally peer-owned so consumers do not load a second reconciler copy.
- Align `react` and renderer peer ranges with APIs actually used; do not claim older compatibility that was never built and tested.
- Preserve ESM/CJS policy, conditional exports, types, subpath exports, CSS/assets, side-effect declarations and tree-shaking behavior.
- Make client-only entrypoints explicit without marking an entire mixed package client-side unnecessarily.
- Test the packed tarball or equivalent, not only the workspace symlink, in representative consumers.
- Avoid broad barrel entrypoints when they force unrelated modules or server/client code into one graph; measure rather than assuming every barrel is harmful.

Treat code generation, plugins and install scripts as executable inputs. Inspect them before use, keep dependency edits narrow and require separate authority for network resolution.

## Verify artifact behavior

Verify development startup separately from production build, SSR/streaming output, hydration and package consumption. Inspect bundle composition, duplicated React, secret-bearing strings, emitted module conditions, types, CSS/assets and source maps as applicable. A successful compiler exit does not prove browser execution, server isolation or a valid published package.

## Change safely

When configuration, manifests, lockfiles or source must change, call `implementation` after identifying the exact build cause. Pass the intended artifact, current toolchain, affected files, dependency constraints and reproduction evidence. This skill retains React build decisions; `implementation` exclusively owns mutation safety and diff accounting. If it is unavailable, preserve the diagnosis and stop before mutation. Calls do not grant network, credential, publication or deployment authority. Do not call it if it is already active in the invocation chain; report the cycle.

## Verify and report

Run the narrow repository-native build, then inspect the actual artifact or packed consumer seam. Report tool/framework/React versions, compiler status, commands, artifact paths, entrypoints, dependency placement, security-patch evidence, warnings, source-map or client/server concerns, and anything not verified because network or credentials were unavailable.
