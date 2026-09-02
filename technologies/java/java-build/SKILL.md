---
name: java-build
description: Inspect, configure, build, or package Java projects while preserving their Maven, Gradle, or other build contracts, dependency ownership, toolchains, module boundaries, artifacts, and verification evidence.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation"
---

# Java build

Produce or change Java build artifacts from the repository's actual build
contract. Preserve its build system, wrappers, module model, version ownership,
toolchains and publishing conventions instead of converting it to a preferred
stack.

This skill owns Java build topology, build-tool configuration, dependency and
plugin resolution, compiler/toolchain configuration, generated inputs,
packaging and artifact evidence. It does not own Java language semantics,
testing strategy, dependency security policy, generic repository mutation,
runtime performance or Java ME preverification and JAR/JAD constraints.

Build scripts and plugins execute code. Inspect relevant project instructions
and build entry points before running them, especially in an unfamiliar or
untrusted repository. Network resolution, credential use, publication,
deployment and mutation of shared or external repositories require the
corresponding authority.

## Discover the build contract

Inspect only the files needed to establish:

- build system and supported entry point: wrapper, pinned tool, CI image or
  documented command;
- root, included modules, composite builds, parent relationships and module
  selected by the task;
- project type: application, library, plugin, platform/BOM, annotation
  processor or another artifact producer;
- Java contract: JVM running the build, compiler/toolchain JDK, source level,
  class-file target or release, and deployment runtime;
- version owners: parent, dependency management, imported BOM/platform,
  property, version catalog, convention plugin, lockfile or inline declaration;
- dependency scopes/configurations and public-versus-internal exposure;
- source sets, generated sources/resources, annotation processors and task
  dependencies;
- packaging, manifest/module metadata, classifiers/variants and expected output;
- profiles, properties, repositories, credentials and CI-only behavior;
- narrowest established verification command.

Use the repository's wrapper when present and trusted. Do not introduce Maven,
Gradle or a second build system merely because this skill understands it. Keep
the existing Gradle DSL and project conventions unless migration is the task.

If descriptors, CI and runtime declarations disagree, surface the mismatch.
Do not silently pick the newest JDK, plugin or dependency version.

## Keep Java versions distinct

Treat these as separate constraints:

- the JVM capable of running Maven, Gradle or another build tool;
- the JDK selected as compiler/toolchain;
- accepted source syntax;
- emitted class-file version;
- documented Java API surface available to compilation;
- actual deployment runtime and vendor/platform constraints.

For ordinary Java SE cross-compilation, use the project's supported release
mechanism when available because it can constrain language, class-file target
and documented APIs together. Separate source and target settings do not by
themselves prevent use of newer host-JDK APIs. A build-tool toolchain selects
tools; it does not automatically prove that every runtime dependency or API is
compatible with the deployment target.

Do not change Java level as a side effect of unrelated build work. A Java
upgrade must include build-tool, plugin, processor, dependency, CI and runtime
compatibility evidence.

## Change the owning configuration

Before editing, identify the file or convention that owns the value. Change it
once at the narrowest shared level that matches the intended impact.

For Maven builds:

- distinguish aggregation from parent inheritance;
- preserve dependency scopes, optionality, type and classifier;
- use active dependency management or imported BOMs instead of scattering
  duplicate versions;
- inspect the effective model when inheritance or profiles obscure ownership;
- declare directly used dependencies explicitly even when currently available
  only through a transitive path;
- place exclusions at the dependency path that introduces the unwanted
  artifact unless the whole reactor intentionally owns the exclusion.

For Gradle builds:

- preserve Groovy/Kotlin DSL, settings, included builds and convention plugins;
- use the existing version catalog, platform, constraints or central convention
  rather than duplicating versions;
- distinguish declarable, resolvable and consumable configurations;
- expose a dependency through `api` only when consumers compile against that
  type; otherwise keep it internal through the suitable configuration;
- preserve lockfiles, verification metadata and generated wrapper files through
  their supported update workflow;
- treat wrapper and plugin upgrades as broad compatibility changes, not routine
  dependency edits.

For another build system, apply the same ownership principles without
translating its model into Maven or Gradle concepts that do not exist.

Do not hand-edit generated output when a descriptor, catalog, lock or generator
owns it. Avoid version ranges, dynamic/changing dependencies or mutable inputs
unless the project deliberately uses and controls them.

## Preserve dependency meaning

Classify a dependency by why and where it is required:

- compile-time API exposed to consumers;
- internal implementation;
- runtime-only provider;
- compile-only host/platform API;
- test-only support;
- annotation processor or code generator;
- plugin/build logic rather than application classpath.

Inspect both compile and runtime graphs when the distinction matters. A
successful compilation does not prove runtime resolution, consumer isolation or
packaging correctness. Conversely, a dependency-management entry or platform
constraint controls a version but does not necessarily add that dependency to
the project.

Do not add a direct dependency merely to silence an unexplained classpath
failure. Determine which module and configuration owns the use, why the
artifact is absent, and whether generated code, optional features or a plugin
created the reference.

Dependency vulnerability assessment belongs to `security`; this skill can
produce the resolved graph and artifact evidence it needs.

## Build with proportionate side effects

Select the narrowest established task that proves the requested build state:

- configuration/model inspection before execution when ownership is unclear;
- affected-module compilation before a full reactor build;
- packaging when artifact structure changed;
- configured verification lifecycle when the requested confidence requires it;
- clean rebuild only when stale/generated outputs are a credible confounder;
- runtime or consumer smoke check when packaging/linkage is the risk.

Do not publish, deploy, release, sign, upload or install artifacts into a shared
repository unless explicitly authorized. Treat installation into a developer's
local artifact repository as a mutation and use it only when the task needs
cross-build consumption. Avoid deleting caches as a first response to a build
failure; preserve diagnostic evidence and isolate the failing layer.

Do not bypass configured checks, dependency verification or signatures merely
to obtain a green build. If network access or credentials are unavailable,
report the unresolved inputs rather than substituting untrusted repositories or
embedding secrets in project files.

## Inspect the produced artifact

Verify the output that matters, not only the task exit code. Depending on the
artifact, inspect:

- expected filename, type, variant/classifier and module origin;
- archive entries, resources, service descriptors and manifest/module metadata;
- main class or launch metadata for an executable artifact;
- class-file target on representative classes;
- generated sources and processor outputs;
- dependency inclusion or exclusion for thin, shaded or bundled artifacts;
- published metadata as it will be seen by consumers;
- signatures, checksums or provenance only when configured and in scope.

A passing local build on a newer host does not prove the artifact runs on the
declared target. Exercise the intended runtime or a representative consumer
when compatibility is the question.

## Evaluate reproducibility honestly

Reproducibility means that equivalent declared source, environment and build
instructions produce the same specified artifact. Stable dependency versions,
toolchains, plugin versions, generated inputs, archive metadata and environment
leaks all affect that claim.

Preserve the project's locking and reproducible-archive mechanisms. If the task
requires a reproducibility claim, compare independent clean builds and state
which environmental dimensions were held or varied. Two builds in one working
directory can detect simple timestamp drift but do not prove independent
reproduction.

Do not enable broad locking or upgrade plugins solely for theoretical
reproducibility when that changes the project's dependency policy. Report the
current gap and propose the smallest separately reviewable change.

## Diagnose by build layer

Classify a failure before changing configuration:

- build tool or wrapper cannot start;
- settings/model/configuration evaluation fails;
- plugin or dependency resolution fails;
- toolchain or compiler selection is incompatible;
- source or generated-source compilation fails;
- tests or verification checks fail;
- packaging or metadata generation fails;
- artifact launches or links incorrectly;
- publication credentials or remote repository fails.

Preserve the first causal error and the exact task/module context. Do not fix a
compiler error by broad dependency upgrades, disable tests for a packaging
failure, or rewrite repositories because of a transient network symptom.

## Report build evidence

Use the project's established report format when present. Otherwise return:

```text
Build system, wrapper/tool version and invoked entry point
Build JVM, compiler/toolchain, source/release target and runtime target
Affected modules and owning configuration
Dependency/plugin changes and resolved scope/configuration
Exact tasks, profiles and relevant properties exercised
Produced artifacts and inspected metadata
Verification results and unexercised paths
Network, credential, cache or environment assumptions
Remaining compatibility or reproducibility risks
```

Do not expose secrets or dump irrelevant environment state.

## Composition boundaries

- `java` owns Java language, API and runtime compatibility; `java-build` owns
  how the build encodes and proves those constraints.
- `implementation` owns the shared repository-mutation contract; `java-build`
  supplies build-specific edits and evidence.
- `testing` and `java-testing` own test strategy and Java test mechanics;
  `java-build` executes configured test tasks only as build gates.
- `security` owns dependency and supply-chain risk decisions; `java-build` owns
  resolution, locking, verification configuration and graph evidence.
- `performance` and `java-performance` own runtime measurement; `java-build`
  owns build inputs and artifacts used by those experiments.
- `java-me-build` owns preverification, JAR/JAD consistency, profile APIs and
  constrained-device packaging; those constraints override ordinary Java build
  guidance when both are selected.

For authorized changes to build files, call `implementation` if available and
not already active in the invocation chain before mutation. Report a missing
capability or cycle instead of copying its workflow. This skill has no other
skill call.
