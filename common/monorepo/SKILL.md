---
name: monorepo
description: Analyze, navigate, change, or verify work in a repository containing multiple packages, modules, applications, services, or build units across languages and workspace tools while preserving boundaries, dependency and task graphs, scoped commands, shared configuration, and affected-consumer evidence.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation"
---

# Monorepo

Work from the repository's declared topology rather than from a preferred
directory layout or tool. This skill owns workspace discovery, package and task
graph reasoning, command scope, cross-package impact and layered verification.
It does not own language-specific build mechanics, test design, generic
mutation safety, release policy, deployment or the decision to adopt a
monorepo.

Manifests, build logic, generators and package lifecycle hooks may execute
code. Inspect unfamiliar entry points before running them. Network access,
credential use, dependency installation, publication, deployment, cache
deletion and changes outside the requested repository require their normal
authority; the presence of a workspace tool grants none of them.

## Establish the workspace contract

Read applicable repository instructions and inspect the working tree before
interpreting the layout. Locate the repository root, nested repositories or
submodules, workspace or module declarations, package manifests, lockfiles,
wrappers and tool-version pins, build and task configuration, CI entry points,
generated-file policy and ownership boundaries.

Do not infer a monorepo merely from directories named `apps`, `packages`,
`modules` or `services`. Treat a unit as real when a manifest, workspace
declaration, build include, deployable artifact, publishing boundary or
maintained repository contract establishes it. Exclude vendored dependencies,
fixtures, examples, generated output and caches unless the repository
explicitly maintains one as a first-class unit.

Build a compact scope map for the task:

```text
Unit name and path
Kind: application | library | service | tooling | root | other
Owning manifest and workspace/build system
Command working directory and selection syntax
Produced or consumed artifact
Direct dependencies and known dependents
Release or deployment boundary when relevant
Applicable local instructions and owner
```

The repository root may coordinate units without itself being a buildable or
publishable unit. When several ecosystems or nested workspaces overlap, map
each tool's root and membership separately; do not force them into one
fictional workspace.

Completion condition: the exact target unit, controlling root, native entry
point and uncertain boundaries are explicit.

## Keep the graphs distinct

Construct the package or project graph from declared dependencies and build
membership. Construct the task graph from task definitions, lifecycle rules
and artifact prerequisites. A task dependency may order work, but it does not
replace a missing package dependency; an import, path alias or accidentally
hoisted library may reveal graph drift, but it does not make that drift valid.

For each relevant edge, identify its meaning: production, compile, runtime,
development, test, plugin, optional, peer, generated artifact or toolchain.
Preserve the ecosystem's distinctions. Detect cycles and surface where they
make ordering, initialization, testing or publishing ambiguous rather than
assuming a runner can schedule them safely.

Derive the affected cone from:

- units directly changed by the task;
- their transitive dependents when contracts or artifacts can change;
- dependencies whose generated outputs or public surfaces the target needs;
- shared configuration, lockfile, toolchain, schema, generator or root-task
  changes whose reach is broader than one package;
- release and deployment units that bundle or expose the result.

Distinguish an observed graph from an intended architecture. When the two
differ, report the undeclared or forbidden edge before proposing a fix.
Boundary constraints are most useful when enforced by the repository's build,
lint or conformance tooling rather than documented only as folder conventions.

Completion condition: both graphs and the resulting affected cone explain why
each selected unit or task is in scope.

## Select commands deliberately

Use the checked-in wrapper, pinned tool and existing lockfile. Do not introduce
another orchestrator or package manager merely to obtain filtering or caching.
Before execution, resolve:

- whether the command must run from the repository root or a unit directory;
- whether the root itself is included by default;
- exact target selection by stable name or path;
- whether dependencies, dependents, both or neither are included;
- topological ordering, concurrency and behavior when no target matches;
- profiles, environment, credentials and generated inputs;
- whether the command is read-only, writes local artifacts, changes manifests
  or reaches an external system.

Prefer a native list, graph, query, dry-run or verbose mode before a broad or
costly command. In automation, make an unexpectedly empty selection fail when
the tool supports it; a successful exit with zero matched units is not useful
verification. Use parallel execution only when dependency and resource
constraints show the tasks are independent. Persistent development processes
need lifecycle handling and are not ordinary finite graph nodes.

Run the narrowest command that can produce a trustworthy signal, but widen it
when shared configuration or consumer impact makes package-local evidence
insufficient. Never replace a required repository gate with a filtered run
whose base revision, graph or selector is unverified.

## Change the owning layer

Declare an internal dependency in the consuming unit through the workspace or
build system's supported project-dependency mechanism. Do not rely on root
hoisting, a shared classpath, relative traversal into another unit's source or
an editor-only alias. Exercise the packed, linked or built consumer seam that
real users receive when it differs from workspace source resolution.

Centralize only policy that is genuinely shared: tool versions, repositories,
base configuration, dependency catalogs, constraints or common task defaults.
Keep unit-specific runtime, artifact, framework and deployment behavior with
its owner, using explicit inheritance or narrow overrides when the ecosystem
supports them. Change a version, plugin or setting at its canonical owner once
instead of scattering equivalent edits across members.

Treat lockfiles, generated sources, API clients and aggregate metadata as
owned outputs. Regenerate them through their native producer and account for
all resulting package changes. For a cross-package contract change, update the
producer and every in-scope consumer coherently; partial compatibility or a
staged migration needs an explicit transition contract.

When an authorized task requires source, manifest, build configuration or test
mutation, call `implementation` after the scope map and affected cone are
stable. Pass the target and affected units, graph edges, owning files, dirty
worktree evidence, preserved boundaries, intended commands and acceptance
signals. This skill retains topology and impact decisions; `implementation`
exclusively owns mutation safety and final diff accounting. If unavailable,
return the change brief and stop before mutation.

Do not call `implementation` when it is already active or is an active
ancestor; report the cycle and preserve the analysis. The call does not grant
installation, network, publication, deployment or destructive authority.

## Make caches and changed selection truthful

Cache only finite, deterministic tasks whose observable outputs are reusable.
Model every result-affecting source file, dependency artifact, lockfile,
configuration file, tool version, argument and environment value, and model
every output that must be restored. Disable caching for deployments, mutable
external operations, watchers, servers and tasks whose hidden inputs cannot be
made reliable.

Do not confuse dependency-download caches with task-result caches. Test a task
uncached, then test a warm hit and an intentional invalidation of a material
input. A cache hit that restores no required artifact or survives a
result-changing input is a correctness failure, not a performance success.
Keep secrets out of logs and cache artifacts; if a secret changes output, its
effect still needs a safe fingerprinting strategy.

Changed-project selection depends on a correct comparison base and sufficient
version-control history. Validate the base revision, merge-base semantics,
renames, ignored files and treatment of shared root files. If CI lacks the
needed history or the selector cannot represent a global change, fetch or
provide the missing evidence when authorized, otherwise widen the run and
report why.

## Diagnose at the failing seam

Reproduce with the same root, unit, tool versions, selector, environment and
cache mode as the failure. Determine whether the defect is in membership,
dependency declaration, task ordering, working directory, configuration
inheritance, artifact consumption, changed selection or cache invalidation.
Use graph and execution explanations before clearing state.

Do not fix a package-local symptom by granting every unit access to a root
dependency, flattening all configuration, disabling boundary checks, forcing
unbounded rebuilds or deleting caches. Those actions hide the failing seam and
often create environment-dependent behavior.

## Verify in layers

Match validation to the affected cone:

1. validate workspace membership, manifests, dependency constraints and task
   configuration;
2. run the target unit's narrow checks from the correct working directory;
3. build or prepare required dependency artifacts through the graph;
4. exercise affected dependents when an API, schema, type, artifact or runtime
   contract changed;
5. run root or repository-wide gates for shared configuration, lockfile,
   toolchain or global-policy changes;
6. inspect packed, assembled, containerized or deployed-unit artifacts when
   local workspace linking can mask consumer defects.

Read exit codes, selected-unit counts, skips, cache hits and restored outputs.
Repeat in a clean or isolated environment only when stale outputs, implicit
host tools or undeclared dependencies are credible confounders.

Report the workspace and unit map, package and task graph decisions, affected
cone, exact commands and working directories, cache state, observed results,
changed files, checks not run and residual uncertainty. Do not claim the whole
monorepo is verified from one green package-local command.
