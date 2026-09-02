---
name: testing
description: Design useful behavioral tests with clear contracts, deterministic signals, regression value, and appropriate scope.
license: MIT
metadata:
  uses-glossary: "true"
---

# Testing

Design tests that provide durable evidence about behavior. Choose the cheapest
stable observation point that can fail when the contract breaks, control the
variables that affect the result, and make failures explain what regressed.

This skill owns technology-neutral test strategy and test quality. It does not
own framework syntax, runner commands, emulator or browser operation, or
platform-specific fixtures; selected technology testing skills supply those
mechanics when useful.

## Establish the behavior contract

Before adding or changing a test, identify:

- the behavior, invariant or failure being protected;
- the consumer-visible input and outcome;
- the source of truth for the expected result;
- the risk that justifies durable automation;
- whether the request is test design, test implementation, repair of a failing
  test, or review only.

Use requirements, confirmed domain behavior, public interfaces and known-good
examples as the oracle. Do not silently treat the current implementation as the
specification. If expected behavior is genuinely ambiguous, surface the missing
decision rather than freezing an accident into a test.

Read applicable repository instructions, established test commands and nearby
test conventions before choosing a location or tool. Consult applicable
`GLOSSARY.md` files from the repository root toward the target, with an
explicit nearer definition taking precedence, so test names and scenarios use
canonical project language.

## Choose the observation seam

Test through the outermost practical seam that observes the contract while
keeping setup, runtime and diagnosis proportionate. A useful seam may be a
public function, component boundary, service interface, persisted effect,
protocol response or complete user path.

Choose scope by the evidence needed, not by a fixed test pyramid:

- isolate intricate deterministic logic when a narrow test proves it clearly;
- cross real boundaries when their integration is part of the risk;
- use an end-to-end path for critical behavior that lower seams cannot prove;
- avoid a broad environment when a smaller stable seam gives the same evidence.

Do not reach into private structure merely because it is easy to assert. A test
that fails on a behavior-preserving refactor is coupled to the implementation,
not the contract.

## Construct a discriminating scenario

Use the smallest scenario that distinguishes correct from incorrect behavior.
Control irrelevant variables and make the expected result independent from the
implementation under test.

Cover the dimensions that materially change the contract, such as:

- normal, boundary and invalid inputs;
- state transitions and idempotency;
- ordering, duplication and partial failure;
- authorization or ownership boundaries;
- time, concurrency, retry or interruption;
- resource cleanup and recovery.

Do not enumerate combinations without a risk model. Prefer representative
partitions, known failure modes and invariants over a large collection of
nearly identical examples. Coverage numbers can reveal unexercised code, but
they do not prove that assertions are meaningful.

## Keep the signal deterministic

Control clocks, randomness, locale, environment, configuration and external
state when they are not the subject of the test. For genuinely stochastic
behavior, assert a justified invariant or distribution over enough observations
rather than pinning one lucky sample.

Keep each test independent of execution order and previous failures. Isolate
mutable state and release resources even when the assertion fails. A retry is
not a repair for flakiness: identify whether the nondeterminism comes from the
product, the fixture, the environment or an invalid expectation.

## Use doubles at real boundaries

Prefer real, controlled implementations when they remain fast and reliable.
Use fakes, stubs or mocks where an external dependency is slow, destructive,
unavailable or nondeterministic.

Do not mock internal collaborators merely to verify call sequences. Assert the
observable outcome when one exists. Every double encodes an assumption about a
boundary; keep that contract small and ensure a higher-level test or another
verification path checks that the assumption still matches reality.

## Prove regression value

For a bug fix, reproduce the failure before changing the implementation when it
is practical and safe. Confirm that the test is red for the expected reason,
then make it green and run relevant neighboring tests. When the fix already
exists or reverting it would be unsafe, introduce a controlled mutation,
fixture or counterexample that demonstrates the assertion can detect the bug.

Do not require a red-first cycle for documentation-only changes, generated
artifacts, exploratory spikes, or situations where creating the failure would
be unsafe or disproportionately expensive. State the alternate evidence used.

When test-driven development is requested, work in small vertical cycles:

```text
one failing behavioral test → minimum implementation → green → cleanup
```

TDD is an implementation mode, not a mandatory property of every testing task.
Do not write a speculative batch of tests against an interface that is still
being discovered.

## Run and interpret tests

During iteration, use the repository's focused command to keep feedback clear.
Before completion, run the smallest broader suite that covers plausible impact
and any project-mandated gate. Verify the command, selected tests, exit status,
skips and infrastructure errors; “no red output” is not proof that the intended
tests ran.

When a test fails, classify the failure before editing code or expectations:

- product behavior violates the contract;
- the contract intentionally changed and the test is obsolete;
- the test is coupled to an implementation detail;
- the fixture or environment is invalid;
- the signal is nondeterministic;
- the failure is unrelated but blocks trustworthy verification.

Never weaken an assertion, update a snapshot, widen a tolerance or skip a test
solely to recover green. Explain why the expected behavior changed or repair
the invalid test mechanism.

## Report the evidence

Summarize:

```text
Behavior or risk
Chosen seam and why
Scenarios and oracle
Commands and scope executed
Observed red/green evidence when applicable
Skipped or unexecuted checks
Failures and interpretation
Residual gaps
```

Do not claim the entire system is correct because a selected suite passed.
State exactly what the tests demonstrate and what remains outside their scope.

## Composition boundaries

- `testing` supplies behavioral proof, seam selection and signal-quality rules.
- A base technology skill supplies language and runtime constraints.
- A technology testing skill supplies frameworks, runner mechanics, fixtures
  and platform validation techniques.
- `implementation`, when selected, owns changing production behavior; this
  skill owns the evidence that the behavior meets its contract.
- `review`, when selected, owns review findings and reporting; this skill
  supplies the test-quality lens and verification evidence.
- `performance`, when selected, owns workloads, measurement noise and
  performance budgets; this skill owns broader behavioral test strategy.

These responsibilities compose without hidden dependencies. This skill does
not automatically invoke another skill.
