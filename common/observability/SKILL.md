---
name: observability
description: Design or improve operational signals when a system's behavior, health, failures, or user impact must be understood from outside, with explicit semantics, correlation, cost, privacy, alerting, and verification.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation"
---

# Observability

Make consequential system behavior explainable from evidence available outside
the implementation. Start from the decisions operators, maintainers or product
owners must make; emit only signals that help make those decisions reliably.

This skill owns technology-neutral diagnostic questions, signal semantics,
correlation, dimensions, sampling, retention, alert intent and telemetry
verification. It does not own incident diagnosis, performance experiments,
security policy, generic testing, implementation safety or vendor-specific
instrumentation.

Observability analysis and design are read-only by default. Do not change code,
production configuration, dashboards, alerts or external telemetry systems
without explicit authorization.

## Establish the observability contract

Identify:

- the behavior, component, workflow or change in scope;
- who consumes the evidence and which decisions they must make;
- normal, degraded, failed and recovered outcomes that matter;
- service objectives, operational commitments or known failure modes;
- deployment topology, asynchronous boundaries and version coexistence;
- available telemetry systems, data policy, retention, volume and cost limits;
- whether the request is signal design, instrumentation, review or diagnosis of
  an observability gap.

Write concrete questions before selecting a signal. Useful questions connect an
observation to an action, for example:

- Is the promised outcome currently succeeding for affected consumers?
- Where is a workflow delayed, dropped, duplicated or failing?
- Which dependency, version, region or state is associated with degradation?
- Which individual execution needs investigation or recovery?
- Did a rollout change an important outcome beyond its accepted range?

If a proposed signal does not help answer a scoped question or satisfy a known
audit obligation, omit it. “Log more” is not an observability requirement.

## Model observable behavior

Trace the relevant work through inputs, state transitions, dependencies,
retries, queues, concurrency, side effects and completion. Mark the points where
an observer must distinguish:

- accepted from rejected work;
- success from partial success, retry, cancellation and terminal failure;
- expected absence or low activity from a stalled producer or broken exporter;
- user-visible delay from internal work that does not affect the objective;
- one execution from an aggregate trend;
- current behavior from a previous version or configuration.

Instrument the authoritative transition or boundary. Avoid counting the same
logical event at several layers unless each count has distinct semantics.
Telemetry must describe the product behavior, not merely that a function ran.

## Choose the smallest useful signal set

Select signal types by the question and operating environment:

- **Events or logs** preserve discrete context about what happened in one case.
- **Metrics** summarize rates, counts, states or distributions across cases.
- **Traces** connect causally related work across components or asynchronous
  boundaries.
- **Health or state snapshots** expose current readiness, backlog or progress
  when event history alone cannot distinguish silence from failure.

Not every system needs every signal. A local batch may need completion state and
failure events but no distributed trace. A constrained device may retain only
bounded local diagnostics. A high-volume service may need aggregation and
sampling before storage.

Choose averages, totals, rates, distributions or percentiles according to the
decision. Tail distributions matter for latency and skewed workloads, but an
average can still be meaningful for additive quantities or a defined aggregate.
State the aggregation and window so consumers do not compare incompatible
values.

## Define signal semantics

For every retained signal, specify:

- stable name and purpose;
- exact event, state or interval that records it;
- value type, unit and valid range;
- success, error and unknown-outcome semantics;
- dimensions or attributes and their allowed value sets;
- time semantics and handling of delayed or duplicated delivery;
- correlation or causal context when individual work must be followed;
- sampling, aggregation and retention behavior;
- schema owner and compatibility expectations;
- the question, view or alert that consumes it.

Use structured, machine-queryable fields when events need filtering or
aggregation. The encoding need not be JSON. Keep names and attributes consistent
with the project's domain language and existing semantic conventions.

Propagate correlation only across boundaries where causal reconstruction is
needed. Preserve an accepted incoming context when safe, create a new one at an
actual root, and avoid treating untrusted identifiers as authority. Request IDs,
trace context and business identifiers serve different purposes and need not
appear on every signal.

Control dimensionality before emission. A dimension with user, request, raw
URL, error text or another unbounded value can multiply storage and query cost
or make aggregation unusable. Keep bounded dimensions in aggregate signals;
put case-specific context in an appropriately protected event or trace when it
is genuinely required.

## Protect signal quality and data

Telemetry is a data product with failure modes of its own:

- missing or duplicated events can distort conclusions;
- retries can count attempts when the question concerns logical outcomes;
- clocks and delayed delivery can reorder apparent behavior;
- sampling can hide rare paths or bias a population;
- instrumentation can add latency, allocation, network or battery cost;
- exporter failure can look like healthy silence;
- schema drift can split one concept across incompatible names;
- fields can leak secrets, personal data or cross-tenant context.

Define loss, ordering and sampling expectations proportionate to the decision.
Use an allowlist for sensitive contexts and collect the least data needed.
Never make an authorization decision from telemetry merely because a field
looks like an identity or role.

When security evidence is required, let `security` define the event and data
protection objective. Observability owns how that evidence remains queryable,
bounded and operational without disclosing the protected asset.

## Design views and alerts

Build a dashboard, query or report around a scoped operational question, not
around every available series. Show enough context to distinguish user impact,
scope, trend, version and dependency contribution without requiring viewers to
mentally join unrelated panels.

Alert only when a recipient has a defined action or decision. Prefer signals of
objective or user-visible harm when they provide timely evidence. A causal or
resource signal may still justify an alert when it reliably predicts imminent
harm and an operator can act before the symptom appears.

For each alert, define:

- condition, evaluation window and missing-data behavior;
- affected objective or protected capability;
- urgency based on impact and response window;
- owner, destination and deduplication or grouping behavior;
- first diagnostic query and response guidance;
- expected false-positive and false-negative trade-off;
- safe validation method and review condition.

Derive thresholds from an objective, capacity boundary, historical behavior or
tested failure mode. Do not invent a percentage or duration because it looks
conventional. Avoid alerts that flap, duplicate another signal or are routinely
ignored.

## Verify telemetry end to end

Instrumentation is not verified merely because code emits a value. In a safe
environment or controlled fixture, exercise representative normal, degraded and
failure paths and confirm that evidence is:

- emitted once with the intended semantics;
- exported, stored and queryable within the required delay;
- correlated across the boundaries that matter;
- aggregated without unexpected dimensions or double counting;
- free of prohibited sensitive data in actual output;
- understandable from the documented question and field definitions;
- resilient to exporter failure according to the system's requirements;
- sufficient to trigger and route alerts without unsafe production testing.

Record sampling, unavailable environments and untested alert delivery as
limits. Do not claim absence of failures from absence of telemetry until signal
health and expected activity have been established.

## Apply authorized instrumentation

For authorized repository mutation, call `implementation` if it is available
and the invocation is acyclic. Pass the observability questions, signal
contracts, data constraints, cost limits and verification scenarios; do not
duplicate its mutation, dirty-tree or final-diff workflow.

If `implementation` is unavailable, report the configuration gap before
mutation. Signal analysis, design and review remain useful without that call.
Changing a live alert, retention policy, external dashboard or telemetry backend
requires separate authorization for that external state.

## Report

Return a proportionate observability design or assessment:

```text
Scope, consumers and operational questions
Observable outcomes and boundaries
Signal contracts and correlation
Data, cardinality, sampling, retention and cost constraints
Views and alert decisions
Verification performed and outcomes
Blind spots and residual operational risk
Authorized changes or next evidence needed
```

## Composition boundaries

- `debugging` owns diagnosis and root cause; `observability` owns the durable
  evidence that makes later diagnosis possible.
- `performance` owns controlled workloads, measurement validity and
  optimization; `observability` owns ongoing operational trends and signals.
- `security` owns threat, sensitive-data and security-event requirements;
  `observability` owns safe signal semantics, transport and consumption.
- `testing` owns test strategy and oracle quality; `observability` supplies the
  telemetry contracts and end-to-end signal path to verify.
- `review` owns findings and verdicts; `observability` supplies the operational
  evidence and instrumentation-quality lens.
- `api-design` owns interface behavior; `observability` owns diagnostic context
  needed across that interface without changing its business semantics.
- Technology skills own logging APIs, telemetry SDKs, exporters, platform
  limits, backend queries and deployment mechanics.
- `implementation` owns authorized mutation and final diff accountability.

Except for the conditional call to `implementation` for authorized
instrumentation, this skill does not automatically invoke another skill.
