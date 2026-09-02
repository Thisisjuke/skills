---
name: planning
description: Turn a sufficiently defined objective or specification into an ordered, dependency-aware, risk-conscious execution plan with verifiable tasks and checkpoints.
license: MIT
---

# Planning

Turn a defined contract into a route for execution. Decompose work by outcomes,
map dependencies and uncertainty, choose an order, and make every planned unit
verifiable without rewriting the product or system requirements.

This skill produces a plan. It does not define missing behavior, implement work,
manage an ongoing project or assign work to agents automatically.

## Validate the planning input

Start from an existing specification, accepted brief or objective whose outcome,
scope and acceptance are sufficiently clear. Read applicable instructions,
current repository structure, relevant contracts, decisions and validation
commands.

If different reasonable interpretations would change the plan materially,
report the missing contract instead of choosing one silently. Use
`specification` separately when durable behavior must be defined; planning does
not auto-invoke it or write the missing spec inside the plan.

Separate facts observed in the repository from assumptions and open questions.
Do not require a formal spec for a small but nontrivial objective that is already
clear enough to sequence.

## Model the work

Identify the deliverable surfaces and the changes needed to reach acceptance.
Represent dependencies by what one unit must provide before another can begin:

```text
Work unit
Outcome it unlocks
Inputs or dependencies
Affected contract or surface
Acceptance reference
Verification seam
Risk or unresolved assumption
```

Dependencies should be causal, not merely the order in which files appear in
the repository. Detect cycles, shared bottlenecks and contracts that must be
stabilized before parallel work.

Distinguish:

- **dependency:** another result is required first;
- **coordination:** work may proceed but shares a contract or files;
- **checkpoint:** evidence must be reviewed before greater cost is incurred;
- **milestone:** several outcomes combine into a meaningful delivery state.

## Choose useful slices

Prefer work units that produce a coherent, testable outcome. Use vertical slices
when they expose integrated behavior early, risk-first probes when feasibility
is uncertain, and contract-first tasks when independent consumers need a stable
boundary.

Do not size tasks by universal line, file or duration limits. Size them by
coupling, unresolved decisions, verification cost and the ability to hand the
work over without rediscovering its purpose. Split a task when it has multiple
independent outcomes or cannot be accepted with one coherent evidence set.

Keep planning decisions distinct from implementation decisions. Name a likely
surface or seam when useful, but avoid prescribing internal code structure that
repository evidence during implementation may legitimately improve.

## Order by dependency and learning

Build a directed, acyclic execution graph when the work has real dependencies.
Select the next frontier from units whose prerequisites are satisfied.

Within that frontier, prioritize according to the objective:

- resolve high-impact uncertainty before dependent investment;
- establish shared contracts before parallel consumers;
- deliver an early useful path when it improves feedback;
- postpone reversible polish that does not unblock learning;
- keep destructive or hard-to-reverse operations behind explicit checkpoints.

Identify parallel opportunities without assuming agents, branches or worktrees
exist. Note file or contract contention that would make apparent parallelism
unsafe.

## Define executable tasks

Each task should state:

```text
Outcome
Scope and non-scope
Dependencies
Acceptance criteria or spec references
Verification
Likely surfaces, when useful
Open decisions or stop conditions
```

Reference requirements instead of copying them into every task. A task is ready
when an implementer can begin without inventing scope or behavior, not when
every internal implementation choice has been predetermined.

Add checkpoints where evidence can invalidate later work, where multiple paths
integrate, or before an irreversible action. Do not insert periodic gates by a
fixed task count.

Provide estimates only when requested. State assumptions, range and uncertainty;
do not infer elapsed time from file counts or imply precision unsupported by
historical evidence.

## Choose the plan artifact

Follow the repository's existing planning and task-tracking convention. If none
exists, return a self-contained Markdown plan or write one only when file output
is authorized. Do not automatically create `tasks/`, tracker tickets, branches
or project-management records.

A useful plan contains:

```text
Objective and source contract
Assumptions and non-goals
Dependency graph or ordered work units
Milestones and checkpoints where justified
Risks and learning strategy
Verification map
Open decisions
```

Keep one owner for each fact: link to the specification for behavior, an ADR for
rationale and project-management artifacts for ongoing status. The plan owns
execution structure, not copies of those records.

## Review plan quality

Before handoff, verify:

- every task advances an accepted outcome;
- dependency direction and frontier are coherent;
- acceptance and verification are present and distinguishable;
- risky unknowns are surfaced early enough;
- parallel work has stable boundaries;
- no task hides a product decision or unrelated scope;
- completion of all tasks would satisfy the source contract;
- the plan can be revised when evidence invalidates an assumption.

Report the plan, its critical path, first dependency-ready work, major risks and
any missing decision that blocks execution.

## Composition boundaries

- `planning` owns decomposition, dependencies, sequencing, milestones and
  execution checkpoints.
- `specification` owns behavior, scope, constraints and acceptance.
- `project-management` owns ongoing tracking, capacity, budget, stakeholders
  and governance.
- `spec-implementation` owns progress and traceability while executing a spec.
- Technology skills contribute platform constraints without owning the plan.

Planning remains standalone and never auto-invokes specification or
implementation.
