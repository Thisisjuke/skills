---
name: project-management
description: Establish or control a project across software, product, design, operational, or other domains by integrating outcomes, scope, governance, milestones, capacity and cost, risks, stakeholders, decisions, changes, and forecasts.
license: MIT
---

# Project management

Keep a project decision-ready by maintaining one coherent view of the outcome,
constraints, commitments, uncertainty and forecast. Integrate the management
dimensions that trade off against each other; do not replace them with a task
list or a methodology ceremony.

This skill owns the integrated project baseline, governance, milestone view,
capacity and cost envelope, material risk and issue position, stakeholder
alignment, change control and progress forecast. It does not own specialist
risk analysis, roadmap design, detailed work decomposition, product
requirements, technical execution, delivery mechanics, retrospective method or
a particular framework and tracker.

Project analysis is read-only by default. Update local project artifacts only
when requested or clearly part of an authorized management task. Changing an
external tracker, budget, commitment, assignment or stakeholder communication
requires explicit authority for that external or organizational action.

## Establish the project contract

Determine:

- the intended outcome, beneficiary and reason the project exists;
- measurable success and the evidence that will demonstrate it;
- sponsor or accountable owner, project lead and decision rights;
- scope boundary, exclusions and acceptance authority;
- target dates, fixed commitments and negotiable constraints;
- available people, skills, funds, facilities and other capacity;
- lifecycle, delivery approach and governance required by the organization;
- current phase, source artifacts and requested management decision.

Tailor the control effort to project size, uncertainty, reversibility and
consequence. A short internal initiative does not need portfolio ceremony; a
multi-team, regulated or irreversible change needs stronger ownership,
traceability and assurance.

If the objective or required behavior is unresolved, surface that gap and use
the appropriate discovery or specification capability. Do not manufacture a
stable baseline from an unconfirmed idea.

## Build an integrated baseline

Create the minimum accepted reference against which change and progress can be
judged. Connect:

- outcomes and scope boundaries;
- deliverables or capability increments;
- milestones, decision gates and acceptance points;
- external and cross-team dependencies;
- capacity assumptions and cost envelope;
- quality, compliance and operational constraints;
- major risks, assumptions and opportunities;
- ownership and reporting cadence.

A milestone represents an outcome, decision or externally meaningful state,
not merely a date. Define its acceptance evidence and owner. Keep detailed
tasks in the selected `planning` artifact; the project baseline should expose
only the resolution needed to understand commitment, dependency and forecast.

Do not present an estimate as a fact. Record its basis, range or confidence,
included work, excluded work and assumptions. Keep effort, elapsed time,
capacity and cost distinct. More people do not automatically shorten work with
coordination, scarce skills or sequential dependencies.

## Define governance and stakeholder alignment

Identify people or groups who fund, decide, deliver, operate, use, regulate or
are materially affected by the project. For each relevant stakeholder, capture
only what changes management decisions:

- interest, impact and required outcome;
- decision or approval authority;
- information needed, timing and responsible communicator;
- commitments, concerns and unresolved conflicts;
- escalation path when agreement or action is late.

Make decision rights explicit. Distinguish who recommends, who contributes
evidence, who decides and who must be informed. Avoid consensus theater: some
decisions need consultation, not unanimous approval. Conversely, silence is not
approval when an accountable authority is required.

Use an existing governance model and communication cadence when suitable. Do
not impose Scrum roles, stage gates, weekly status meetings or another method
without a project reason.

## Manage uncertainty as distinct records

Keep these concepts separate:

- **Risk:** an uncertain event or condition that could help or harm objectives.
- **Issue:** a condition that is already affecting the project.
- **Assumption:** an unverified belief used to plan or decide.
- **Dependency:** an external or internal condition another outcome relies on.
- **Decision:** an accepted choice that constrains subsequent work.

For each material item, record owner, evidence, affected outcome, impact,
urgency, response or next check, trigger and review date. Add probability or a
quantitative exposure only when the available evidence makes it meaningful.

Risks include opportunities as well as threats. Choose a response that changes
likelihood, impact, exposure or preparedness; “monitor” without a trigger,
owner or next review is not a response. Escalate when the response exceeds the
project's authority, capacity or risk tolerance.

Resolve stale assumptions deliberately. Once evidence confirms or disproves
one, update the baseline, risk, issue or decision it affected rather than
leaving contradictory records.

## Track progress and forecast

At each useful control point, compare current evidence with the accepted
baseline:

- outcomes and milestones accepted, not just activity completed;
- forecast dates and confidence, including dependency movement;
- capacity consumed and remaining by relevant skill or resource;
- actual and forecast cost against the authorized envelope;
- scope accepted, added, removed or awaiting a decision;
- risks, issues, assumptions and decisions that changed;
- quality or acceptance evidence and unresolved exceptions;
- stakeholder commitments and approvals due.

Report trend and forecast, not only a color or percent complete. Define any
status label with objective criteria. Separate evidence from interpretation and
state the date or data basis of the snapshot.

Progress is not proportional to effort spent. Prefer accepted outcomes,
retired uncertainty and cleared dependencies over task counts. When data is
incomplete, state what cannot be forecast rather than filling the gap with
false precision.

When an external tracker is the maintained source of status, first resolve its
provider, project identity, state and label vocabulary, item identifiers and
available read/write operations. Keep that provider mapping as an adapter to
this management model rather than embedding GitHub, GitLab or local-Markdown
commands in the skill. Reading tracker state does not authorize labels,
comments, assignments, closure or other writes; preview mutations and obtain
the authority required by the selected system.

## Control change

When scope, timing, capacity, cost, quality or constraints change:

1. Describe the requested change and its reason.
2. Assess impact across outcome, scope, schedule, capacity, cost, risk,
   dependencies, operations and affected stakeholders.
3. Present viable options, including deferral, substitution or stopping.
4. Identify the authority required and obtain the decision.
5. Update the accepted baseline and linked records once approved.
6. Communicate the decision to affected owners and verify consequential
   commitments changed with it.

Change control is not a ban on change. Its purpose is to prevent one dimension
from moving invisibly while reports still assume the old commitment. Preserve
the previous baseline or decision history through the project's existing
artifact convention; do not create competing copies.

## Handle phase boundaries and closure

Before a phase transition or project closure, verify:

- acceptance authority has evaluated the intended outcomes;
- operational, support or business ownership is explicit;
- unresolved work, risks and obligations have an owner and destination;
- commitments, contracts, access, data and temporary controls are closed or
  transferred;
- actual cost, schedule and outcome evidence is captured proportionately;
- durable decisions and documentation are updated where needed;
- learning that merits a retrospective is routed to that separate capability.

Do not call a project complete merely because implementation stopped or a date
arrived. Conversely, do not keep it open for unrelated improvements outside its
accepted outcome.

## Report a project control snapshot

Use an established project format when one exists. Otherwise return:

```text
Outcome, scope and current phase
Accountability and decisions required
Milestones, acceptance evidence and forecast
Capacity and cost position
Material risks, issues, assumptions and dependencies
Stakeholder commitments and communications due
Approved or proposed changes
Overall outlook with evidence and confidence
Next control point and escalation needs
```

Keep the snapshot decision-oriented. Link detailed plans, registers, budgets,
specifications and evidence rather than pasting them into every update.

## Composition boundaries

- `interview-me`, `idea-refine` and `specification` establish intent and required
  behavior; `project-management` controls the accepted project around them.
- `planning` owns detailed work units, dependencies, sequencing and execution
  checkpoints; `project-management` owns commitments, capacity and forecast.
- `handoff` owns portable continuation state; `project-management` owns the
  maintained project position it references.
- `documentation` owns durable document and ADR lifecycle;
  `project-management` owns decision authority, status and project impact.
- `research` owns source-backed factual synthesis; `project-management` uses
  accepted evidence to update assumptions and decisions.
- `risk-management` owns systematic risk identification, analysis, responses
  and residual exposure; `project-management` owns how the material risk
  position affects the integrated baseline and forecast.
- `roadmap` owns the outcome-oriented trajectory, horizons and prioritization
  rationale; `project-management` owns accepted commitments, resources and
  project control around that direction.
- Delivery and retrospective capabilities, when selected, own rollout/transition
  mechanics and structured learning respectively.
- Domain and technology skills supply constraints, estimates and evidence
  specific to the work; they do not replace integrated project governance.

This skill does not automatically invoke another skill or mutate an external
project system.
