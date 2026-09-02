---
name: specification
description: Define a durable, implementation-neutral contract for a product or system change through scope, behaviors, rules, constraints, non-goals, acceptance evidence, and unresolved decisions.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED domain-modeling"
  uses-glossary: "true"
---

# Specification

Turn a sufficiently understood intent into a durable contract describing what
must be true. Make behavior, boundaries and acceptance independently reviewable
before planning or implementation commits to a particular route.

This skill writes or revises a specification. It does not create an execution
plan, task graph or implementation.

## Establish the specification boundary

Identify:

- the problem or opportunity and intended beneficiary;
- the observable outcome and why it matters;
- the authority and evidence behind the request;
- the product, system or domain boundary covered;
- the decisions already confirmed and those still open;
- the expected lifetime and repository convention for the artifact.

Read relevant repository instructions, current behavior, adjacent contracts,
existing specs and applicable root-to-nearest `GLOSSARY.md` files. Do not ask
the user for facts the repository answers cheaply.

If intent is too ambiguous to define stable behavior, stop and request the
missing product decision or use a separately selected discovery skill. Do not
hide uncertainty behind implementation detail.

When the request is to turn an already completed conversation, brief or issue
into a specification, synthesize its confirmed decisions before asking new
questions. Preserve dissent, assumptions and unresolved choices as such; do not
repeat discovery merely to populate a preferred template. Ask only for a gap
that would permit materially different behavior or acceptance.

## Choose proportionate depth

Use the smallest specification that can prevent materially different
interpretations. A narrow change may need a concise contract; a multi-surface
feature may need states, scenarios and interface constraints. Do not require a
fixed template, file count or ceremony based on task size alone.

Split a specification when it contains independently valuable capabilities with
different consumers, lifecycles or acceptance. Keep one specification when the
parts only make sense as one behavior. Record relationships between separate
specs rather than creating a monolithic initiative document.

## Specify behavior, not build order

Define applicable elements:

- actors, triggers and preconditions;
- primary, alternate, error and recovery behavior;
- state transitions and invariants;
- inputs, outputs and externally observable effects;
- ownership, authorization and trust boundaries;
- compatibility, data, performance or accessibility constraints;
- non-goals and explicitly unchanged behavior;
- acceptance evidence and unresolved decisions.

Use examples to clarify rules, not replace them. Describe implementation detail
only when it is itself a confirmed constraint or externally owned contract.
Avoid file paths, task order, estimates and internal class designs likely to
become stale without changing required behavior.

When API or interface design is substantial, a separately selected `api-design`
skill owns that contract's detailed shape. When performance or accessibility is
selected, those skills own the quality and evidence of their constraints; the
spec records the agreed requirement and points to its source.

## Make acceptance discriminating

Each acceptance statement should distinguish a conforming result from a
non-conforming one without prescribing unnecessary implementation. Cover the
highest-risk boundaries and negative cases, not only the happy path.

For each requirement, make clear:

```text
Condition or trigger
Required observable behavior
Important invariant or prohibited result
Evidence that can demonstrate acceptance
```

Do not invent numerical targets. Use confirmed product requirements, standards,
budgets or baselines. If no decision exists, keep the target open and identify
who or what can resolve it.

## Control terminology and decisions

Use canonical project language. When specification work establishes a new
canonical term, exposes conflicting meanings or resolves a durable ambiguity,
call `domain-modeling` if it is available and the call is acyclic. Obtain
explicit human approval before that called skill changes a glossary.

If it is unavailable, preserve the term and definition in the specification
and report the configuration gap. Do not auto-run repository setup.

Separate requirements from rationale and from undecided questions. Link an
existing ADR when it governs the spec. A possible new ADR is handled through
the established `domain-modeling → documentation` workflow rather than being
invented inside this skill.

## Reconcile and review the artifact

Follow an existing specification location and format when present. Otherwise
use a concise structure such as:

```text
Objective and beneficiaries
Scope and non-goals
Terms and related decisions
Behavior and invariants
Scenarios and states
Constraints and external contracts
Acceptance evidence
Open decisions
```

Check that every requirement supports the stated outcome, no acceptance item
introduces new hidden scope, terms are consistent, and implementation choices
have not replaced unresolved product decisions.

Ask for confirmation of material behavior and scope when the workflow is
interactive. Record approved corrections in the spec. Do not mark unresolved
decisions as accepted merely to make the artifact look complete.

## Report readiness

Return the artifact location or complete specification, then summarize:

```text
Outcome and scope defined
Important behaviors and constraints
Acceptance evidence
Confirmed non-goals
Open decisions and blockers
Terminology delegated to domain-modeling
Readiness for planning
```

A spec is ready for planning when implementation order can be chosen without
inventing product behavior. It may still contain explicitly delegated technical
choices that do not alter its contract.

## Composition boundaries

- `specification` owns required behavior, scope, constraints and acceptance.
- `planning` owns decomposition, dependencies, sequencing and checkpoints.
- `spec-implementation` owns requirement traceability during execution.
- `interview-me` owns intent clarification before the contract is stable.
- `documentation` owns general durable documentation and ADR lifecycle.

Specification never calls planning or implementation automatically.
