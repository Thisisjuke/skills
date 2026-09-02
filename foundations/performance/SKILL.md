---
name: performance
description: Diagnose, improve, or guard performance with representative workloads, reproducible measurements, bottleneck evidence, budgets, and regression checks.
license: MIT
---

# Performance

Turn a performance concern into measured evidence and a proportionate decision.
Use representative workloads, isolate the limiting resource or wait, and judge
changes against both correctness and measurement noise.

This skill owns the technology-neutral performance method. It does not provide
runtime-specific profilers, framework remedies, product targets or platform
limits; selected technology skills supply those details when useful.

## Establish the performance question

Translate “slow,” “heavy” or “does not scale” into an observable workload and
outcome. Identify:

- the user, system or operational path that matters;
- the input shape, size, concurrency and state;
- the metric that represents the concern;
- the target, budget or regression threshold, if one exists;
- the relevant environment and constraints;
- whether the request authorizes diagnosis only or also implementation.

Do not invent a universal target. Prefer an existing requirement, service
objective, product expectation or historical baseline. When no target exists,
report comparative evidence and the trade-off instead of declaring an arbitrary
pass or failure.

## Build a trustworthy baseline

Reuse established benchmarks, traces, production telemetry and project
commands when they answer the question. Otherwise choose the smallest
measurement that reproduces the relevant workload.

Record enough context to repeat it:

- revision and configuration;
- workload and input data;
- environment, resource limits and dependency state;
- warm-up, cache or startup state;
- sample count or observation window;
- metric definition and units;
- observed distribution or run-to-run variation.

Prefer tail behavior, throughput under load, resource growth or progress rate
when an average would hide the actual failure. Do not compare unlike conditions
or claim precision beyond the measurement method.

If reliable measurement is impossible, identify the missing instrumentation or
fixture and limit the conclusion accordingly. Static code inspection may
produce a risk hypothesis, but not a measured regression.

## Locate the limiting mechanism

Follow the workload through its production path. Attribute elapsed time,
waiting and resource use before proposing a remedy. Depending on the system,
use evidence such as profiles, traces, query plans, allocation data, queue
depth, network timing, I/O volume or counters.

Check especially for:

- work that grows with input, history, tenants or outage duration;
- repeated work, fan-out or avoidable serialization;
- contention, blocking, backpressure failure or queue accumulation;
- retries, polling or fixed-prefix batches that do not guarantee progress;
- memory, storage, handle or connection growth without a lifecycle bound;
- expensive work on a latency-sensitive path;
- a faster local step that merely moves cost or risk elsewhere.

Quantify amplification in concrete units where possible. Falsify the hypothesis
by checking existing caps, indexes, batching, deadlines, recovery owners and
caller frequency. A large bounded operation is not automatically a defect, and
a suspicious code pattern is not automatically the bottleneck.

## Design one attributable experiment

State the causal hypothesis before changing anything:

```text
Under [workload], [mechanism] causes [observed cost].
Changing [one factor] should move [metric] without violating [correctness or
operational invariant].
```

Prefer the smallest change that tests the hypothesis and preserves behavior.
If implementation is not authorized, stop at the experiment or recommendation.
If several changes must travel together, measure their contributions separately
when practical and disclose when attribution remains uncertain.

Treat limits as explicit policies, not magic numbers. Define what is bounded,
why the bound is safe, what happens at the limit, and how overload is observed.
Preserve recovery: caches, shortcuts, batching and backpressure must not hide
drift, lose accepted work or prevent eventual progress.

## Verify the result

Re-run the baseline method under comparable conditions. Compare the result with
variance and with the declared threshold or budget. Validate that the apparent
gain did not remove required work, reduce result quality, shift cost outside the
measured boundary or break functional and operational invariants.

Classify the outcome:

- **supported improvement:** the relevant metric moves beyond noise and
  correctness remains intact;
- **inconclusive:** the signal is inside noise or the comparison is not valid;
- **regression:** performance or required behavior worsens;
- **trade-off:** one relevant dimension improves while another degrades.

Do not keep added complexity solely for an inconclusive performance benefit.
Retain it only when another explicit benefit justifies its maintenance cost.
Record failed or reverted experiments when the repository has an established
performance ledger or when repeating them is otherwise likely.

## Guard against regression

Add a budget, benchmark, alert or regression check only at the cheapest stable
seam that represents the requirement. Avoid brittle microbenchmarks that do not
predict the real workload. Specify expected variability, failure meaning and
the owner or response when a guard fires.

Do not turn every observation into a permanent gate. A one-time trace may be
the right evidence for a local bottleneck; a durable check is justified when
the metric represents an ongoing requirement and can be measured reliably.

## Report evidence and limits

Return a compact performance record:

```text
Question and workload
Metric, target and environment
Baseline and variability
Bottleneck evidence
Hypothesis and intervention
Before/after result
Correctness and trade-offs
Verdict
Regression guard or follow-up
Evidence gaps
```

For diagnosis-only work, distinguish supported findings from hypotheses and
rank them by user or operational impact, amplification, likelihood and ease of
verification. For implementation work, include the exact measurement method so
another person can reproduce the claim.

## Composition boundaries

- `performance` supplies the measurement, attribution and verification method.
- A base technology skill supplies the applicable runtime and ecosystem model.
- A technology performance skill supplies profiler choices, platform metrics,
  characteristic bottlenecks and platform constraints.
- `review`, when separately selected, supplies finding and review-reporting
  methodology; this skill supplies performance evidence.
- `testing`, when separately selected, supplies broader test strategy; this
  skill owns performance workloads, budgets and regression interpretation.

These are additive responsibilities, not hidden dependencies. The skill remains
useful alone and does not automatically invoke another skill.
