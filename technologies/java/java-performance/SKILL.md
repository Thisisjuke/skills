---
name: java-performance
description: Analyze Java performance using JVM-aware measurements, allocation, garbage collection, threading, and profiling evidence.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED java-build"
---

# Java Performance

Diagnose Java performance with evidence that distinguishes application work,
JVM activity and host or container constraints. Preserve the exact runtime
contract, select the least intrusive diagnostic that can test the hypothesis,
and tune code or the JVM only after attributing the limiting mechanism.

This skill owns Java and JVM performance mechanics. It does not own generic
workload design, targets, experiment validity or regression policy; the
technology-neutral `performance` skill supplies those methods when selected.

## Establish the runtime contract

Do not compare Java results until the relevant runtime conditions are known:

- exact JDK vendor, feature and update version, VM and architecture;
- selected garbage collector and effective JVM flags;
- heap, metaspace, code cache, direct/native memory and thread limits;
- host or container CPU and memory limits, throttling and competing work;
- artifact, class path or module path, framework and relevant agents;
- workload, concurrency, data shape, warm-up, cache and steady-state phase;
- JIT compilation state and whether startup or sustained behavior matters.

Use effective runtime values rather than assuming build configuration became
the launched process. Preserve the same JDK, flags, artifact and workload for a
before/after comparison unless one of them is the factor under test. Treat an
upgrade as a new runtime baseline: collectors, compilers, defaults, diagnostics
and supported options evolve between JDKs.

## Choose evidence by symptom

Start with process, host and container signals. Confirm whether elapsed time is
CPU, waiting, I/O, throttling, paging, garbage collection or an external
dependency before narrowing to Java evidence.

Use the least intrusive evidence that can discriminate the hypotheses:

| Symptom | Useful JVM evidence | Interpretation guard |
|---|---|---|
| CPU or latency | JFR execution samples, an approved sampling profiler, per-thread CPU and repeated stacks | `RUNNABLE` does not prove CPU use; sampling attributes observed stacks, not intent. |
| Allocation or GC pressure | JFR allocations, GC logs, heap occupancy and allocation rate | High allocation is not itself a leak; correlate pauses and retained growth with the workload. |
| Suspected heap leak | Histograms over time, JFR heap statistics when justified, then a heap dump and GC-root paths | Retained growth across comparable cycles matters more than one large object count. |
| Native or process memory | RSS/container metrics, OOM message, direct buffers, threads and Native Memory Tracking when already enabled | A Java heap dump cannot explain every native, thread, metaspace or container OOM. |
| Hang or contention | Several thread dumps, JFR lock/thread events, pool and queue state | Repeated waiting may be healthy; identify the condition, owner and expected progress. |
| I/O wait | JFR file/socket events, traces and dependency timing | Event thresholds can hide short frequent operations or increase recording cost when lowered. |

Record timestamps and correlate all JVM evidence with traffic, latency,
throughput and OS/container metrics. One snapshot can generate a hypothesis;
it rarely proves a trend or a causal bottleneck.

## Use JFR and diagnostic commands deliberately

Prefer Java Flight Recorder when the target JDK supports it and its event model
can answer the question. Choose a named recording configuration, duration,
event set, thresholds, stack traces, maximum age or size, and output location.
Measure overhead for the application. More events and lower thresholds can
improve fidelity while increasing CPU, allocation or recording volume; heap
statistics may be materially more intrusive than a standard recording.

Before running `jcmd`, inspect commands supported by the target JVM and their
reported impact. Diagnostic commands require suitable process access and may
pause or perturb the JVM. In particular:

- capture repeated thread information when diagnosing progress or contention;
- use a bounded recording or dump an existing continuous recording when that
  is sufficient;
- do not call `GC.run` merely to make a graph look cleaner;
- do not take a heap dump without authorization, disk capacity and an expected
  impact window;
- do not assume a command or option exists across vendors and JDK versions.

JFR files, heap dumps, thread dumps and GC logs can expose class names, paths,
URLs, payloads, credentials, personal data or system topology. Store, transfer,
redact and delete them under the project's diagnostic-data policy. Do not send
them to a hosted analyzer unless that transfer is explicitly allowed.

## Attribute CPU and latency

For CPU saturation, separate application execution from GC, JIT compilation,
safepoints, native work and container throttling. Determine whether CPU is
distributed across useful workers or concentrated in a thread, method or lock.
Use a representative sampling interval and inspect callers as well as hot leaf
frames.

For latency, separate on-CPU time from waiting. Inspect lock contention, thread
pool or connection-pool saturation, queueing, blocking I/O and downstream time.
A wider flame-graph frame is not automatically waste: it may represent required
work proportional to traffic. Quantify the input and frequency before changing
algorithms, allocation, synchronization or caching.

Virtual threads change how Java tasks map to operating-system threads and how
thread evidence is captured. Use diagnostics supported by the selected JDK and
do not infer application concurrency from platform-thread counts alone.

## Diagnose memory and garbage collection

Classify the exhausted resource before proposing a larger heap. Distinguish at
least Java heap, metaspace or class-loader growth, direct/native memory, thread
creation, code cache and an external container or operating-system kill.

For Java heap pressure:

1. correlate allocation rate, collection causes, pause distribution and
   occupancy before and after collection with the workload;
2. determine whether the live set stabilizes under comparable conditions;
3. distinguish short-lived allocation pressure from retained growth;
4. use dominators and paths to GC roots to explain retention when a dump is
   justified;
5. verify lifecycle ownership before labeling a cache, listener, thread local,
   queue or collection as a leak.

Do not infer a leak solely from a rising sawtooth before collection, a large
histogram class or one heap snapshot. Do not infer healthy behavior solely from
short pauses: excessive collection CPU or allocation can still reduce
throughput.

GC selection and heap sizing are workload decisions. Begin with the supported
JDK's ergonomics, then evaluate alternatives against the actual latency,
throughput, CPU and memory objectives. Leave headroom for non-heap and native
memory under the process or container limit. Treat pause targets as goals, not
guarantees. Change one attributable factor at a time; copied collector flags,
fixed heap ratios and old version-specific recipes are not defaults.

## Benchmark Java code safely

Use an end-to-end or component workload when system behavior is the question.
Use JMH only when an isolated JVM operation is genuinely the unit of interest.
Run it through the project's supported build path, preferably as a standalone
benchmark artifact rather than an uncontrolled IDE loop.

For a microbenchmark, make explicit:

- warm-up, measurement iterations, forks and JVM arguments;
- benchmark state scope, setup and parameter values;
- result consumption and protection against dead-code elimination;
- protection against constant folding or work moved outside the measured path;
- whether setup, allocation, contention or I/O belongs inside the operation;
- distribution, uncertainty and the practical magnitude of the difference.

Inspect generated or profiled behavior when the result depends on compiler
optimization. A JMH score is evidence for that isolated benchmark contract; it
does not prove end-to-end latency, capacity or user impact.

## Design and verify the intervention

State the Java-specific causal chain before mutation:

```text
Under [runtime and workload], [JVM/application mechanism] produces [evidence].
Changing [code, configuration or runtime factor] should move [metric] while
preserving [behavior, compatibility and resource constraint].
```

Prefer a code or configuration change that removes the demonstrated cause over
a flag that masks it. Conversely, do not rewrite correct code when collector,
heap or container configuration is the measured constraint. Re-run comparable
evidence and check functional behavior, startup, steady state, tail latency,
throughput, CPU, allocation, memory headroom and operational complexity as
applicable.

For authorized source or non-build runtime-configuration mutation, call
`implementation`. If the change instead belongs to a build descriptor,
toolchain, packaging or launch configuration owned by the build, call
`java-build` when available and let it delegate mutation safety. If the
applicable capability is unavailable, report the missing contract; do not
reproduce it here. Do not call a target already active in the invocation chain;
report the cycle and preserve the evidence collected so far.

## Report the JVM evidence

Return a compact record:

```text
Java artifact and workload
JDK/VM, collector, effective flags and resource limits
Warm-up and measurement window
Host/container context
Diagnostic artifacts and their collection impact
Observed CPU, waiting, allocation, GC or memory mechanism
Causal confidence and evidence gaps
Intervention and before/after result
Correctness, compatibility and operational trade-offs
Reproduction command or procedure
Verdict and follow-up
```

Keep hypotheses separate from demonstrated mechanisms. Report when production
evidence could not be captured safely or when local results do not represent
the deployed runtime.

## Composition boundaries

- `performance` owns representative workloads, targets, experiment validity,
  comparison and regression policy; `java-performance` owns JVM evidence.
- `java` owns language and runtime validity; this skill owns performance
  diagnosis within that contract.
- `java-build` owns build graphs, toolchains, artifacts and encoded launch
  configuration; this skill interprets their runtime performance effects.
- `java-testing` owns Java test mechanics; this skill owns JMH and performance
  evidence rather than general test strategy.
- `observability` owns durable signal semantics and alerting; this skill may use
  those signals for a bounded investigation.
- `debugging` owns general causal defect diagnosis; this skill specializes
  attribution for performance failures.
- `java-me-performance`, when selected, overrides assumptions requiring a full
  Java SE JVM, serviceability tools or unconstrained host resources.
- `review`, when selected, owns finding format and review disposition; this
  skill supplies Java performance evidence.

These boundaries support explicit composition. The skill does not automatically
load `performance`, `java`, `review` or a platform specialization.
