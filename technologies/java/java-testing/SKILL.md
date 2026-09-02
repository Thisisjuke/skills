---
name: java-testing
description: Design, implement, run, or diagnose Java tests using the project's actual JUnit Platform, JUnit, TestNG, Mockito, assertion, integration, module, and build-runner contracts without imposing a framework migration.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED java-build"
---

# Java testing

Create trustworthy Java test evidence through the framework, runner, JVM and
classpath that the project actually uses. Preserve legacy or specialized test
stacks unless migration is explicitly in scope.

This skill owns Java test discovery and lifecycle, JVM test isolation, Java
assertion semantics, parameterized tests, doubles, extensions, integration
fixtures, module/classpath concerns and runner evidence. It does not own generic
test strategy, production behavior, build-system configuration, framework-
specific application testing or Java ME emulator/device validation.

Test execution runs repository code and may start external services, containers
or embedded runtimes. Inspect the relevant test configuration before executing
an unfamiliar suite. Network access, privileged services, persistent fixtures
and mutation of shared environments require the corresponding authority.

## Establish the Java test contract

Determine only what the task needs:

- Java version and JVM used to execute tests;
- framework and generation: JUnit Platform/Jupiter/Vintage, JUnit 4/3, TestNG,
  a framework runner or another engine;
- Maven/Gradle/other runner, test tasks or phases, filters and report locations;
- source set/module, package, module path versus classpath and test visibility;
- assertion and mocking libraries already used nearby;
- lifecycle, extensions/listeners, tags/groups and parallel-execution settings;
- integration resources such as databases, ports, files, containers or clocks;
- established naming, fixture and cleanup conventions;
- behavior and observation seam the test must prove.

Do not infer the engine from an annotation import alone. The API can compile
while the runtime engine/provider is absent, a filter can select no tests, or a
legacy engine can execute only part of a mixed suite. Inspect resolved test
runtime configuration and reports when discovery is uncertain.

Preserve JUnit 4, TestNG or another working stack unless migration is the task.
Migrate a class coherently: mixed lifecycle annotations and incompatible runner
models can compile while silently skipping setup or tests.

## Place tests at the owned seam

Follow the project's source sets and modules. Keep a test near the module that
owns the behavior, while testing through the narrowest accessible contract that
provides useful evidence.

Java visibility affects the seam:

- package-private tests can exercise package-level contracts without widening
  production visibility;
- do not make a member public solely to test a private implementation detail;
- generated code should be tested through its maintained input or public
  output unless generated sources are themselves the product;
- for Java modules, decide whether the test is intentionally white-box with
  relaxed module boundaries or black-box as a consumer;
- reflection used only to reach private members is usually a signal that the
  test is coupled to structure or the responsibility needs extraction.

Do not require one class-to-one-test-class layout. Organize around behavior and
the repository's reporting/discovery conventions.

## Use lifecycle deliberately

Understand the selected framework's test-instance and callback lifecycle before
sharing fixtures.

For JUnit Jupiter projects:

- default test instances are isolated per test method unless lifecycle is
  configured otherwise;
- per-test setup is appropriate for mutable fixtures, but constructors and
  parameter resolution may also be valid project conventions;
- class-level setup often implies static state under the default lifecycle;
  avoid using it to share mutable scenario state;
- extensions are composable lifecycle and parameter mechanisms, but adding one
  can alter every test in its scope;
- nested and parameterized tests have their own discovery and lifecycle effects;
  verify them through the actual runner.

For other frameworks, preserve their runner/listener and lifecycle semantics
rather than translating annotations mechanically.

Release files, executors, sockets, database state, system properties and other
resources even after assertion or setup failure. Prefer framework-provided
temporary directories and scoped fixtures over hard-coded paths. Shared static
state, default locale/time zone, environment assumptions and global registries
can leak between tests or forks.

## Express cases without hiding them

Use an ordinary test when one scenario has distinct setup or intent. Use a
parameterized test when the same behavior and oracle apply across meaningful
input partitions.

For parameterized tests:

- give each invocation a stable, diagnosable identity;
- keep expected values independent from the implementation under test;
- use typed method/factory sources for complex cases rather than unreadable
  encoded literals;
- avoid enormous Cartesian products without a risk-based selection model;
- verify that the runner reports each intended invocation;
- do not replace a property/invariant test with a long table of arbitrary
  examples.

A loop inside one test may be correct when the sequence itself is the behavior.
Do not mechanically convert every loop to a parameterized test.

## Assert Java values by their contract

Use the project's established assertion library. Choose assertions that expose
the behavioral difference clearly; do not add AssertJ, Hamcrest or another
library only for stylistic preference.

Pay attention to Java-specific equality and representation:

- object equality depends on the type's `equals` contract; identity assertions
  answer a different question;
- arrays require content-aware assertions rather than ordinary object equality;
- `BigDecimal.equals` includes scale while numerical comparison does not;
  select the one required by the domain contract;
- floating-point comparisons need a justified tolerance or invariant;
- collections may require order-sensitive or order-insensitive assertions based
  on their promised semantics;
- exceptions should verify the type and stable, actionable fields or cause;
  avoid pinning incidental full messages or stack traces;
- asynchronous outcomes need bounded coordination, not arbitrary sleeps;
- multiple assertions should remain focused on one coherent behavior even when
  the framework can aggregate failures.

An assertion that only checks non-null or “does not throw” is sufficient only
when that is the actual contract.

## Use doubles at Java boundaries

Prefer real value objects, collections and small deterministic collaborators.
Use Mockito or the project's existing double mechanism for a boundary whose
real implementation would make the test unsafe, slow, unavailable or
nondeterministic.

When using Mockito:

- mock dependencies, not the class whose behavior is being tested;
- stub only interactions required by the scenario;
- use strict-stubbing support when it fits the existing integration so unused
  or mismatched stubs remain visible;
- use leniency narrowly and explain the fixture reason instead of disabling
  validation globally;
- verify an interaction only when emitting that call is an observable
  obligation, not every internal step;
- keep argument matchers type-correct and consistent within an invocation;
- prefer state/result assertions over `verifyNoMoreInteractions` as a blanket
  rule;
- scope static or construction mocking tightly and restore it on every path;
- treat spies and deep stubs as design/testability warnings, not shortcuts.

Mocking final, static or constructor behavior depends on Mockito version,
mock-maker, agents and the test JVM. Inspect the project's actual setup before
assuming support. Do not add a mocking agent or weaken JVM controls solely to
avoid a small adapter or fake.

## Cross real boundaries when needed

Use an integration test when the contract depends on behavior that a mock
cannot represent reliably, such as:

- SQL dialect, schema, transaction or migration behavior;
- serialization, service loading, reflection or generated metadata;
- filesystem, process, network protocol or message-broker semantics;
- framework wiring or deployment/container integration;
- public library consumption through the produced artifact.

Use the real production type or a faithful controlled substitute. Testcontainers
can provide disposable infrastructure when the project supports its required
container runtime, but it is not a universal default. Pin intentional image and
service versions, isolate data, bound startup/teardown and report unavailable
infrastructure separately from product failure. Reuse can accelerate local
runs but may preserve state and reduce isolation; follow project policy rather
than enabling it globally.

Framework-specific slices, annotations and context caches belong to the
selected framework's capability or project conventions. Do not import Spring
Boot test advice into plain Java by default.

## Control concurrency and process state

Java test runners may parallelize methods, classes or forked JVMs independently.
Before enabling or diagnosing parallel execution, identify:

- shared static and singleton state;
- ports, filenames, schemas, queues and external accounts;
- locale, time zone, system properties and environment-sensitive behavior;
- thread pools and non-daemon threads that outlive a test;
- runner-level forks versus framework-level parallelism;
- test reports or fixtures keyed only by class/method name.

Do not “fix” concurrency failures by imposing global ordering unless ordering is
part of the contract. Allocate isolated resources or synchronize the real
shared invariant. A test timeout bounds reporting; it does not guarantee that
the underlying thread, process or remote action stopped safely.

## Run through the configured runner

Use the repository's Maven, Gradle or other test entry point so discovery,
engines, JVM arguments, agents, generated classes and reports match supported
execution. Direct IDE success is supplementary evidence.

During iteration, select the narrowest test/class/module supported by the
runner. Before completion, execute the relevant broader task or phase. Confirm:

- intended tests and parameterized invocations were discovered;
- selected engine/provider and test JVM were the expected ones;
- zero-test selection did not masquerade as success;
- skips, disabled tests, aborted assumptions and retries are visible;
- setup/teardown and infrastructure failures are not misreported as assertions;
- cached/up-to-date results are understood when fresh execution matters;
- XML/HTML or other reports agree with the command outcome.

Do not skip tests, ignore failures or add retries merely to make a build green.
Classify whether the failure is product behavior, test mechanism, discovery,
JVM/classpath/module setup or external infrastructure.

## Report Java test evidence

Use the repository's established format when present. Otherwise return:

```text
Behavior and Java observation seam
Framework, engine/provider, runner and test JVM
Source set/module and relevant classpath/module-path boundary
Fixtures, doubles and external infrastructure
Tests and parameterized cases selected/discovered/executed
Assertions and Java-specific comparison semantics
Skipped, aborted, retried or infrastructure-failed tests
Commands/tasks/phases and reports inspected
Residual runtime, integration or platform gaps
```

Do not claim that a test passed when it was undiscovered, filtered out, cached
without relevant inputs, or aborted.

## Composition boundaries

- `testing` owns generic strategy, oracle quality, seam selection,
  determinism and regression value; `java-testing` owns Java frameworks,
  lifecycle, assertions, doubles, JVM isolation and runner mechanics.
- `java` owns language/API/runtime constraints; `java-testing` applies them to
  fixtures and test execution.
- `java-build` owns test dependencies, source sets, tasks/phases, engines and
  build-runner configuration; `java-testing` owns test content and interprets
  execution evidence.
- `implementation` owns shared mutation safety for test files and production
  changes.
- `java-me-testing` owns emulator/device, keypad, RMS, network and constrained-
  profile validation; its platform constraints override ordinary Java test
  assumptions when both are selected.
- Framework technology skills own Spring, Jakarta, Android or other framework-
  specific test configuration and semantics.

For authorized test-file mutation, call `implementation` if available before
editing. When the work requires changing test dependencies, source sets,
engines or runner configuration, call `java-build` if available before that
build change. Report an unavailable capability instead of copying its workflow.
These calls must remain acyclic; this skill has no other skill call.
