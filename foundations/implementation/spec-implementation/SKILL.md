---
name: spec-implementation
description: Execute an existing specification with requirement traceability, controlled deviations, dependency-aware progress, and closure evidence. Always call implementation for the shared mutation contract.
license: MIT
metadata:
  skill-calls: "ALWAYS CALLS implementation"
---

# Spec implementation

**ALWAYS CALLS:** `implementation`.

Call `implementation` before repository mutation and retain its scope,
authorization, preservation, verification and diff-accounting contract
throughout the task. If it is unavailable or already active in the invocation
chain, report the configuration error and do not duplicate or improvise the
base workflow.

Implement an existing specification while keeping requirements, decisions,
code and evidence synchronized. Use this skill only when a durable specification
or equivalent requirement set already exists.

## Establish the source of truth

Locate the spec, its status, linked plans and decisions, applicable instructions
and acceptance artifacts. Identify its objective, non-goals, requirements,
dependencies, open decisions, required gates and completion rules.

If no adequate spec exists, report the missing precondition. Do not invent one
inside this workflow; use `feature-implementation` for a clear feature brief or
a separately selected specification capability.

## Reconcile spec and repository

Inspect current code and prior progress. Classify each requirement:

```text
not started | partially implemented | implemented and evidenced |
implemented without evidence | contradicted | obsolete by approved decision
```

Maintain a compact traceability ledger linking each requirement to its owning
surface, dependency and expected proof. Reuse the project's existing status
convention rather than creating a parallel one.

When repository evidence conflicts with the spec, distinguish:

- an implementation detail may change while preserving the requirement;
- the requirement or acceptance behavior must change;
- a blocker or missing decision prevents honest progress.

Proceed independently only in the first case. Obtain appropriate authority for
a material deviation and update the spec only when that mutation is authorized.
Never let code and spec drift silently.

## Execute the dependency frontier

Choose requirements whose dependencies are satisfied. Independent work may run
in parallel when the configured workflow supports it, but this skill does not
create agents, branches or worktrees. Serialize shared interfaces and unresolved
contracts.

For each pass:

1. name the requirement being advanced;
2. execute its coherent change through `implementation`;
3. verify its specified acceptance behavior and relevant neighbors;
4. update traceability and record justified deviations;
5. continue while authorized requirements remain dependency-ready.

Code existence is not completion. Each requirement needs its declared evidence
or an approved reason that the evidence was superseded.

## Control deviation and closure

Keep implementation choices, spec deviations and new scope distinct. Record
choices that affect future maintenance, require appropriate authority for
material deviations and do not absorb adjacent scope.

Before closure, reconcile every requirement, decision and deferred item against
final code. Re-run cumulative gates, verify contracts shared across passes and
check that later work did not invalidate earlier evidence. A closed slice list
does not override an unreconciled global requirement.

If work must stop, leave a durable pickup point using the project's convention:

```text
Last completed requirement and evidence
Current traceability state
Next dependency-ready requirement
Open blocker or decision
Changed assumptions or gates
```

## Report spec closure

Return:

```text
Specification and implemented objective
Requirement traceability summary
Approved deviations and implementation choices
Cumulative validation evidence
Unresolved, deferred or obsolete requirements
Residual risks
```

## Ownership boundary

`spec-implementation` owns traceable execution and closure of an existing
specification. It does not own the shared mutation method, spec writing, feature
discovery, generic planning, test design, review reporting, orchestration,
publishing or technology mechanics.
