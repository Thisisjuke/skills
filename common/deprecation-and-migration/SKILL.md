---
name: deprecation-and-migration
description: Decide, plan, execute, or verify the safe retirement of an API, feature, data shape, dependency, or system while moving consumers to an accepted replacement.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED risk-management; CALLS WHEN NEEDED documentation; CALLS WHEN NEEDED implementation"
---

# Deprecation and migration

Retire an obsolete capability without stranding consumers, corrupting data or
leaving two permanent sources of truth. Treat deprecation as a measured
lifecycle with an owner, replacement, migration path and removal evidence.

This skill owns the retirement decision, consumer inventory, transition model,
compatibility window, cutover and removal criteria. It does not own product
priority, generic risk analysis, maintained documentation, repository mutation
or deployment execution.

## Establish the retirement contract

Identify:

- the old capability and observable behavior consumers may depend on;
- why maintaining it is no longer justified;
- known consumers, owners and discovery evidence for unknown consumers;
- the accepted replacement and behavior it intentionally does not preserve;
- data, protocol, deployment and compatibility constraints;
- whether retirement is advisory or has an authorized deadline;
- authority required for notices, migrations, removal and communication.

Do not announce removal before a viable replacement or an explicit decision to
end the capability without one. Treat undocumented behavior as a possible
dependency until usage evidence disproves it.

## Decide with evidence

Compare ongoing cost and risk with consumer migration cost, replacement
maturity and the consequence of forced change. Quantify usage through dependency
analysis, telemetry, configuration, support evidence and owner confirmation.

When uncertainty or blast radius needs systematic analysis, call
`risk-management` before selecting the transition. Pass consumers, outcomes,
failure scenarios, reversibility and evidence. It owns risk analysis and
response options; this skill retains the retirement decision. If unavailable,
report the unresolved risk surface and do not present a high-impact cutover as
ready.

Before any call, check the active invocation chain and stop delegation if the
target is already active or an ancestor. A call never adds mutation,
communication or production authority.

## Design the transition

Choose the smallest transition that keeps old and new consumers valid:

- direct replacement when consumers can move atomically and rollback is sound;
- an adapter when an old contract must temporarily front a new owner;
- parallel routing or a feature flag for progressive exposure and comparison;
- expand, migrate, contract for data or schemas that cannot change atomically.

For expand/migrate/contract, add the new shape first, maintain compatibility
during mixed-version operation, backfill in bounded observable batches, switch
consumers, prove the old shape is unused, and remove it in a later destructive
step. A rollback claim must account for data written after cutover.

Every temporary compatibility path needs an owner, removal condition, usage
signal and verification path. Avoid a permanent dual-write or adapter whose
retirement is nobody's responsibility.

## Communicate and support consumers

Define the audience, reason, replacement, migration steps, compatibility
differences, support path, timeline and completion evidence. An advisory notice
must not imply a deadline; a compulsory migration requires an authorized
deadline and proportionate tooling or support.

When durable notices, migration guides or decision records must change, call
`documentation`. Pass the accepted contract, audience, evidence and writing
authority. `documentation` owns canonical location, format and lifecycle; this
skill owns technical accuracy and removal criteria. If unavailable, return a
documentation brief and do not claim communication complete.

## Execute incrementally

For authorized repository mutation, call `implementation` before changing
code, configuration or migrations. Pass the transition slice, preserved
contracts, consumer scope, rollback boundary and verification requirements.
`implementation` owns mutation and final diff accountability; this skill owns
sequencing and retirement evidence. If unavailable, stop before mutation.

Move consumers in coherent slices. After each slice, verify replacement
behavior, required fallback, telemetry and rollback assumptions. Pause when a
hidden dependency, contract mismatch or data hazard invalidates the transition.

## Prove removal

Remove the old capability only when:

- every in-scope consumer is migrated or explicitly retired;
- usage evidence covers an agreed observation window;
- mixed-version and rollback obligations have ended;
- old code, configuration, data paths, tests and docs have dispositions;
- the destructive action and its recovery boundary are authorized.

After removal, search for stale references and verify the replacement through
consumer-visible behavior. Report what was measured, what could not be observed
and residual obligations. Zero known callers is not proof of zero callers when
discovery was incomplete.

## Report

Return the decision rationale, consumer evidence, compatibility contract,
migration stages, owners, notices, rollback boundary, verification by slice,
removal evidence and residual obligations.
