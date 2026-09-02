---
name: design-system-build
description: Build and package design-system tokens, styles, components, icons, assets, and multi-platform artifacts while preserving source ownership, transforms, public APIs, versioning, and consumer contracts.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation"
---

# Design system build

Produce consumable design-system artifacts from their declared sources. This skill owns token and asset transformation graphs, package surfaces and consumer build compatibility. It does not own generic mutation safety, component behavior, testing strategy or release approval.

## Discover the build graph

Inspect manifests, lockfiles, token sources, generators, style/component/icon entrypoints, workspace boundaries, package exports, CI, generated-file policy and release scripts. Map source decision to every generated platform artifact and identify which files are authored versus regenerated.

Determine the declared token format and version, parser/resolver capabilities, naming rules, platform targets and extension policy before editing data. If the repository targets DTCG 2025.10, validate its token and group model—including `$value`, inherited or explicit `$type`, `$root` tokens and group extension/reference forms—against the version-matched [format](https://www.w3.org/community/reports/design-tokens/CG-FINAL-format-20251028/) and [resolver](https://www.w3.org/community/reports/design-tokens/CG-FINAL-resolver-20251028/) reports. Do not emit newer syntax merely because another tool accepts it, and do not flatten away distinctions that a round trip must preserve.

## Preserve artifact contracts

- Keep token type, alias, mode, unit and fallback meaning through transforms.
- Make parse, validation, extension, reference resolution, transformation and emission order explicit and deterministic. Fail with a useful source path on unresolved, circular or type-incompatible references.
- Treat group structure and token semantics separately. Do not infer a token type or product meaning from grouping unless the selected format and local contract explicitly define it.
- Preserve color spaces, alpha, dimensions and composite values until a target transformation deliberately converts them. Record lossy conversions and reject unsupported target values instead of silently coercing them.
- Preserve CSS, module, type, asset and framework entrypoints consumed downstream.
- Keep framework runtimes and styling engines in the correct dependency class for library consumers; prefer peer dependencies for a host-owned singleton runtime and direct dependencies only for code the package truly ships and owns.
- Avoid bundling duplicate UI runtimes, fonts, icons or global styles unexpectedly.
- Produce reproducible outputs without timestamps, host paths or network-only mutable inputs when practical.
- Edit generator inputs rather than generated output unless generated files are intentionally maintained.
- Define package exports for every supported import surface and reject accidental deep imports. Verify ESM/CommonJS, types, server/client boundaries, tree-shaking and side-effect declarations only for formats the package promises.
- Generate an artifact inventory or manifest when multiple sources and platforms make provenance otherwise ambiguous. It should identify source/version and output ownership without embedding machine-specific paths.

Treat package publication, registry credentials and release versioning as separately authorized operations.

Treat parsers, icon optimizers, documentation plugins and code generators as supply-chain inputs. Pin or lock them according to repository policy, inspect executable configuration, validate untrusted SVG or rich assets, and never pass secrets into a client artifact or generated stylesheet.

## Change safely

When sources, transforms, manifests, scripts or generated artifacts must change, call `implementation` with the build graph, authoritative inputs, target artifacts, affected consumers and reproduction command. This skill retains design-system build semantics; `implementation` exclusively owns mutation safety and diff accounting. If unavailable, preserve the build diagnosis and stop before mutation. The call grants no install, network, credential, publication or deployment permission. Do not call an active target or ancestor; report the cycle.

## Verify and report

Run generation twice from a clean temporary output when determinism is material, inspect semantic as well as textual diffs, package the result and exercise representative consumers or import surfaces from the packed artifact. Report sources, format/tool versions, resolution and transform policy, artifact inventory, dependency exposure, consumer checks, lossy conversions and unverified platforms.
