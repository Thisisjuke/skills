---
name: java-me-performance
description: Analyze Java ME performance under strict heap, CPU, allocation, rendering, storage, network, and device limits.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED java-me-build; CALLS WHEN NEEDED java-me-testing"
---

# Java ME Performance

Diagnose and improve Java ME performance against the limits that affect the
declared devices: heap headroom, allocation and collection pressure, CPU or
interpreter progress, interaction and frame latency, startup, storage, network,
media and artifact constraints. Keep every conclusion bound to the exact suite,
runtime, workload and evidence lane that produced it.

This skill owns Java ME performance mechanics and interpretation. It does not
own technology-neutral experiment design, ordinary JVM serviceability, Java ME
platform validity, static suite packaging or general emulator automation.

## Establish the constrained target

Start from the user-visible or system outcome, then record the target facts
that can change its cost:

- configuration, profile, optional APIs and source/class-file level;
- exact JAR/JAD or diagnostic suite and its relevant variant;
- runtime or emulator implementation and whether it uses a JIT;
- physical device model, firmware and Java runtime when device behavior matters;
- configured and observed heap, persistent storage and artifact limits;
- display size, rendering mode, input path, network and media implementation;
- clean, warm, persisted, upgraded and failure-recovery state;
- workload route, input bounds, interaction sequence and background activity;
- metric, existing product budget and correctness oracle.

Do not import heap, JAR-size, frame-rate or method-size thresholds from another
project. A project-owned device floor or interaction budget is a contract; an
emulator slider, anecdote or reference-codebase limit is only a hypothesis.

## Match the judge to the cost

Select evidence from the mechanism being changed:

| Evidence lane | Useful for | Not authoritative for |
|---|---|---|
| Host JVM | Correctness or profiling hints for shared pure code | Java ME timing, allocation, collection, class loading or device headroom |
| Target-compatible VM | Runtime-specific A/B when it executes the affected path under a known interpreter/JIT contract | Other VMs, display, radio, media or physical-device limits |
| Emulator | Finding hot paths and exercising configured UI, storage, permission, network or media scenarios | Exact handset timing, heap, GC, radio, graphics or audio latency |
| Physical device | End-user latency, responsiveness, heap survival, rendering, media and radio behavior on that exact target | Untested devices, firmware or runtime implementations |

An emulator CPU throttle or heap cap is a pressure test, not proof that it
reproduces a handset. QEMU or another translated host runtime is timing evidence
only when the measured question explicitly includes that translation layer.
Desktop JIT results can generate a hypothesis but cannot accept an optimization
for a no-JIT target.

Use a target VM as an authoritative A/B only when the affected code runs there
with a runtime contract representative of the declared target. Use a physical
device for costs implemented at handset boundaries such as LCDUI/Canvas
presentation, MMAPI, real RMS, radio, keypad response and actual heap behavior.
Keep a result provisional when its required lane is unavailable.

## Measure without hiding the signal

Use the narrowest instrumentation that can distinguish the hypothesis. Keep
setup, fixture loading, correctness checking and diagnostic output outside the
timed region unless they are part of the user path. Preserve an untimed oracle
or replay so a faster result cannot silently skip work.

Account for device timer resolution and run-to-run noise. Prefer repeated,
interleaved baseline/candidate pairs on unchanged artifacts and state when the
effect is small. Reject samples below timer resolution and avoid adding
percentage gains from independently measured patches. A counter, instruction
count or synthetic kernel explains a mechanism; it does not replace a
representative route.

Profilers, memory monitors, tracing and network monitors alter execution. Record
which monitors were enabled and separate instrumented diagnosis from clean
timing. If an emulator profiler finds a hot path, verify the performance verdict
on the lane that owns the cost.

## Diagnose heap and allocation pressure

Treat Java ME memory readings as implementation-dependent observations.
`Runtime.freeMemory()` is approximate, `totalMemory()` may vary, and object size
depends on the VM. A requested collection is a best effort, not a portable way
to create identical baselines or prove that retained state is a leak.

Measure the relevant lifecycle, not one convenient snapshot:

- startup and first useful screen;
- repeated open/use/close cycles;
- pause, resume and relaunch;
- bounded growth with history, payload, image, cache or queue size;
- recovery after failed allocation, full storage or interrupted work;
- steady-state headroom on representative devices.

Separate live state, temporary peak allocation, repeated churn and native or
device resources. Use emulator allocation traces to locate candidates, then
confirm survival and headroom where the target claim requires it. Do not infer
device heap capacity from compressed JAR size or emulator memory graphs.

Prefer eliminating unnecessary peak lifetime or work before introducing pools
or caches. Reuse buffers and objects only with explicit ownership, reset and
maximum-size rules; otherwise reuse can retain the largest payload, alias
mutable state or create a leak. Stream or chunk only when it preserves protocol,
storage and rendering correctness and has a bounded no-progress path.

Do not catch `OutOfMemoryError` as routine capacity control unless the target
contract deliberately defines and tests a narrow recovery boundary. Avoid
allocation-to-failure probes on shared devices or valuable state without
explicit authorization and a recovery plan.

## Diagnose CPU, frames and responsiveness

Measure the complete latency boundary that matters: input-to-visible response,
frame update, startup-to-useful-state, operation completion or background
progress. Split it into computation, allocation/collection, rendering, storage,
network, media and waits only after the end-to-end symptom is reproduced.

For Canvas or game loops, inspect:

- work and allocation performed per frame or input event;
- worst-case routes rather than idle or average frames alone;
- repaint, flush and display-call boundaries;
- large single-frame work that blocks interaction;
- bounded progress when a task spans several frames;
- frame pacing and dropped, delayed or repeated input;
- correctness of the final framebuffer or semantic state.

An instruction or operation budget is useful when it bounds progress and has a
defined failure or yield behavior. It is not automatically a time budget across
VMs. Do not make a workload appear responsive by dropping required updates,
reducing result quality or moving unbounded work into another frame.

For MIDlet startup, include class/resource loading, persisted-state validation,
UI construction and required initialization. Distinguish cold install, ordinary
relaunch and resumed state. Lazy work can improve first screen latency while
moving a stall to first use or increasing retained memory; measure both sides.

## Diagnose RMS, network and media costs

For RMS, measure operations against representative record counts and sizes,
including open, scan, update, compaction or migration paths where applicable.
Track application payload separately from implementation overhead. Available
store space is not necessarily equivalent to additional application data, and
emulator storage timing does not establish flash behavior on a device.

For GCF traffic, distinguish connection setup, permission prompts, DNS, radio or
TLS negotiation, transfer, parsing and UI publication. Measure bytes, requests,
round trips, retries and bounded progress as well as elapsed time. A fast
localhost endpoint does not represent handset radio or production latency.

For media, separate decode or synthesis work, buffering, API-call latency and
physical playback. Emulator output can identify functional paths; audible
latency and fidelity remain implementation/device claims. External services,
billable traffic, account use and device mutation require explicit authority
immediately before execution.

## Evaluate an optimization as a constrained trade-off

Before mutation, state the platform-specific mechanism:

```text
On [artifact, runtime/device and route], [mechanism] consumes [measured cost].
Changing [one factor] should improve [metric] while preserving [correctness,
heap, artifact, compatibility and interaction constraints].
```

First compare one attributable mechanism. Preserve target API, source and
class-file compatibility, preverification, exact behavior, exception and
recovery paths, storage compatibility and device variants. Record deltas in
heap peak/headroom, persistent state, class or JAR size and complexity even
when time improves.

Keep a durable candidate entry when the project has a performance ledger or a
failed experiment is likely to be repeated. Include hypothesis, target identity,
commands, workload, correctness evidence, measurements, trade-offs, verdict and
conditions for reconsideration. Remove rejected speculative code unless it has
another explicit value.

For authorized source mutation, call `implementation` if it is not already
active in the invocation chain. If the change belongs to preprocessing,
dependencies, packaging, preverification or an encoded launch/benchmark
artifact, call `java-me-build` instead and let it delegate mutation safety. When
target-VM, emulator or device execution mechanics are needed, call
`java-me-testing` if available; it owns the session and artifact evidence while
this skill owns metrics and verdicts. Do not call a target already active in the
invocation chain; report a cycle and preserve the evidence collected so far.

## Report bounded performance evidence

Return a compact record:

```text
Outcome, workload and existing budget
Exact artifact, runtime/emulator or device/firmware
State, limits and instrumentation
Metric definition, timer resolution and sample design
Baseline, variability and bottleneck evidence
Candidate mechanism and correctness oracle
Before/after timing, heap, allocation, storage and artifact deltas
Compatibility, quality and recovery trade-offs
Authoritative, provisional, inconclusive or regressed verdict
Unproved devices, lanes and next evidence
```

Never report “Java ME optimized,” “low-memory safe” or “device-fast” from a host
benchmark, one emulator profile or one unrecorded handset run.

## Composition boundaries

- `performance` owns workload choice, causal experiment design, comparison and
  regression policy; `java-me-performance` owns Java ME metrics and judges.
- `java-performance` owns full-JVM serviceability and JIT/GC evidence; this
  skill overrides assumptions requiring Java SE diagnostics or host resources.
- `java-me` owns platform validity and bounded API behavior; this skill measures
  costs within that contract.
- `java-me-testing` owns target-VM, emulator and device execution mechanics;
  this skill owns performance instrumentation, comparison and interpretation.
- `java-me-build` owns static JAR/JAD and artifact budgets; this skill records
  their deltas and binds measurements to exact artifacts.
- `testing` owns general behavioral proof; performance oracles here only guard
  measured work against being removed or changed.
- `observability` owns durable production signals; this skill uses bounded
  runtime evidence without assuming server-grade telemetry exists.
- `review` owns review findings when selected; this skill supplies measured
  Java ME performance evidence.

The skill remains useful alone. It does not automatically call `performance`,
`java`, `java-me`, `java-performance`, review or observability capabilities.
