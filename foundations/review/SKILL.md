---
name: review
description: Review code, diffs, or completed changes with a technology-neutral method for scope, evidence, findings, prioritization, and reporting. Use for code review, implementation critique, regression-risk analysis, or a final quality pass; add technology-specific guidance separately when useful.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED domain-modeling"
  uses-glossary: "true"
---

# Review

Produce a focused, evidence-backed assessment of the requested code or change.
Find material defects and risks without turning preferences into requirements.

A review is analysis by default. Do not modify code, documentation, branches,
or external systems unless the request explicitly includes those actions.

## Establish the review contract

Before judging the implementation:

1. Fix the scope: a diff, commit range, branch comparison, files, component, or
   completed body of work. Resolve an obvious scope from the request and
   repository state; ask only when different reasonable scopes would materially
   change the result.
2. Read the repository instructions that apply to the scope, then relevant
   specifications, plans, issue descriptions, tests, and documentation.
3. State important limits: missing requirements, unavailable tools, generated
   code, unexecuted tests, sampled files, or areas requiring expertise not
   present in the selected guidance.
4. Preserve the reviewed state. Do not silently broaden the scope or treat
   unrelated pre-existing problems as regressions caused by the change.

When reviewing a change, inspect both the diff and enough surrounding code to
understand its real behavior. Account for every human-authored changed line in
scope, or disclose what was sampled and why.

## Apply the project's domain language

For each file or package in scope, consult every applicable `GLOSSARY.md` from
the repository root toward that target. Combine broader and narrower
definitions; accept a nearer override only when it explicitly states how it
differs. Use the resulting language when interpreting intent and writing the
review.

Consulting glossaries does not invoke another skill and does not authorize
editing them. If none exists, continue the review and disclose the missing
domain context when it limits a conclusion; do not invoke repository setup.

Call `domain-modeling` only when a project's ambiguous, contradictory or newly
agreed terminology materially affects the review. Ask it to resolve or
formulate the terminology without copying its workflow into this skill.
Preserve the review's mutation boundary: a review-only request requires
explicit human authorization before the called skill changes any glossary. If
`domain-modeling` is unavailable or calling it would create an invocation
cycle, report the terminology question and configuration gap instead of
guessing.

## Review independently along these axes

Keep requirements and implementation quality distinct so one cannot hide the
other.

### Intent and scope

- Does the work implement the stated behavior and acceptance criteria?
- Is required behavior missing, partial, or contradicted?
- Does it introduce unrequested behavior, public surface, dependency, or
  migration cost?

If no specification exists, infer intent conservatively from the request,
tests, interfaces, and surrounding behavior. Label the uncertainty instead of
inventing requirements.

### Correctness and regression risk

- Trace normal, boundary, empty, invalid, failure, retry, and cleanup paths
  that are relevant to the change.
- Check state transitions, ordering, ownership, error propagation, concurrency,
  and resource lifetime where they occur.
- Look for stale callers, names, comments, configuration, tests, and
  documentation after a rename, deletion, or behavior change.
- Distinguish a demonstrated defect from a plausible risk that still needs
  verification.

### Tests and verification

- Check whether tests exercise observable behavior and would fail for the
  regression they claim to prevent.
- Treat passing tests as evidence, not proof that untested contracts are safe.
- Run the narrowest relevant existing checks when doing so is within the
  request's permissions and practical for the repository.
- Never report an unrun, skipped, unavailable, or timed-out check as passing.

### Design and maintainability

- Judge whether responsibilities, boundaries, dependencies, and abstractions
  make the system easier or harder to change safely.
- Flag complexity when it creates a concrete comprehension, modification, or
  defect risk; do not use line counts or personal taste as proof.
- Prefer consistency with documented project conventions. A convention does
  not excuse an objective correctness, safety, or compatibility problem.
- Treat code smells as investigation prompts, not automatic violations.

### Cross-cutting impact

Screen for effects on security, privacy, performance, accessibility,
compatibility, persistence, public APIs, deployment, and operations. Report a
finding when the evidence is sufficient. Otherwise identify the risk or need
for focused expertise without fabricating a specialist conclusion.

Check whether behavior, contracts, setup, migration, operational procedures, or
user-visible changes require corresponding documentation updates.

## Build findings from evidence

For a high-impact, irreversible or assumption-heavy conclusion, isolate the
smallest claim, supporting artifact and contract it must satisfy. When a fresh
review context is available within the authorized workflow, use it to search
for counterexamples, hidden assumptions and contract violations. Do not pass
the original conclusion as a premise or treat the second opinion as a verdict:
reconcile every finding against the artifact. Bound this pass and disclose when
independent context was unavailable.

A finding must be actionable and tied to a concrete consequence. Before
reporting one, verify:

- the cited location exists in the reviewed state;
- the relevant execution or data path reaches it;
- surrounding code, tests, or documented constraints do not already address it;
- the impact is caused or exposed by the reviewed scope, unless the request
  explicitly asks for a broader audit;
- the recommendation addresses the cause rather than merely hiding a symptom.

Group multiple manifestations under one root finding. Omit formatter output,
tool-enforced style, subjective rewrites, and speculative edge cases with no
credible path or impact.

## Qualify each item

Use these kinds:

- **Defect:** the implementation contradicts a requirement or demonstrably
  behaves incorrectly.
- **Risk:** evidence shows a credible failure mode, but available information
  does not prove it occurs.
- **Suggestion:** a non-required improvement with a stated benefit.
- **Question:** missing context prevents a sound judgment. Ask only questions
  whose answers could change the verdict or a finding.

For defects and risks, assign severity by impact and reach:

- **Blocker:** credible catastrophic harm, irreversible loss, critical security
  exposure, or a failure that invalidates the change's primary purpose.
- **High:** major incorrect behavior, data or compatibility damage, or a likely
  regression affecting an important path.
- **Medium:** localized incorrectness or a maintainability problem with a
  concrete future failure mode.
- **Low:** limited impact that is still concrete and worth correcting.

State separately whether the item is **blocking** or **non-blocking** for the
reviewed objective. Severity is not a substitute for that decision.

Use qualitative confidence:

- **High:** directly demonstrated or supported by decisive code and tests.
- **Medium:** strongly supported but one relevant assumption remains.
- **Low:** material uncertainty remains; normally present this as a risk or
  question, not as a definite defect.

## Report

Lead with findings, ordered by blocking status, severity, then confidence. Use
this compact structure for each defect or risk:

```text
[Severity] Title
Kind: Defect | Risk
Disposition: Blocking | Non-blocking
Confidence: High | Medium | Low
Location: file:line
Evidence: what the code does and how the path is reached
Impact: observable consequence and affected scope
Recommendation: smallest sound direction, without writing an unsolicited patch
Verification: evidence that would prove the correction
```

Place suggestions and questions after findings so they cannot obscure defects.
Keep comments about the work, never the author, and explain the reasoning when
it is not self-evident.

End with:

- a verdict: **Blocked**, **Non-blocking findings**, **No findings**, or
  **Inconclusive**;
- verification performed and its outcome;
- residual risks and unreviewed scope;
- documentation impact.

If no material findings exist, say so directly. Do not invent issues to make
the review appear thorough, and do not imply that untested or unreviewed areas
are safe.
