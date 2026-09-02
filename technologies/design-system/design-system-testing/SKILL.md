---
name: design-system-testing
description: Design, implement, run, or diagnose tests for design-system tokens, components, variants, states, themes, public contracts, generated artifacts, and representative consumers.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED design-system-build"
---

# Design system testing

Create evidence that a design-system contract survives from source decisions to consumers. This skill owns system-specific test coverage and invariants. It does not own generic test strategy, accessibility conformance, visual approval or build mechanics.

## Establish the test surface

Identify authoritative token sources, generated artifacts, public packages, component states, themes, supported platforms, consumer fixtures and existing test tools. Prioritize stable contracts over broad snapshots.

Map each promised layer to its cheapest discriminating evidence: format/schema validation, resolver invariants, deterministic generation, package/import checks, component behavior, rendered state matrices and consumer integration. A single showcase or snapshot cannot cover this chain.

## Test layered contracts

- validate the declared token-format version, token/group shape, types, aliases or references, cycles, modes, extensions and required semantic roles;
- test resolution order, inheritance/extension and target conversions with small positive and negative fixtures, including missing, circular and type-incompatible references;
- compare generated artifacts with committed sources, detect nondeterminism and explain intentional semantic changes independently of formatting churn;
- exercise component variants, interactive and content states, themes and overrides;
- verify public and intentionally unsupported imports, types, styles, providers, server/client boundaries, peer dependency behavior and required runtime setup from a packed consumer fixture;
- test migrations or deprecated aliases when compatibility is promised;
- use focused structural assertions for exact contracts and visual evidence only for presentation behavior;
- keep fixtures representative of supported platforms rather than one internal showcase;
- use generated case tables or property-based tests for large invariant spaces such as alias graphs and theme fallbacks only when failures remain reproducible and readable;
- ensure examples and stories use public APIs, because an internal fixture can accidentally prove a path consumers cannot import.

A snapshot update is not evidence that the change is correct. Explain the intended decision before accepting broad output changes, and split semantic fixtures from formatting snapshots so reviewers can see which contract changed.

## Delegate edits

For authorized tests or fixtures, call `implementation` with the invariant, failing evidence, consumer scope and exact writable surface; this skill retains system-test semantics and `implementation` owns safe mutation. When the defect or test seam lies in generators, package exports or build scripts, call `design-system-build` with the source-to-artifact trace; it exclusively owns build mechanics. If a target is unavailable, preserve valid findings and report the missing portion. Calls grant no install, publication or shared-environment authority. Do not call an active target or ancestor; report the cycle.

## Verify and report

Run the smallest invariant or consumer fixture, then affected matrices and the packed-artifact consumer when publication surfaces changed. Report layers exercised, format/tool versions, variants/themes/platforms covered, commands, artifact diffs, ordering or randomness controls, false-positive controls and unsupported consumers.
