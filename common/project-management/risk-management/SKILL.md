---
name: risk-management
description: Identify, analyze, treat, monitor, and communicate uncertainty that could affect objectives across projects, products, operations, or other domains, including both threats and opportunities.
license: MIT
---

# Risk management

Make uncertainty explicit enough for proportionate decisions and action. Treat
risk as the effect of uncertainty on objectives, including both harmful threats
and beneficial opportunities. Do not turn the work into a generic list of bad
things, a compliance ritual, or false numerical precision.

This skill owns risk context, identification, analysis, evaluation, response,
residual exposure, triggers, monitoring and risk communication. It does not own
the integrated project baseline, security threat modeling, detailed execution
planning, incident response, or domain- and technology-specific controls.

Risk analysis is read-only by default. Update a local risk artifact only when
requested or clearly part of the authorized task. Accepting exposure,
committing response capacity, changing an external register or communicating a
risk position externally requires the appropriate human or organizational
authority.

## Establish the decision context

Before rating a risk, determine:

- the objectives, outcomes, assets or obligations that could be affected;
- the decision, planning horizon and audience the analysis must support;
- relevant scope, assumptions, constraints and external conditions;
- existing risk appetite, tolerance, thresholds and escalation rules;
- the evidence available and the important blind spots;
- the cadence or event that should cause reassessment.

Use the organization's established risk model when it is suitable. Tailor the
depth to uncertainty, reversibility and consequence. A small reversible choice
may need a short risk note; a high-impact or irreversible decision may need
multiple scenarios, independent evidence and explicit acceptance authority.

Do not invent appetite or thresholds. If none are defined, state the decision
criteria being used and identify who must confirm them.

## Identify risks as causal statements

Describe a material risk through:

```text
cause or source of uncertainty
event or condition that may occur
effect on one or more objectives
```

For example, “supplier risk” is only a category. A usable risk explains what is
uncertain about the supplier, what may happen and which outcome would change.

Look for uncertainty in objectives, assumptions, dependencies, estimates,
interfaces, people, operations, technology, regulation, security, safety,
market conditions and timing. Consider interactions and common causes instead
of treating every risk as independent. Include opportunities where uncertainty
could improve value, learning, time, cost or another objective.

Keep these records distinct:

- a **risk** has not yet materialized;
- an **issue** is already affecting the work or objective;
- an **assumption** is an unverified belief used in reasoning;
- a **dependency** is a condition on which an outcome relies;
- a **decision** selects an action or accepted exposure.

Convert records when their state changes. Do not leave a materialized risk open
as if it were still hypothetical.

## Analyze and evaluate

For each material risk, assess the dimensions that affect the decision:

- likelihood or plausible frequency;
- impact by affected objective, including range and reversibility;
- proximity, urgency and velocity after a trigger;
- detectability and time available to respond;
- interactions, concentration and common-cause exposure;
- strength, freshness and independence of the evidence;
- confidence in the assessment.

Use qualitative scales only when their meanings are defined. Use quantitative
analysis only when the data, model and decision justify it. Record ranges,
assumptions, correlations and sensitivity; a calculated number is not more
credible than its inputs.

Evaluate exposure against the accepted criteria, not against an arbitrary
universal matrix. Prioritize risks by decision relevance and response urgency,
not merely by multiplying ordinal labels. Surface low-probability,
high-consequence risks when preparedness or escalation is still warranted.

Distinguish individual risks from aggregate or systemic exposure. A set of
moderate risks may share one dependency or exhaust the same response capacity.

## Select and own responses

Choose a response that changes exposure or preparedness. For threats, options
may include avoiding, reducing, transferring or sharing, accepting, preparing
contingency, or escalating. For opportunities, options may include exploiting,
enhancing, sharing, accepting or escalating.

For every active response, capture:

- a responsible risk owner and, when different, a response owner;
- the action, resources and decision authority required;
- the trigger, leading indicator or deadline;
- contingency and fallback where consequence warrants them;
- expected effect on likelihood, impact or readiness;
- residual and secondary risks after treatment;
- the next review point and evidence of completion.

“Monitor” is not a complete response without an owner, signal and next review.
Acceptance means a conscious decision by the correct authority, not absence of
action. Verify that response cost and side effects are proportionate to the
exposure.

## Monitor change

Review risk when evidence, assumptions, scope, environment, dependencies or
objectives change. At each useful checkpoint:

- validate whether causes, likelihood and impact remain current;
- inspect triggers and leading indicators;
- verify response actions and their actual effect;
- identify emerging, secondary and retired risks;
- convert realized risks into issues and activate contingency where relevant;
- update aggregate exposure and escalation needs;
- preserve the rationale for accepted or changed exposure.

Do not keep stale risks open indefinitely. Close a risk only when the relevant
uncertainty or exposure is no longer material, and retain any needed decision
history through the project's documentation convention.

## Return a decision-ready risk view

Use an established register or report format when one exists. Otherwise return:

```text
Context, objectives and decision horizon
Assessment criteria and evidence limits
Material threats and opportunities
Cause → event → objective impact for each risk
Likelihood, impact, urgency and confidence
Interactions and aggregate exposure
Response, owner, trigger and review date
Residual or secondary exposure
Acceptance or escalation decisions required
Changes since the previous assessment
```

Make uncertainties and disputed assessments visible. Separate evidence,
estimate and decision. Avoid burying urgent exposure in a long undifferentiated
register.

## Composition boundaries

- `project-management` owns the integrated project baseline, commitments and
  forecast; `risk-management` supplies deeper risk analysis and response state.
- `roadmap` owns the outcome trajectory and horizon choices; `risk-management`
  exposes uncertainty, scenarios and triggers that may alter it.
- `security` owns adversarial threats, trust boundaries and security control
  objectives; `risk-management` evaluates those risks alongside other
  uncertainty when selected together.
- `planning` owns work decomposition and sequencing; `risk-management` owns
  uncertainty about the plan and risk responses.
- `research` may supply source-backed evidence; `risk-management` owns how
  uncertainty in that evidence affects a decision.
- Domain and technology skills supply context-specific hazards, constraints and
  controls without replacing the risk method.

This skill does not automatically invoke another skill or authorize a response.
