---
name: release-management
description: Prepare, authorize, stage, observe, roll back, and close a production release using explicit readiness evidence, decision thresholds, and operational ownership.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED risk-management; CALLS WHEN NEEDED observability"
---

# Release management

Turn a deployable change into a controlled production transition. Make the
release decision, exposure stages, operating signals, rollback boundary and
ownership explicit before increasing impact.

This skill owns integrated release readiness, rollout stages, go/hold/rollback
criteria, coordination and closure. It does not own implementation, provider
mechanics, specialist assurance, monitoring implementation, or authority to
change production.

Release analysis and planning are read-only by default. A plan does not grant
permission to deploy, change flags, communicate externally or operate a live
system.

## Establish the release contract

Identify the release unit, environments, intended outcome, accepted scope,
decision owner, operator, dependencies, migrations, flags, affected users and
data, release window, stages, hold points, maximum exposure, and success,
degradation and rollback signals.

Use the established delivery procedure. Do not invent a production command or
infer credentials, environment names or approval from a development task.

## Build a readiness case

Collect evidence proportionate to blast radius:

- build, test and acceptance gates on the release artifact;
- unresolved defects and accepted exceptions with owners;
- client, schema, data and mixed-version compatibility;
- specialist security, performance, accessibility or compliance evidence when
  affected;
- operational capacity, support and dependency readiness;
- recovery prerequisites, backups and data reconciliation;
- operator and user documentation or communication.

Record each gate as passed, failed, waived by an identified authority, or
unavailable. Do not replace missing specialist evidence with a generic checklist.

When material uncertainty needs structured treatment, call `risk-management`
with stages, affected outcomes, failure scenarios, reversibility and evidence.
It owns risk analysis; this skill owns the integrated release decision. If
unavailable, expose the gap and keep the gate unresolved.

## Define observation and thresholds

For every stage, specify population or traffic, evidence duration, signals,
expected range, decision owner and next action. Distinguish outcome signals from
safety signals such as errors, latency, saturation, data integrity, support
volume and security events.

If required signals or alerts do not exist or cannot be interpreted, call
`observability` with behavior, stage boundaries, failure modes and thresholds.
It owns instrumentation and signal quality; this skill owns how signals govern
rollout. If unavailable, do not claim an unobservable stage is controlled.

Before either call, check the active invocation chain. Do not call an active
ancestor or current skill. Calls add no production, network or communication
authority.

## Plan rollback and recovery

Define the rollback trigger and decision owner, last reversible stage, code,
configuration, flag, schema and data actions, mixed-version behavior, treatment
of data written after cutover, verification after recovery and conditions for a
renewed rollout.

Use roll-forward when rollback would worsen data or compatibility, but name the
decision and evidence. Reverting a deploy is incomplete when state, messages,
caches or external consumers have changed.

## Control the rollout

At each separately authorized stage:

1. verify artifact, configuration, approvals and operator;
2. record starting state and active incidents;
3. expose only the planned population;
4. observe the agreed signals for the evidence bar;
5. choose go, hold, rollback or stop through the decision owner;
6. record evidence and deviations before proceeding.

Never silently widen exposure. Stop when evidence is missing, thresholds are
crossed, ownership is unavailable or production differs from the plan.

## Close the release

Verify intended exposure, critical paths, operational signals, migration state,
temporary controls and support ownership. Cleanup of flags or compatibility
paths needs its own authorization and lifecycle.

Report the release unit, readiness evidence and waivers, stages and thresholds,
rollback plan, authorizations and decisions, observed results, incidents,
residual risks and follow-up owners.
