---
name: java
description: Apply Java language, runtime, API, compatibility, resource, concurrency, and ecosystem constraints when designing, implementing, debugging, or analyzing Java code. Use across Java versions and runtimes; preserve the project's actual target instead of assuming modern Java SE or a framework.
license: MIT
---

# Java

Make Java decisions from the project's declared language, compiler, runtime,
API and compatibility contracts. Do not modernize the project or introduce a
framework merely because a newer or more familiar option exists.

This skill owns Java semantics and constraints. It does not define a generic
review workflow, build-tool mechanics, testing strategy, performance
methodology, framework conventions, or the restrictions of a specialized Java
platform.

## Establish the Java contract

Before changing or judging code, determine only the facts relevant to the task:

1. **Runtime and platform:** Java SE/JDK, Android, a constrained profile, an
   application server, a framework-managed runtime, or another implementation.
2. **Language level:** accepted source syntax and whether preview features are
   enabled.
3. **Class-file target:** the bytecode level that produced artifacts must use.
4. **Available APIs:** the platform and library APIs present on the actual
   runtime, not merely on the compiler's host JDK.
5. **Consumers:** same module, other repository modules, external binaries,
   serialized data, reflection, native integration, or framework-generated
   callers.
6. **Local conventions:** package boundaries, nullability, mutability, error
   handling, concurrency model, annotation processing and generated sources.

Derive these from repository instructions, build configuration, CI, runtime
images, manifests and nearby code. If they disagree, surface the mismatch; do
not silently choose the newest value.

Hard runtime or platform constraints override general Java preferences. Use a
newer construct or API only when the declared source level, target runtime and
toolchain all support it.

## Keep compatibility dimensions separate

“It compiles” proves only one part of compatibility. Consider the dimensions
that apply to the change:

- **source:** existing source consumers still compile;
- **binary:** already compiled consumers still link and execute;
- **runtime/API:** every referenced class and method exists on the deployment
  platform;
- **behavioral:** ordering, errors, side effects, defaults and timing still
  satisfy callers;
- **serialization and persistence:** names, identifiers and representations
  remain readable across versions;
- **reflective or framework:** annotations, constructors, visibility and member
  names still satisfy runtime discovery;
- **tooling:** annotation processors, code generators and static analyzers
  support the selected JDK.

For Java SE targets, prefer a compiler release mechanism that checks language,
bytecode and the documented API surface together when the project toolchain
supports it. Do not treat separate source and target flags as proof that older
runtime APIs were respected.

Preview features, JDK-internal packages and implementation-specific behavior
are opt-in compatibility risks. Use them only when the project explicitly owns
that constraint and its deployment environment supports it.

## Preserve API and type contracts

- Classify changed members as private, package-internal, protected or public,
  and identify real consumers before changing their shape.
- For stable APIs, prefer evolution that preserves source, binary and behavioral
  compatibility unless a breaking change is explicitly intended.
- Do not change a parameter, return value, exception or enum constant's meaning
  while leaving an apparently compatible signature.
- Preserve generic type information. Do not use raw types, unchecked casts or
  broad suppression merely to silence a mismatch; make the boundary explicit.
- State nullability through the project's established mechanism. Do not replace
  an ambiguous contract with an arbitrary `null`, empty value or exception.
- Define collection and array ownership: whether values are mutable, copied,
  retained, exposed or safe to iterate concurrently.
- Keep `equals` and `hashCode` consistent, and ensure equality remains stable
  while an object is used as a hash key. Keep ordering consistent with equality
  when the surrounding API relies on that relationship.
- Treat records, sealed types, default interface methods and other
  version-specific constructs as compatibility choices, not universal
  improvements.

When reflection, serialization or a framework participates, apparently unused
constructors, fields and methods may be externally consumed. Verify before
removing or narrowing them.

## Handle failures and resources deliberately

- Preserve original causes and add context at the boundary that can act on the
  failure. Avoid repeated log-and-rethrow chains.
- Choose checked or unchecked exceptions from the caller's recovery contract,
  not from a blanket preference.
- Catch broad `Exception` or `Throwable` only at a genuine isolation or
  lifecycle boundary; keep the protected scope narrow.
- When interruption represents cancellation, propagate it or restore the
  interrupted status after cleanup. Do not accidentally convert cancellation
  into ordinary success or an unrelated failure.
- Define resource ownership at acquisition. Release files, streams, sockets,
  locks, executors and framework resources on every completion and failure
  path.
- Use automatic resource management only when the target supports it. If close
  can fail, preserve the primary failure and make the resource's terminal state
  unambiguous before propagating the close failure.
- Make retries bounded and safe for the operation's side effects; Java exception
  handling does not make a non-idempotent operation retryable.

## Treat concurrency as a state model

- Identify mutable state, its owner and every execution context that accesses
  it.
- Establish visibility and safe publication, not just mutual exclusion.
- Keep multi-field invariants atomic across the complete read/modify/write
  operation.
- Do not expose mutable internals or iterate collections concurrently unless
  their contract permits it.
- Define task, thread, executor and callback lifecycles, including shutdown,
  cancellation, queue bounds and failure propagation.
- Avoid holding locks across unknown callbacks, blocking I/O or code that may
  acquire other locks unless the ordering is explicit.
- Select platform threads, virtual threads, reactive execution or
  platform-specific mechanisms from the actual runtime and workload. None is a
  universal default.

Where the target provides them, `volatile`, atomic classes, concurrent
collections and synchronization solve different problems. Choose among the
available mechanisms from the invariant being protected rather than treating
them as interchangeable thread-safety labels.

## Respect project and ecosystem boundaries

- Preserve the existing framework, dependency-injection style, logging facade,
  persistence approach and annotation tools unless changing them is the task.
- Do not add Lombok, a logging facade, a collection library or another
  dependency to avoid a small amount of ordinary Java code without project
  evidence.
- Edit generator inputs, schemas or annotation-processor sources instead of
  generated output, unless generated artifacts are explicitly maintained.
- In multi-module repositories, identify which module owns the contract and
  which modules consume it before moving types or widening dependencies.
- Keep framework-specific annotations and lifecycle assumptions at framework
  boundaries; do not leak them into a framework-neutral core without a concrete
  reason.

## Verify against the real target

Use the repository's supported commands and the narrowest evidence appropriate
to the task. Depending on the change, verify:

- compilation with the declared language and class-file target;
- linkage and execution on the actual runtime profile;
- affected callers and public API compatibility;
- failure, cleanup, interruption and concurrency paths;
- annotation processing and generated artifacts;
- tests under the relevant runtime rather than only an IDE or newer host JDK.

Report the compiler and runtime actually exercised. A successful build on a
newer host does not prove that an older or specialized target can load the
classes or provide the referenced APIs.
