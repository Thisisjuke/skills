---
name: interview-me
description: Clarify an unclear request through focused questions, explicit assumptions, reformulation, and user confirmation.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED domain-modeling"
  uses-glossary: "true"
---

# Interview me

Turn an unclear request into confirmed intent before planning, specification or
implementation begins. Surface assumptions, identify the few uncertainties
that could change the direction, and make it easy for the user to correct the
current interpretation.

This skill needs an interactive user. In a non-interactive workflow, report the
missing decisions instead of guessing or simulating answers.

## Decide whether an interview is useful

Use the interview when different reasonable interpretations would materially
change the outcome, scope or trade-offs. Typical missing information includes:

- who experiences the problem or uses the result;
- the outcome sought beneath a proposed solution;
- why the work matters now;
- observable evidence of success;
- the binding constraint or priority;
- important exclusions, authority or risk tolerance.

Do not interview for a clear mechanical request, a factual explanation, or an
ambiguity that repository evidence resolves cheaply. Respect an explicit user
preference for a quick assumption-based answer: state the consequential
assumptions and proceed within them.

## Load the current context

Read applicable repository instructions and the minimum project evidence needed
to avoid asking questions the repository already answers. Consult every
applicable `GLOSSARY.md` from the root toward the relevant package so questions
and reformulations use established project language.

Do not turn discovery into an exhaustive repository audit. Read only what can
resolve a current uncertainty or expose a contradiction between the request and
the project.

## Form an initial interpretation

State the current best interpretation in one or two sentences, followed by the
specific unknowns that could change it. Separate:

- evidence from the user's words or repository;
- assumptions being made;
- questions that still require the user.

Avoid numerical confidence scores. Confidence is useful only through the
concrete assumptions and missing decisions that justify it.

## Ask focused questions

Ask the smallest set that can resolve the highest-impact uncertainty.

- Prefer one question when its answer determines the next question.
- A short batch is acceptable when the questions are independent and answering
  them together reduces unnecessary turns.
- Attach a concise best guess or candidate interpretation when it helps the
  user react, and explain the evidence behind it.
- Offer two or three concrete choices when they clarify a real branch; do not
  manufacture options merely to fill a format.
- Do not ask the user to repeat facts available in code, documentation or the
  current conversation.

Update the interpretation after each answer. If several rounds do not reduce
the important uncertainty, explain what foundational information is missing
and ask whether to reframe, proceed with an explicit assumption, or stop.

## Challenge solution-shaped requests

When the request names an artifact or technique but not its intended outcome,
test whether the proposed solution is essential or merely the first familiar
answer. Probe vague goals such as “modern,” “scalable,” “clean” or “best
practice” until they become an observable need, constraint or conscious
preference.

Challenge respectfully and concretely. Do not presume that the user's stated
solution is wrong, and do not expand a narrow request into product discovery
without a decision-changing ambiguity.

## Maintain domain language

When the interview establishes a new canonical term, exposes conflicting
meanings, or resolves an ambiguity that should persist, call `domain-modeling`
if it is available and not already active in the invocation chain.

The call is conditional, not a requirement for every interview. Pass the
confirmed meaning and relevant scope; do not copy glossary authoring rules into
this skill. An interview-only request does not authorize a file change, so
obtain explicit human approval before the called skill updates a glossary. If
the skill is unavailable or the call would create a cycle, preserve the agreed
terminology in the final intent and report the configuration gap.

If no glossary exists, continue the interview and do not invoke repository
setup automatically.

## Confirm the intent

When the remaining unknowns would no longer change the next meaningful action,
restate the intent in the user's language:

```text
Outcome: the result being sought
Beneficiary: who needs it
Problem / why now: the situation motivating it
Success evidence: what would show it worked
Constraints and priorities: the limits that govern trade-offs
Out of scope: what is explicitly excluded
Open questions: only material decisions intentionally deferred
```

Ask the user to confirm or correct the restatement. Accept a clear affirmative
instruction to proceed; do not demand ritual wording. Fold corrections into the
statement and reconfirm when they materially change it.

## Stop at confirmed intent

The deliverable is a confirmed statement of intent. Do not produce a solution
catalogue, specification, roadmap, implementation plan or task list unless the
request separately includes that work or another selected skill owns it.

Stop interviewing when additional answers would refine implementation rather
than alter intent. Record intentionally deferred questions instead of forcing
premature decisions.

Report the confirmed intent, assumptions the user accepted, unresolved material
questions and any terminology delegated to `domain-modeling`.
