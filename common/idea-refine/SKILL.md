---
name: idea-refine
description: Expand, challenge, and refine a product, process, or technical idea into a chosen direction with explicit value, assumptions, tradeoffs, validation needs, and non-goals.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED domain-modeling"
---

# Idea refine

Turn an early idea or a seemingly settled direction into a sharper choice worth
taking forward. Explore genuinely different possibilities, expose what each
one assumes, and converge without disguising preference as evidence.

This skill owns divergence, comparison, convergence and the boundary of the
chosen idea. It does not own a full intent interview, external research,
durable specification, execution planning or implementation.

## Choose the useful mode

Match the process to the request:

- **Expand** when the user is anchored on one solution or asks for alternatives.
- **Stress-test** when a preferred idea exists but its value or assumptions are
  uncertain.
- **Converge** when alternatives already exist and a reasoned choice is needed.
- **Full refinement** when the idea is both open and important enough to justify
  divergence followed by convergence.

Do not force a full workshop for a narrow question. If the request only needs
one alternative or one decisive challenge, provide that. If the underlying
intent is too unclear to tell what would count as a better idea, resolve the
few decision-changing questions or use a separately selected `interview-me`.

## Ground the idea

Establish the current best framing:

- who experiences the problem or benefits from the opportunity;
- the change in outcome or experience being sought;
- why the current approach is insufficient;
- known constraints, prior decisions and non-negotiables;
- evidence already available versus beliefs still assumed;
- what decision this refinement needs to enable.

When working in a repository, inspect only the project evidence that can reveal
existing capabilities, constraints or prior art relevant to the idea. Do not
turn ideation into a general codebase audit, and do not let current architecture
silently make every alternative incremental.

Reframe a solution-shaped request around the desired outcome when that opens
useful space. A “How might we” statement can help, but it is a tool rather than
a required format.

## Diverge deliberately

Generate a small set of meaningfully different directions. Vary the mechanism,
beneficiary, boundary or operating model—not merely names and presentation.
Choose lenses that challenge the current framing, such as:

- inversion or reversal;
- elimination and radical simplification;
- adding or removing a binding constraint;
- changing audience, context or scale;
- combining adjacent capabilities;
- rebuilding from known facts rather than conventions;
- borrowing a structural pattern from another domain.

Explain why each direction exists and which assumption it challenges. Include
the current idea as a candidate when it remains credible. Do not reward novelty
for its own sake, generate a large shallow catalogue, or expand beyond the
decision the user needs to make.

## Compare and stress-test

Evaluate each serious direction against dimensions relevant to the context:

- **Value:** whose problem changes, by how much, and compared with what current
  workaround or alternative;
- **Feasibility:** hardest uncertainty, dependencies, capability and time to
  meaningful evidence—not a premature delivery estimate;
- **Distinctiveness:** whether the difference matters to the beneficiary and
  has a defensible cause;
- **Risk and reversibility:** what could invalidate the direction and how much
  commitment precedes learning;
- **Strategic fit:** alignment with confirmed constraints, existing strengths
  and explicit priorities.

Use evidence where it exists. Otherwise label the statement as an assumption.
Classify assumptions by consequence: a dealbreaker if false, an important
influence, or a later optimization. Name what could kill each direction and
what is intentionally ignored for now.

Challenge weak value, accidental complexity and fashionable mechanisms
directly, with reasons. Do not manufacture criticism for balance or remain
neutral when the evidence supports a recommendation.

## Converge on a bounded direction

Recommend one direction when the user asks for a choice. A hybrid is justified
only when its parts reinforce the same outcome; do not combine every attractive
feature to avoid tradeoffs.

Define:

- the chosen direction and why it wins now;
- the beneficiary and intended outcome;
- the smallest experiment or first version that can test the riskiest
  assumption;
- explicit non-goals and rejected alternatives with reasons;
- assumptions to validate and evidence that could reverse the choice;
- material open decisions before specification or execution.

“Minimum” means enough to learn about the core value, not an arbitrary feature
count or delivery deadline. A process change, research probe or manual service
may be a better first test than software.

## Maintain terminology

When refinement establishes a new canonical term, resolves conflicting
meanings or creates a distinction that should persist, call `domain-modeling`
if it is available and the call is acyclic. Pass only the confirmed terminology
and scope. Obtain explicit human approval before the called skill changes a
glossary; ideation alone does not authorize repository mutation.

If the skill is unavailable, preserve the term in the refinement result and
report the configuration gap. Do not invoke repository setup automatically.

## Deliver the refinement

Return a concise artifact or response containing:

```text
Problem or opportunity framing
Beneficiary and desired outcome
Directions considered
Comparison and recommendation
Critical assumptions and validation evidence
Smallest useful experiment or first version
Non-goals and rejected alternatives
Open decisions and handoff readiness
```

Follow an existing idea-document convention when present. Otherwise return
Markdown in the conversation and write a file only when authorized. Do not
silently create `docs/ideas`, a roadmap, specification or task list.

The refinement is ready for specification when the direction and product
boundary are chosen and remaining questions can be expressed as explicit open
decisions rather than hidden behavior choices.

## Composition boundaries

- `interview-me` owns clarification and confirmation of unclear intent;
  `idea-refine` owns generating and choosing possible directions.
- `research` owns external source quality and factual synthesis;
  `idea-refine` owns identifying which assumptions need evidence.
- `specification` owns the durable behavior contract after a direction is
  selected.
- `planning` owns execution decomposition and order after the contract is clear.
- `product-design`, technology or domain skills may supply specialist lenses
  without taking over the refinement method.

Except for the conditional terminology delegation above, this skill does not
automatically invoke another skill.
