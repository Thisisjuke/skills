---
name: product-design
description: Design or resolve a product experience by connecting user and product outcomes, flows, alternatives, interactions, states, evidence, and trade-offs into a testable direction before implementation.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED domain-modeling"
  uses-glossary: "true"
---

# Product design

Turn an understood product problem into a coherent, testable design direction.
Connect user outcomes, product intent, flows, information, interactions, states
and trade-offs before implementation commits the team to a solution.

This skill owns the product-design frame, experience model, alternatives,
interaction direction, design rationale and validation questions. It does not
own user-research protocol, project roadmap, visual styling system,
accessibility conformance, detailed specification, UI engineering or production
implementation.

Design work is read-only by default. Create or update design artifacts only
when requested or clearly part of the authorized task. Do not change product
scope, publish a design, implement code or represent assumptions as approved
decisions without the corresponding authority.

## Ground the design

Inspect the evidence and existing product context before proposing a direction:

- intended users, their situation, goals and constraints;
- product or organizational outcome and definition of success;
- current journey, workarounds, pain points and surrounding channels;
- research, analytics, support evidence and prior design decisions;
- existing terminology, information architecture, patterns and design system;
- technical, operational, legal, accessibility and business constraints;
- scope, non-goals, decision owner and maturity of the work.

Consult applicable `GLOSSARY.md` files from the repository root toward the
target when they exist; an explicit nearer definition takes precedence.
Distinguish observed evidence, accepted decisions and assumptions. Stakeholder
opinion is useful context but is not automatically evidence of a user need.

If the problem or intended outcome is unresolved, expose the gap instead of
decorating a presumed solution. Do not fabricate personas, research findings,
analytics or constraints.

## Frame the experience

Describe the experience from the user's point of view:

- trigger and entry point;
- desired outcome and evidence of completion;
- essential decisions, information and actions;
- stages, transitions and channels involved;
- dependencies on people, systems or prior knowledge;
- moments of uncertainty, trust, effort or failure;
- recovery, cancellation and continuation paths;
- needs that differ by context, ability, device or role.

Choose the smallest useful representation: a task flow, state model, journey,
service blueprint, information map or another established project artifact.
Do not force every design into screens, and do not map an entire service when a
bounded interaction is the real decision.

## Define design principles for this problem

Translate evidence and constraints into a few decision criteria specific to the
work. They may concern comprehension, effort, speed, safety, trust,
discoverability, continuity, accessibility, reversibility or another relevant
quality.

Make trade-offs explicit. “Simple”, “intuitive” and “delightful” are not useful
criteria until connected to a user, task and observable outcome. Preserve hard
constraints without treating conventions or current implementation accidents as
immutable requirements.

## Explore alternatives

Generate meaningfully different approaches before converging when the decision
is consequential or uncertain. Vary the model, sequence, information,
responsibility or interaction—not merely color and layout.

For each viable direction, explain:

- the underlying idea and user outcome;
- what becomes easier or harder;
- assumptions and evidence required;
- important states and edge cases;
- consequences for accessibility, operations and implementation;
- compatibility with existing product patterns;
- reversibility and cost of learning.

Include the current approach when it is a credible option. Reject alternatives
for explicit reasons rather than choosing the first plausible design.

## Resolve the interaction direction

For the selected or recommended direction, define enough behavior to make it
reviewable:

- entry, orientation and primary path;
- information grouping, hierarchy and navigation;
- actions, labels, feedback and system status;
- loading, empty, partial, error, offline and success states as applicable;
- validation, prevention, recovery, undo and cancellation;
- permissions, destructive actions and trust-sensitive moments;
- responsive or constrained-context changes where evidence requires them;
- content and terminology that materially affect comprehension;
- unresolved choices that should remain open.

Prefer recognition over hidden knowledge and preserve context across detours or
failures. Do not assume one web, mobile, touch or desktop interaction model.
Domain and technology capabilities determine what is feasible on the selected
platform.

## Choose the right validation artifact

Match fidelity to the unanswered question:

- sketch or flow for structure and sequence;
- content draft for language and comprehension;
- state model for transitions and edge cases;
- wireframe for hierarchy and layout;
- interactive prototype for behavior and task flow;
- production-like prototype only when realism is necessary for the evidence.

A prototype is a learning instrument, not proof of usability or production
readiness. State what the artifact can test, whom it should be tested with, the
task or scenario, the evidence to collect and the decision that evidence will
inform. Avoid polishing details that cannot change the current decision.

## Converge without overstating certainty

Recommend a direction from evidence and criteria. Capture:

- selected approach and rationale;
- alternatives considered and why they were not selected;
- expected user and product outcomes;
- assumptions, risks and unresolved questions;
- constraints and decisions that must be preserved downstream;
- validation completed and evidence still required;
- next decision and its owner.

When evidence is weak, recommend the next learning step rather than presenting
the design as validated. A design review, stakeholder approval and usability
test answer different questions; name which one occurred.

## Return a design direction

Use an established project artifact when one exists. Otherwise return:

```text
Problem, users, context and intended outcome
Evidence, assumptions, constraints and non-goals
Current experience and key friction
Design principles and decision criteria
Alternatives and trade-offs
Recommended flow, information and interaction model
Important states, recovery and content decisions
Accessibility and operational considerations
Validation questions, artifact and evidence plan
Open decisions, owner and downstream constraints
```

Keep detailed requirements and implementation tasks in their owned artifacts.
Link source evidence instead of duplicating it.

## Composition boundaries

- `interview-me` and `idea-refine` clarify intent and broaden ideas;
  `product-design` turns accepted context into an experience direction.
- `research` and future user-research capabilities own evidence collection and
  synthesis; `product-design` owns design implications and choices.
- `ux-audit` evaluates an existing experience; `product-design` may use its
  findings to explore and resolve a future direction.
- `accessibility` owns access needs, requirements and bounded conformance
  evidence; `product-design` integrates relevant constraints into the design.
- Visual-review skills own visible artifact critique and comparison;
  `product-design` owns the product and interaction rationale behind them.
- `design-system` owns reusable tokens, components and governance;
  `product-design` uses that system to shape a product-specific experience.
- `specification` owns durable required behavior and acceptance;
  `product-design` supplies the validated direction and unresolved decisions.
- `implementation` and technology skills own repository mutation and platform
  mechanics.

When new or ambiguous domain language becomes part of the accepted design,
call `domain-modeling` if available and not already active in the invocation
chain. Report the gap if it is unavailable or the call would create a cycle;
do not copy its glossary workflow.

This skill has no other skill call and does not auto-load a missing capability.
