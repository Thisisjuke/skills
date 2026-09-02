---
name: feature-implementation
description: Deliver a complete feature across affected flows, states, consumers, integration surfaces, and acceptance evidence. Always call implementation for the shared mutation contract.
license: MIT
metadata:
  skill-calls: "ALWAYS CALLS implementation"
---

# Feature implementation

**ALWAYS CALLS:** `implementation`.

Call `implementation` before repository mutation and retain its scope,
authorization, preservation, verification and diff-accounting contract
throughout the task. If it is unavailable or already active in the invocation
chain, report the configuration error and do not duplicate or improvise the
base workflow.

Deliver one feature as a complete product behavior, not merely a code patch.
Use this skill when the unit of work is a feature spanning multiple states,
consumers, components or layers. Use `spec-implementation` instead when an
existing durable specification is the execution contract.

## Confirm the feature contract

Establish:

- who receives the feature and what they can do afterward;
- the trigger, primary flow and observable outcome;
- acceptance evidence and important non-goals;
- compatibility, rollout or data constraints explicitly in scope;
- the repository areas and external contracts likely to be affected.

If product outcome or acceptance behavior is materially ambiguous, resolve the
decision before encoding it. A formal spec is unnecessary when the feature
contract is already clear.

## Map feature completeness

Trace the current behavior and build a compact impact map from the relevant
dimensions:

```text
Entry points and consumers
Domain rules and state transitions
Interfaces, data and persistence
Primary and alternate flows
Empty, loading, error and recovery states
Authorization and ownership boundaries
Observability and operational behavior
Tests, documentation and compatibility surfaces
```

Not every feature touches every dimension. Mark inspected areas as affected or
irrelevant so the obvious happy path does not masquerade as completeness.

## Deliver meaningful slices

Choose increments that answer a feature question or deliver an observable
vertical capability. Prefer a thin primary flow, a risk-first experiment, a
state-transition slice or an integration slice according to the dominant risk.
Avoid horizontal batches that create layers without testable behavior.

Record which feature dimensions each slice completes. Temporary scaffolding,
flags and compatibility paths need a real exposure requirement and an explicit
owner or removal condition.

Implement each slice through the shared `implementation` contract. When code
evidence contradicts the feature contract, resolve the product decision rather
than redefining acceptance around the easiest implementation.

## Verify integrated behavior

Per-slice evidence is necessary but not sufficient. Before completion, revisit
the impact map and verify the integrated primary, alternate, error, recovery,
permission and upgrade paths that apply. Confirm that all affected consumers
and states tell the same story and that later slices did not invalidate earlier
evidence.

When selected, `testing`, `performance` and technology skills own their
specialized evidence. This skill owns deciding whether the combined evidence
proves feature completeness.

## Report feature completion

Return:

```text
Feature outcome and acceptance evidence
Completed flows, states and consumers
Important feature choices
Affected surfaces
Integrated validation
Irrelevant, unverified or deferred dimensions
Residual risks
```

The feature is complete when its confirmed outcome and affected states are
coherent, not when the first slice or happy path turns green.

## Ownership boundary

`feature-implementation` owns end-to-end feature completeness. It does not own
the shared mutation method, specification execution, planning, test design,
review reporting, documentation lifecycle or technology mechanics.
