---
name: test-driven-development
description: Implement a feature or bug fix test-first through small red-green-cleanup cycles when the user explicitly requests TDD or red-green-refactor evidence.
license: MIT
metadata:
  skill-calls: "ALWAYS CALLS testing; ALWAYS CALLS implementation"
---

# Test-driven development

**ALWAYS CALLS:** `testing` and `implementation`.

Use a failing behavioral example to drive one small production change at a
time. This is an implementation mode selected explicitly for TDD; it is not a
rule that every code change must begin with a test.

`testing` owns the behavior contract, oracle, observation seam and signal
quality. `implementation` owns authorized production mutation, preservation of
existing work, validation and final diff accountability. This skill owns cycle
size, red/green evidence and the discipline of letting each cycle inform the
next.

## Establish the cycle contract

Identify the requested behavior or reproduced bug, source of expected behavior,
target repository, available test command and authorized production changes.
Read applicable instructions and nearby test conventions.

Before work, check the active invocation chain. Call `testing` with the behavior,
risk, likely public seam and oracle. Then call `implementation` with the bounded
production-change authorization and evidence requirements. If either target is
unavailable or would create a cycle, report the configuration gap and do not
claim the TDD workflow completed. Missing `implementation` prevents production
mutation; missing `testing` prevents a trustworthy TDD cycle.

Neither call grants permission to install dependencies, modify external state,
commit, push or deploy.

## Choose one vertical slice

Select the smallest consumer-visible example that advances the request. Prefer
a public function, component boundary, protocol surface, persisted effect or
user path over a private method or collaborator call.

Do not design a speculative batch of tests before learning from the first
implementation. Split by behavior, not horizontal layers. Confirm a
consequential new public interface when choosing it would decide behavior not
already established by the request.

## Red: prove the test can fail

Write one discriminating test through the selected seam. Derive the expected
result independently from production: a requirement, confirmed example,
invariant or known failure.

Run the focused test and verify it was executed, fails for the intended missing
or incorrect behavior, and is not failing from syntax, fixture, environment or
unrelated setup. Do not change existing behavior merely to manufacture red.

If reproduction is unsafe or impossible, use a controlled mutation,
counterexample or alternate evidence accepted by `testing`, and disclose that
this was not a literal red-first run.

## Green: make only this slice pass

Through `implementation`, make the smallest coherent production change that
satisfies the current test and required neighboring behavior. Do not hard-code
the example, weaken the assertion, update an unrelated snapshot or add imagined
future functionality merely to reach green.

Run the focused test and relevant neighbors. A pass is credible only if the red
evidence showed the test could detect the targeted defect.

## Cleanup while green

Improve names, duplication and local structure only inside the authorized slice
while behavior remains green. If structural work becomes a distinct refactor
with broader consumers or migration risk, stop and route it separately. Run the
focused and affected tests after cleanup and preserve the behavioral oracle.

## Continue or stop

Record what the cycle taught and choose the next smallest in-scope slice. Stop
when the requested behavior and boundaries are evidenced, the next test would
specify unapproved behavior, a decision or environment blocks red, or more
cycles would add coverage without meaningful risk reduction.

Before completion, run the proportionate broader gate and inspect the final
diff through `implementation`. Report the seam, red and green evidence per
cycle, cleanup, broader checks, skipped evidence and remaining risk.
