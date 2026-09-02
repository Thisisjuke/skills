---
name: java-review
description: Review Java code for language, API, compatibility, type-contract, exception, resource, concurrency, and stream-specific defects. Use for a focused Java review or alongside a broader review method; judge every rule against the project's actual Java version and runtime.
license: MIT
---

# Java Review

Inspect Java code for defects that arise from Java language, runtime and API
contracts. Apply only checks supported by the project's actual platform; code
that is valid for a modern Java SE release may be invalid for an older or
specialized runtime, and the reverse may also be true.

This skill provides a Java-specific review lens. It does not define a complete
generic review process, repeat all Java development guidance, prescribe build
or testing tools, or cover framework- and platform-specific rules.

## Fix the applicable Java target

Before making a Java-specific claim, establish the relevant facts from project
instructions, build configuration, manifests, CI and surrounding code:

- platform and runtime profile;
- source and class-file levels;
- APIs and libraries available at runtime;
- public, binary, serialized, reflective and generated consumers;
- concurrency, nullability, resource and framework conventions.

If these facts are missing or contradictory, report the uncertainty. Do not
assume the host JDK, the latest Java release, Java SE APIs, or a familiar
framework represents the deployment target.

## Inspect compatibility and API contracts

- Separate source, binary, runtime/API, behavioral, serialization and
  reflective compatibility. Compilation alone does not prove all of them.
- Trace changed public and protected members to real consumers. Check method
  descriptors, return and parameter meaning, declared failures, constants,
  enum values, annotations, constructors and visibility.
- Treat apparently unused members as potentially live when reflection,
  serialization, dependency injection, native integration or generated code
  can reach them.
- Check every introduced syntax feature and API against the declared source
  level and runtime profile, not only against the compiler running locally.
- For removed parameters or methods, inspect overload resolution, overridden
  and implemented declarations, method references, reflection, serialization
  and callers compiled against the previous descriptor. A parameter unused in
  one implementation may still be required by an interface, framework binding
  or binary consumer.
- Investigate raw types, unchecked casts and broad suppressions at the boundary
  where type information was lost. Do not flag a justified legacy or
  interoperability boundary merely because it cannot be fully generic.
- Verify ownership and mutability of returned or retained collections, arrays
  and buffers. A defensive copy, live view and mutable shared value have
  different caller contracts.

For a compatibility finding, identify the affected consumer and the exact
dimension that fails. Avoid vague claims that a change is simply “breaking.”

## Inspect values, equality and null contracts

- Determine whether `null` means absence, invalid input, deferred
  initialization or failure. Flag code only when producer and consumer
  contracts disagree or the ambiguity creates a reachable failure.
- Check unboxing, chained dereferences, collection lookups and nullable
  callbacks for paths that can actually produce `null`.
- Treat `Optional`, nullability annotations and empty collections as
  version-, framework- and contract-dependent tools, not mandatory rewrites.
- When `equals` is overridden, verify reflexivity, symmetry, transitivity,
  consistency and null handling, and verify that equal objects have equal hash
  codes.
- Check that fields participating in equality or hashing remain stable while
  instances are used in hash-based collections.
- Check boxed values and domain values for accidental reference comparison.
  Account for type-specific semantics such as scale-sensitive
  `BigDecimal.equals` before recommending an alternative.
- Where sorted and hashed collections or APIs interchange values, verify that
  natural or supplied ordering has the consistency the surrounding contract
  requires.

Generated equality for persistence entities, proxies and mutable models is a
review trigger, not proof of a defect. Establish identity, lifecycle and
framework behavior before reporting it.

## Inspect failures and resource lifetime

- Follow normal, exceptional, cancellation and cleanup paths from every
  acquisition to the terminal state.
- Check streams, readers, sockets, channels, database objects, locks, executors
  and framework-managed handles according to their actual ownership contract.
- Prefer automatic resource management only when the target supports it and
  the resource participates in that mechanism. Otherwise verify explicit
  cleanup on every path.
- Ensure cleanup failures do not leave internal state falsely open, skip
  required notification, or hide the primary failure without an intentional
  policy.
- Flag empty catches, catch-and-continue and log-and-rethrow only when they lose
  a required signal, permit invalid continuation, or duplicate handling.
- Treat broad catches as valid at genuine isolation or lifecycle boundaries;
  verify that their protected scope is narrow and their recovery behavior is
  deliberate.
- When the platform defines interruption as cancellation, check that catching
  `InterruptedException` does not silently turn cancellation into success.
  Restore status, propagate, or terminate according to the surrounding
  contract; do not prescribe a Java SE pattern to a runtime with different
  semantics.

## Inspect concurrency from invariants

Identify each mutable state invariant, its owner and every thread or execution
context that can observe it. Then verify:

- safe publication and visibility between writers and readers;
- atomicity of the complete check/read/modify/write sequence;
- consistency across multiple fields, collections or registries;
- correct monitor or lock ownership, ordering and release;
- wait conditions guarded by loops and paired with the notifications that can
  make those conditions true;
- cancellation, shutdown and failure propagation for threads, tasks and
  executors;
- cleanup of per-thread state when execution contexts are reused;
- absence of unknown callbacks or blocking operations under a lock unless the
  risk is intentionally controlled.

Do not infer safety from a `volatile` field, atomic primitive or concurrent
collection alone. Each protects only specific operations; compound invariants
may still race. Conversely, do not demand synchronization for state that is
immutable, confined, or otherwise proven not to be shared.

Apply guidance about virtual threads, concurrent utilities and other
version-specific mechanisms only when those APIs and semantics exist on the
target runtime.

## Inspect streams and functional pipelines when available

- A stream is normally single-use; look for reused or already-consumed
  pipelines.
- Behavioral parameters should satisfy the API's non-interference and
  statelessness requirements. Trace mutations of the source or shared external
  state rather than rejecting all side effects categorically.
- Do not rely on an intermediate operation being invoked solely for its side
  effect; pipeline optimization and short-circuiting may elide work.
- Check encounter-order assumptions and mutable reductions when execution may
  be parallel.
- Require evidence before claiming a parallel stream improves performance;
  also inspect common-pool contention, blocking work and thread-safety.
- Close streams backed by I/O resources according to their source contract;
  collection- and array-backed streams generally do not need closing.

Skip this section when the target has no Stream API. Never propose streams,
lambdas or modern collection factories merely to modernize compatible legacy
code.

## Screen Java-specific security surfaces

During an ordinary Java review, identify credible exposure involving native
deserialization, reflection or class loading, dynamic expression execution,
process creation, path handling, secrets, unsafe temporary resources, or
untrusted input passed into privileged APIs.

Report a concrete Java defect when the execution path and impact are supported
by evidence. For broader threat modeling, cryptography, authorization,
dependency vulnerabilities or framework-specific security, request a focused
security review instead of expanding this skill into one.

## Verify and report Java evidence

Use the narrowest repository-supported checks that can prove or disprove the
suspected issue. Depending on the finding, useful evidence includes:

- compiler diagnostics against the declared source, class and API target;
- a minimal reproducer for equality, null, exception or stream behavior;
- a failing test that exercises the relevant caller and failure path;
- a documented JLS, JVM or API contract applicable to the target;
- a concurrency test plus reasoning about the violated invariant or
  happens-before relationship;
- inspection of generated code, bytecode or runtime linkage when source alone
  is insufficient.

When used alone, report each material issue with its location, the Java
contract involved, the applicable target, the reachable consequence, evidence
and the smallest sound correction direction. Distinguish a compiler failure, a
demonstrated runtime defect and a risk requiring further proof.

## Avoid mechanical findings

Do not report an issue solely because code:

- uses an older but supported Java idiom;
- contains `null`, synchronization, a checked exception, a broad catch or a raw
  type with a justified boundary contract;
- does not use records, streams, `Optional`, virtual threads or the latest JDK;
- differs from personal style where project tooling and conventions accept it;
- could be rewritten more elegantly without a concrete correctness,
  compatibility, safety or maintenance consequence.
