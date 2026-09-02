---
name: refactoring
description: Reshape existing code without intentionally changing its observable behavior by clarifying ownership, removing duplication and obsolete paths, reducing accidental complexity, and verifying consumers across the migration.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation"
---

# Refactoring

Improve the internal shape of existing code while preserving its observable
contract. Reduce the number of concepts, owners and paths a maintainer must
understand; do not optimize for line count or stylistic preference.

This skill owns structural diagnosis, target ownership, migration shape and
behavior-preservation strategy. It does not own feature behavior, bug diagnosis,
generic mutation safety or technology-specific refactoring mechanics.

## Establish the refactoring contract

Before proposing or changing structure, identify:

- the concrete maintenance, comprehension, duplication or change-safety problem;
- the code and consumers in scope;
- externally observable behavior and interfaces that must remain stable;
- compatibility, performance, ordering, error and resource-lifetime constraints;
- the intended structural improvement and how it can be observed;
- whether the request is analysis, a proposal, review or authorized mutation.

Do not use “cleaner,” “modern,” “too large” or “technical debt” as sufficient
justification. Connect the current shape to a real cost: inconsistent behavior,
multiple places to change, hidden coupling, difficult testing, unsafe extension
or recurring defects.

If the requested end state intentionally changes behavior or a public contract,
separate that change from the refactor and obtain the corresponding requirement.
Do not hide product decisions inside structural work.

## Understand before reshaping

Read applicable instructions, specifications, decisions, glossaries and nearby
conventions. Trace:

- callers, consumers and dependency direction;
- inputs, outputs, state changes and side effects;
- normal, boundary, error and cleanup paths;
- tests and other evidence that define behavior;
- compatibility and migration obligations;
- historical context when it explains an otherwise surprising constraint.

Do not preserve an awkward shape solely because it exists, but do not remove it
until its purpose is understood well enough to replace or retire safely. When
behavior is uncertain, characterize it through focused evidence before
refactoring.

## Diagnose the structural problem

Look for evidence of accidental complexity:

- one concept computed, named or enforced by several owners;
- one module owning unrelated concepts and dependencies;
- repeated conditionals or transformations encoding the same policy;
- wrappers, adapters or aliases that add no boundary or semantic value;
- temporary branches and compatibility paths with no remaining consumer;
- concepts split across files that must be reconstructed mentally;
- misleading names, dead paths or comments that narrate obsolete history;
- abstractions whose cost exceeds the variability they isolate.

These are investigation signals, not automatic violations. A large cohesive
module may be clearer than arbitrary fragments. Similar-looking code may
represent different contracts. An adapter at a real external boundary may be
the correct owner.

## Design the target shape

Name each concept and choose one natural owner for its invariant or computation.
Make consumers depend on that owner rather than re-deriving or synchronizing the
same fact elsewhere.

Use two complementary moves:

- **Consolidate** duplicate or parallel owners into one source of truth.
- **Decompose** an overloaded owner along coherent responsibilities, moving
  their dependencies and tests with them.

Prefer a direct model that the current requirements justify. Do not preserve
lineage in names such as `new`, `v2` or `legacy` once the historical distinction
no longer matters. Keep rationale in durable documentation when it remains
useful; code names should describe the present concept.

Compare plausible target shapes when the ownership boundary is genuinely
ambiguous. Choose based on cohesion, dependency direction, consumer simplicity,
change locality and compatibility—not novelty or the volume of code replaced.

## Plan the migration

Choose the smallest safe transition:

- replace directly when all consumers can move together and rollback is clear;
- migrate in slices when consumers, deployments or data cannot move atomically;
- retain a compatibility seam only for a real external or staged-migration
  boundary;
- give every temporary seam an owner, removal condition and verification path.

Keep behavior changes, broad renames and unrelated cleanup outside the refactor
unless they are required to complete the target ownership safely. A refactor
may span many files when one coherent concept moves; file count alone does not
define scope.

When removing a parameter, field, method or compatibility surface, first prove
that it carries no required behavior. Trace callers, overloads, interfaces,
implementations, reflection or serialization use, generated bindings and
binary compatibility obligations. Update consumers coherently and verify that
the narrower contract is intentional; an unused local name does not prove that
a public contract is removable.

## Apply safely

For authorized repository mutation, call `implementation` if it is available
and the invocation is acyclic. Pass the structural target, behavior-preservation
contract, scope and migration checkpoints; do not duplicate its dirty-tree,
authorization or final-diff workflow here.

If `implementation` is unavailable, report the configuration gap before
mutation. A read-only analysis or proposed target shape does not require the
call and does not authorize changes.

Move in reviewable increments that preserve a coherent state. Update consumers
to use the new owner, then remove stale paths when their consumers and migration
obligations are gone. Do not leave two permanent owners merely because keeping
both makes the intermediate diff smaller.

## Verify preservation and improvement

Verify through consumer-visible seams, not only the newly shaped module:

- run focused behavior and regression checks;
- exercise relevant errors, ordering, side effects and resource cleanup;
- compile or statically validate affected public boundaries;
- confirm every consumer uses the intended owner;
- search for stale names, duplicate computations, dead adapters and old paths;
- measure performance when the refactor can alter a critical cost.

Existing tests are evidence, not the whole contract. Do not weaken assertions
or rewrite expected behavior merely to make the new structure pass. Tests may
need structural updates when they were coupled to internals; preserve their
behavioral oracle and explain the change.

Judge the result by the target problem:

- fewer places own or change the concept;
- responsibilities and dependencies are easier to state;
- consumers need less coordination or special casing;
- obsolete paths are removed or have explicit retirement conditions;
- behavior evidence remains valid.

If the result only moves complexity, scatters one concept or replaces familiar
code with a fashionable abstraction, revise or abandon the refactor.

## Report

Summarize:

```text
Structural problem and evidence
Behavior-preservation contract
Previous and target ownership
Migration performed or proposed
Compatibility seams and removal conditions
Verification and outcomes
Residual risks and deferred behavior changes
```

## Composition boundaries

- `implementation` owns authorized repository mutation and final diff
  accountability; `refactoring` owns the structural target and migration logic.
- `testing` owns test strategy and oracle quality; `refactoring` identifies the
  behavior and consumer seams that must remain stable.
- `review` owns generic findings and verdicts; `refactoring` supplies the
  structural lens and remediation direction.
- `debugging` owns root-cause diagnosis for a failure; refactoring should not be
  used to conceal an unproven fix.
- `api-design` owns intentional public-contract evolution; refactoring preserves
  the current contract unless a separate decision changes it.
- Technology skills own language, runtime and tool-specific transformations.

Except for the conditional call to `implementation` for authorized mutation,
this skill does not automatically invoke another skill.
