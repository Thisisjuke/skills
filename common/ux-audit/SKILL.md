---
name: ux-audit
description: Evaluate journeys, navigation, discoverability, feedback, errors, friction, cognitive load, and task completion.
license: MIT
metadata:
  uses-glossary: "true"
---

# UX audit

Evaluate how well an existing product, service, flow or bounded interaction
supports intended users in completing a goal. Ground findings in inspected
evidence and make the limits of heuristic analysis explicit.

This skill owns audit scope, task walkthrough, UX evidence, friction analysis,
finding prioritization, systemic patterns and recommendation framing. It does
not own user-research execution, accessibility certification, visual-fidelity
comparison, product strategy, redesign, implementation or generic code review.

An audit is read-only by default. Do not fix the experience, change production
state, publish findings or perform consequential user actions unless separately
authorized. Use test or disposable data where walkthrough actions could mutate
real state.

## Establish the audit contract

Determine:

- the product, service or surface in scope;
- the intended users, context and goal;
- the starting state, end condition and critical path;
- included channels, devices, roles, variants and edge states;
- available evidence and access limitations;
- relevant product decisions, terminology and known constraints;
- the audience and decision the audit should support.

Consult applicable `GLOSSARY.md` files from the repository root toward the
target when they exist; an explicit nearer definition takes precedence. Define
a bounded audit instead of promising to assess “the whole UX”. Include upstream
discovery and downstream support or offline steps when they materially affect
task completion.

## Classify the evidence

State which evidence supports each conclusion. Useful layers include:

- direct walkthrough and observed system behavior;
- screenshots or recordings of specific states;
- structure, semantics, copy or implementation inspection;
- analytics, support records and prior research;
- observation of representative users attempting realistic tasks.

These layers are not interchangeable. A screenshot can support a visible
hierarchy finding but not keyboard behavior. An expert walkthrough can identify
likely friction but not prove how frequently users encounter it. Analytics can
show where people stop but not by itself explain why. Only call an observation
a usability-test result when representative users performed a defined task.

If the experience cannot be accessed or an important state cannot be observed,
name the blocker. Indirect documentation may inform hypotheses but does not
replace inspection of the actual experience.

## Walk the task as a sequence of states

Start from the user's trigger, not from an arbitrary screen. For each step,
record:

- what the user is trying to understand or accomplish;
- available information and visible next actions;
- action taken and resulting state;
- feedback, delay and preservation of context;
- decisions, memory or data demanded from the user;
- prevention, recovery, cancellation and continuation paths;
- evidence captured and anything that remained unobservable.

Inspect relevant loading, empty, partial, validation, error, permission,
success, repeat-use and interruption states. Test alternate paths only within
the authorized scope. Do not infer a healthy journey from the happy path alone.

## Evaluate the experience

Use lenses that matter to the scoped goal rather than a ritual checklist:

- task entry and discoverability;
- match between product language and user concepts;
- information architecture, orientation and navigation;
- clarity of choices, labels, content and hierarchy;
- system status, feedback and progress;
- cognitive, physical and procedural effort;
- consistency and expectation across steps and channels;
- error prevention, recovery, undo and retained work;
- trust, reassurance, permissions and consequence clarity;
- efficiency for new, occasional and frequent users;
- continuity across devices, roles, interruptions and support;
- whether the user can achieve the intended outcome effectively.

Consider accessibility as a user-experience concern, but compose with
`accessibility` for systematic requirements and evidence. Do not claim
conformance from screenshots or heuristic review.

## Write evidence-bound findings

For each material finding, provide:

```text
Title
Affected user goal and step/state
Observed evidence
Friction or failure mechanism
Likely user and product impact
Reach/frequency evidence, or Unknown
Severity and rationale
Confidence and evidence limits
Recommendation or design question
Verification needed
```

Prioritize from task impact, affected users, recurrence, recoverability and
consequence. Use an existing severity model when defined; otherwise use a small
explicit scale. Do not multiply invented scores or equate visual polish with
task failure.

Separate:

- confirmed behavior from likely risk;
- isolated defects from recurring systemic patterns;
- structural issues from presentation polish;
- user harm from internal inconvenience;
- recommendation from evidence.

Record strengths when they materially explain what should be preserved. Avoid
generic praise and personal taste.

## Synthesize patterns and actions

Group findings by shared cause, journey stage or user outcome. Identify:

- highest-impact barriers and broken paths;
- repeated language, navigation, state or feedback problems;
- strengths and conventions worth preserving;
- quick corrections versus questions requiring design exploration or research;
- evidence gaps that could change priority;
- dependencies, owners and affected surfaces where known.

Recommend outcomes, constraints or questions before prescribing a detailed
redesign. A design alternative belongs to `product-design`; implementation
belongs to its selected capability. If several solutions are plausible, explain
the trade-off instead of presenting one preference as inevitable.

## Return an auditable report

Use an established audit format when one exists. Otherwise return:

```text
Scope, users, goal and tested context
Evidence sources and limitations
Journey or state sequence inspected
Overall assessment and preserved strengths
Prioritized findings with evidence
Cross-cutting patterns and affected steps
Highest-value recommendations or design questions
Research, accessibility and technical verification gaps
Next decision and accountable owner when known
```

Attach or link evidence where the medium permits it. Protect personal,
sensitive and production data. Do not claim comprehensive coverage when states,
users or interaction modes were not inspected.

## Composition boundaries

- `review` owns generic review scope, finding discipline and reporting when
  selected; `ux-audit` supplies UX-specific evidence and lenses.
- `product-design` owns exploration and future design direction; `ux-audit`
  diagnoses the current experience without silently redesigning it.
- `accessibility` owns accessibility scope, requirements and bounded claims;
  `ux-audit` reports only supported UX and accessibility risks.
- `screenshot-critique` and `compare-screenshots` own visual inspection and
  comparison; `ux-audit` owns task flow, interaction and user-outcome impact.
- `performance` owns measurement method; `ux-audit` describes user-visible
  delay or interruption and composes with it for causal performance evidence.
- Domain and technology skills provide platform constraints and feasible
  evidence methods without replacing UX reasoning.
- `implementation` owns any authorized correction.

This skill does not automatically invoke another skill or authorize a redesign.
