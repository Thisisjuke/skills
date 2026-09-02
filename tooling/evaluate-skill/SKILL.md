---
name: evaluate-skill
description: Evaluate an Agent Skill's routing, execution quality, reliability, composition, and incremental value with isolated realistic cases and evidence. Use when deciding readiness, investigating missed or false triggers, comparing skill versions, or hardening a skill after observed failures.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED validate-skill-system; CALLS WHEN NEEDED author-skill"
---

# Evaluate skill

Test a skill as an operational capability, not as prose. Separate structural
validity, routing, execution quality, incremental value and human acceptance;
evidence in one dimension does not prove the others.

This skill owns evaluation design, isolated execution, grading, comparison,
failure diagnosis and regression evidence. It does not own the target skill's
content, installation, production authorization or final human readiness
decision.

## 1. Establish the evaluation contract

Resolve the exact target skill and any composition under test. Read its
`SKILL.md`, referenced resources, applicable repository contract and current
catalogue state. State its advertised purpose in one sentence.

Confirm the evaluation intent:

- evaluate only;
- compare with no skill, a previous version or another composition;
- investigate a known miss, false trigger or harmful behavior;
- improve the target after evidence and authorization.

Identify the available execution facilities: fresh sessions or agents,
isolated writable roots, invocation traces, token/time measurements and human
review. Do not promise blind or routing evaluation when the harness cannot
provide the needed isolation or observability.

Use [the evaluation protocol](references/evaluation-protocol.md) when designing
cases, runner and judge packets, baselines or reports.

Completion criterion: target, capability claim, comparison, permissions and
available evidence lanes are explicit.

## 2. Perform structural preflight

Run the target ecosystem's parser and validators before behavioral execution.
Record structural failures separately; do not repair them before capturing the
requested baseline.

When the selected distribution uses the compatible portable contract and
`validate-skill-system` is available, call it with the exact skill roots and
whether they form a complete or deliberately partial installation. The
validator owns frontmatter, target, call-graph and glossary-contract findings;
`evaluate-skill` retains ownership of cases, execution and verdicts. The call
is read-only and grants no authority to rewrite or install anything.

Before calling, inspect the active invocation chain. Do not call
`validate-skill-system` when it is already active or is an active ancestor;
report the cycle. If it is unavailable, use any native validator, disclose the
missing contract gate and continue only with evidence that remains valid.

Completion criterion: structural status and any unvalidated contract surface
are recorded without being mistaken for behavioral quality.

## 3. Design discriminating cases

Build cases from realistic requests, failures and target users. Keep the suite
proportional, but cover the dimensions that matter:

- positive routing: prompts that should select the skill;
- negative routing: nearby prompts that should not select it;
- core execution: ordinary advertised outcomes;
- boundary and failure behavior: ambiguity, unavailable inputs, permissions or
  partial completion;
- standalone and composition behavior where the skill advertises either;
- regression cases for observed defects.

Each case needs an input, fixtures, allowed side effects, evaluation mode,
success bar and failure smells. Use exact assertions for conformance and
observable invariants. Use a qualitative bar for judgment tasks; do not encode
the desired answer as a checklist.

Choose a meaningful baseline: no skill for incremental value, a preserved old
version for revisions, or another explicit composition for overlap questions.
A case that both configurations pass identically may show that the skill adds
no useful behavior.

Completion criterion: every case can distinguish a valuable outcome from a
plausible but wrong or unnecessary one.

## 4. Execute without leakage

For execution cases, give each fresh runner only the target skill or
composition, the user input, required fixtures, allowed tools and output
location. Do not reveal the success bar, failure smells, expected answer,
baseline output or suspected defect.

For routing cases, expose the same discovery catalogue the real client would
see and do not explicitly invoke the target. Record whether it was selected,
ignored or confused with an adjacent skill. Forced invocation proves execution
only, never routing.

Use a separate clean run for each case and configuration. Keep side effects in
an isolated temporary workspace unless the case genuinely requires another
environment and has explicit authority. Capture outputs, relevant trace,
selected skills, tool failures, duration and token use when available. Inspect
the live source checkout after every run and report contamination; do not erase
unexpected user files as cleanup.

If fresh contexts or isolation are unavailable, stop at a designed-case or
author-side scenario review. Label it `NON-INDEPENDENT`; do not claim blind
behavioral evaluation.

Completion criterion: artifacts are attributable to one case and
configuration, with no evaluation criteria leaked to the runner.

## 5. Grade independently

Use a judge that did not run the case. Give it the artifact and trace, the
skill's advertised purpose, the success bar and failure smells. Hide
configuration identity during comparative judgment when practical.

For every assertion or bar dimension, return pass, fail or ungradable with
specific evidence. A heading, keyword or numeric score alone is not proof of
substance. Review the case itself when an assertion is trivial, impossible,
brittle or unverifiable.

Compare quality gained against time, tokens and operational complexity. Cost
is a trade-off, not a universal rejection threshold. Repeat high-risk,
borderline or inconsistent cases enough to reveal instability, and report raw
outcomes rather than implying statistical confidence from tiny samples.

Completion criterion: every verdict is evidence-backed, baseline-relative and
honest about ungradable dimensions and variability.

## 6. Diagnose without teaching to the test

Classify each failure before proposing a change:

- target-skill defect;
- discovery-description defect;
- bad or non-discriminating case;
- missing domain evidence;
- harness, tool or environment limitation;
- composition or configuration gap;
- nondeterministic result needing more runs.

Look for the smallest general defect: missing decision rule, vague completion,
misplaced reference, hidden dependency, excessive prescription, permission
leak, unsupported assumption or unnecessary instruction. Do not add case names,
exact expected wording or one-off fixes to make a benchmark green.

Human review remains necessary for usefulness, surprising failure modes and
qualities the suite did not encode.

Completion criterion: each observed miss has a defensible cause, and wrong
cases are corrected instead of forcing the skill to satisfy them.

## 7. Delegate authorized repairs

If the user requested improvement or approves a proposed repair and
`author-skill` is available, call it after diagnosis. Pass the target package,
evidence-backed defect, affected cases, preserved behavior, source constraints
and mutation boundary. `author-skill` exclusively owns skill edits and resource
design; `evaluate-skill` owns the unchanged cases, reruns and comparative
verdict. The call never authorizes unrelated edits or installation.

Check the active invocation chain before calling. Do not call `author-skill`
when it is already active or is an active ancestor; report the cycle. If the
target is unavailable, complete the evaluation and return a repair brief
without modifying the skill or reproducing the absent authoring workflow.

After an authorized repair, rerun all cases that cover the changed behavior
plus representative previously passing cases. Compare against the preserved
baseline. Stop when the agreed bar is met, improvement plateaus, a harmful
regression appears or the next iteration needs new human judgment.

Completion criterion: repair authority, changed owner and regression result
are explicit; evaluation-only requests leave the target unchanged.

## Report

Return:

- target, version/composition and evaluation mode;
- structural result and evidence limitations;
- case matrix with configuration, runs, verdict and cited evidence;
- baseline-relative quality and available cost observations;
- failure classification and case-quality findings;
- repairs performed or proposed, with authorization status;
- regression results and remaining human decisions;
- the strongest justified claim: structurally valid, scenario-reviewed,
  behaviorally evaluated, or human-validated.

Never promote readiness or claim that a skill improves outcomes when the
available evidence establishes only structure, a forced single run or an
author-side review.
