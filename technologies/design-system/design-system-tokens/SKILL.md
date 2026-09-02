---
name: design-system-tokens
description: Model, migrate, validate, or diagnose design-system tokens and themes; use for semantic taxonomies, DTCG formats, aliases, group extension, resolver sets and modifiers, modes, cross-platform values, or token compatibility.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED design-system-build"
---

# Design system tokens

Turn governed design decisions into a versioned token contract that tools and consumers resolve consistently. This skill owns token taxonomy, source semantics, DTCG interpretation, aliases, themes and compatibility decisions. It does not own product visual direction, generator implementation, package exports, component APIs or release authorization.

## Establish the token authority

Locate the authored token sources, generated artifacts, resolver or theme inputs, schemas, tool versions, platform targets, consumer imports and migration policy. Identify which file owns a decision and which files only encode transformed output. Surface conflicting sources instead of choosing the easiest format to edit.

Record the declared interchange format and version. The [DTCG Format Module 2025.10](https://www.w3.org/community/reports/design-tokens/CG-FINAL-format-20251028/) and [Resolver Module 2025.10](https://www.w3.org/community/reports/design-tokens/CG-FINAL-resolver-20251028/) are stable Community Group Final Reports, not generic promises that every token tool implements them. Require documented support and representative round-trip evidence before using a feature.

## Model durable decisions

Use the smallest taxonomy that preserves intent across value changes:

- primitive tokens express reusable raw scales or source values;
- semantic tokens express contextual roles such as text, surface, border, action or feedback;
- component tokens exist only when a stable component-specific decision cannot be represented clearly by the semantic layer;
- consumer overrides remain explicit and do not silently become global system decisions.

Name by durable purpose rather than current appearance. Keep state, emphasis, theme and platform dimensions consistent, and avoid names that encode one component implementation. Do not create a token for every isolated value or add aliases that carry no semantic or compatibility value.

Define type, unit, color space, alpha, composite shape and fallback behavior. A token value that happens to render correctly on one platform is not evidence that its meaning survives another target.

## Apply the selected format exactly

For DTCG 2025.10 inputs:

- an object with a direct `$value` property is a token; a group has no direct `$value` but may contain a token under the reserved `$root` name;
- every token has a determinable `$type`, either explicit, inherited from its nearest typed group or obtained from a referenced token; never guess type from the value shape;
- groups organize data and may provide supported properties such as `$type`, `$description`, `$root` or `$extends`, but grouping alone does not define product purpose;
- apply the version's curly-brace and JSON Pointer reference rules only in the positions where that format permits them;
- resolve chained aliases, group extension and local overrides in the specified order;
- reject missing, circular and type-incompatible references with the originating token path.

Preserve unknown `$extensions` data and use collision-resistant vendor or project keys for owned extensions. Reject ambiguous unowned syntax. Do not silently normalize a legacy or tool-specific format into DTCG and then claim lossless conformance.

## Resolve themes without combinatorial drift

When the project uses a resolver document, distinguish source token sets from modifiers and their contexts. Make `resolutionOrder` and conflict precedence explicit. Keep modifiers as orthogonal as practical so theme, brand, density, platform and accessibility contexts do not override the same decisions accidentally.

Enumerate supported permutations and designate defaults deliberately. Verify light, dark, high-contrast or brand modes as semantic contexts, not mechanical value swaps. A generated theme is invalid when required roles disappear, aliases change type or a later modifier wins only because incidental array order was misunderstood.

If the project does not adopt the DTCG Resolver Module, document its actual mode and precedence model rather than imitating resolver terminology incompletely.

## Preserve targets and evolution

Keep modern color spaces, dimensions and composite values intact until a named platform transform requires conversion. Record gamut mapping, unit conversion, rounding, unsupported features and other lossy operations. A fallback must preserve intended contrast and hierarchy, not merely parse.

For renames or semantic changes, inventory consumers, distinguish source compatibility from generated-name compatibility, provide temporary aliases only when they have an owner and removal condition, and verify the replacement in representative themes and platforms. Do not repurpose an existing public token name for a different meaning.

## Verify the token contract

Validate positive and negative fixtures for token/group shape, types, inheritance, references, cycles, extension, resolver order and required semantic roles. Compare resolved values across representative permutations and trace at least one decision through a platform output and real consumer. Separate semantic changes from formatting or ordering churn.

Report source and format versions, supported contexts, resolution order, affected consumers, lossy conversions, compatibility window and any target for which conformance or rendering was inferred rather than executed.

## Delegate bounded changes

When authorized authored token or resolver inputs must change, call `implementation` after fixing the semantic decision and compatibility boundary. Pass the authoritative sources, affected names and contexts, consumer impact and verification cases; this skill retains token meaning while `implementation` exclusively owns mutation safety and diff accounting. When parsers, transforms, generator configuration or emitted artifacts must change, call `design-system-build` with the source format, resolution trace, target requirements and expected outputs; it exclusively owns build mechanics and may delegate its mutation. If a target is unavailable, preserve the token diagnosis and stop only the affected mutation or generation portion. Calls grant no package installation, credential, publication or release authority. Do not call a target already active in the invocation chain; report the cycle.

## Composition boundaries

- `design-system` owns the wider component, pattern and governance contract.
- `design-system-testing` owns executable cross-layer coverage.
- `design-system-accessibility` owns reusable accessibility requirements across themes and states.
- Product-design skills own the intended visual and experiential decision.
