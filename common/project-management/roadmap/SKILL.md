---
name: roadmap
description: Create, assess, or maintain an outcome-oriented roadmap that connects a vision to prioritized horizons, learning, dependencies, uncertainty, and explicit choices without turning it into a backlog or false delivery commitment.
license: MIT
---

# Roadmap

Express a credible direction from a desired future state to nearer choices.
Make priorities, intended outcomes, uncertainty and excluded work understandable
without disguising a feature list, release calendar or task plan as strategy.

This skill owns roadmap purpose, audience, horizons, themes or outcome
increments, prioritization rationale, learning gates, scenarios, confidence and
change history. It does not own product discovery, detailed requirements,
execution planning, project governance, release commitments or portfolio
funding decisions.

Roadmap analysis is read-only by default. Update a local roadmap only when
requested or clearly part of the authorized task. Publishing a roadmap,
changing a commitment or representing another stakeholder's priority requires
the corresponding authority.

## Establish the roadmap contract

Determine:

- the vision or future condition the roadmap should advance;
- the users, beneficiaries or objectives that define value;
- the decision the roadmap must support and its intended audience;
- the scope boundary: product, service, platform, capability, organization or
  another coherent subject;
- the planning horizon and level of precision that evidence supports;
- known commitments, constraints, dependencies and decision rights;
- the owner, source of truth, review cadence and change mechanism.

If the vision, problem or outcomes are unresolved, expose that gap rather than
inventing a roadmap. A roadmap may coordinate several kinds of work, but it
must have one clear purpose and reading convention.

## Separate the roadmap from neighboring artifacts

Keep the distinctions explicit:

- a **roadmap** communicates direction, priority and likely evolution;
- a **strategy** explains the choices and logic for achieving objectives;
- a **backlog** contains candidate or selected work at delivery resolution;
- a **plan** decomposes accepted work into dependencies and checkpoints;
- a **release schedule** communicates intended delivery or availability dates;
- a **project baseline** records authorized commitments and control dimensions.

Link these artifacts when useful, but do not duplicate their contents. A
roadmap item should be traceable to evidence and downstream detail without
embedding every task, requirement or status update.

## Build around outcomes and choices

Structure roadmap items around a user, business, operational or capability
outcome. For each meaningful item, capture:

- the problem, opportunity or desired change;
- the objective or value hypothesis;
- evidence supporting its current priority;
- the success signal or learning needed;
- important constraints and dependencies;
- the horizon, sequence or decision window;
- confidence and assumptions appropriate to that horizon;
- what is deliberately excluded or deferred.

Features and projects may appear as candidate means, but should not replace the
outcome. Preserve alternative approaches when the solution is not yet decided.
Use plain language that the target audience can understand.

## Choose horizons honestly

Use `Now / Next / Later`, fixed periods, phases, maturity stages or another
model that fits the decision. Define what each horizon means. Avoid exact dates
where there is no accepted commitment or credible forecast.

Precision should decrease with distance and uncertainty. Near-term work may
have evidence, owners and decision gates; later horizons may contain broader
outcomes, options and assumptions. State which items are committed, forecast,
exploratory or contingent instead of relying on layout alone.

Show meaningful sequence and dependencies without implying that every stream
is linear. Where uncertainty creates materially different paths, represent
scenarios, trigger conditions or decision points rather than one deterministic
future.

For work with a large unknown surface, keep a compact decision map: confirmed
direction, unresolved forks, evidence that would resolve each fork, and the
nearest useful frontier. Revise the map as uncertainty retires instead of
expanding a speculative end-to-end backlog.

## Prioritize transparently

Explain why one outcome precedes or displaces another. Consider, as relevant:

- expected value and affected beneficiaries;
- evidence strength and urgency;
- strategic alignment and obligations;
- dependencies, opportunity windows and sequencing;
- risk reduction and learning value;
- capacity, cost and organizational constraints;
- reversibility and cost of delay.

Use an established prioritization model when the organization has one. Do not
invent scores to simulate objectivity. If stakeholders disagree on weights,
assumptions or outcomes, show the disagreement and identify the decision owner.

A roadmap must also communicate what is not being pursued. Avoid silently
retaining every request in a distant horizon; explicit rejection, deferral or
reconsideration conditions make the roadmap more trustworthy.

## Connect learning and evidence

For uncertain roadmap items, define the evidence that should change the
decision. Examples include user research, technical feasibility, operational
performance, regulatory clarification, dependency completion or measured
outcomes.

Use learning gates to decide whether to continue, adapt, accelerate, defer or
stop. Do not treat completion of an activity as proof that its intended outcome
occurred. Update the roadmap from observed value and changed context, not only
from delivery status.

## Maintain the roadmap

At each useful review:

- compare current priorities with the vision and evidence;
- update outcomes, horizons, confidence and dependencies;
- record material additions, removals and changes in rationale;
- reconcile accepted commitments with project and delivery artifacts;
- retire stale items instead of letting `Later` become indefinite storage;
- verify that linked backlogs and plans have not become the de facto roadmap;
- communicate changes to the audiences affected by them.

Keep one canonical roadmap when several presentations exist. Tailor views for
different audiences without allowing their underlying priorities and status to
diverge.

## Return an understandable roadmap

Use the project's established format when one exists. Otherwise return:

```text
Vision, scope, audience and owner
Roadmap convention and planning horizon
Outcomes by horizon, phase or scenario
Priority rationale and excluded work
Success signals and learning gates
Dependencies, constraints and assumptions
Confidence, contingencies and decision points
Material changes since the previous version
Linked evidence, backlog, plans and project baseline
Next review date and decisions required
```

Keep the main view concise enough to scan. Put detailed requirements, tasks,
research and risk records in their owned artifacts and link them.

## Composition boundaries

- `project-management` owns authorized scope, milestones, resources and
  forecast; `roadmap` owns the directional, outcome-oriented view.
- `risk-management` owns systematic uncertainty analysis and responses;
  `roadmap` represents only the risks, scenarios and triggers that alter
  direction or priority.
- `planning` owns detailed work, dependencies and execution checkpoints;
  `roadmap` owns horizons and strategic sequence at a higher resolution.
- `specification` owns required behavior and acceptance; `roadmap` states why
  and when an outcome is pursued without specifying the solution in detail.
- `idea-refine`, `research`, `product-design` and domain skills may supply
  options and evidence; `roadmap` owns their prioritized trajectory.

This skill does not automatically invoke another skill or create a commitment.
