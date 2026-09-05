---
name: vite-plus-monorepo
description: Configure, migrate, run, cache, or diagnose Vite+ in a multi-package or multi-application repository, including root and package configs, package targeting, workspace task graphs, filters, automatic data tracking, package-manager integration, and CI behavior.
license: MIT
metadata:
  skill-calls: "ALWAYS CALLS monorepo; CALLS WHEN NEEDED implementation"
---

# Vite+ monorepo

Apply Vite+'s unified command and configuration model without losing the
repository's package boundaries. This skill owns Vite+-specific root and local
configuration, command targeting, `vp run` task semantics, task-cache behavior,
package-manager integration and monorepo migration. It does not own the
workspace topology, generic mutation safety, application behavior, test
strategy, release approval or deployment.

Vite+ is evolving quickly. Inspect the locally selected `vite-plus` version,
lockfile and `vp --version` or local help before relying on the current
[Monorepo](https://viteplus.dev/guide/monorepo),
[Run](https://viteplus.dev/guide/run),
[Task Caching](https://viteplus.dev/guide/cache) or
[Migration](https://viteplus.dev/guide/migrate) guides. Treat documentation
from the moving main branch as newer than the installed project until version
evidence shows otherwise. Do not silently upgrade Vite+, its bundled toolchain,
Node.js or the package manager to make an example apply.

Vite+ commands can execute package scripts, plugins, config code, generators
and dependency lifecycle hooks. Inspect unfamiliar inputs first. Installation,
runtime downloads, migration, hook setup, network access, credentials,
publication, deployment and cache deletion retain their normal authorization
boundaries.

## Resolve the workspace and Vite+ contract

Inspect the root and member `package.json` files, workspace declaration,
lockfile, `packageManager` or `devEngines.packageManager` declarations,
package-manager configuration, root and package `vite.config.*` files, related
Vite or Vitest configs, CI workflows, Node version pins and current worktree.
Identify whether Vite+ is local, global or both and which executable and
version the intended command will resolve.

Always call `monorepo` after this initial evidence is available and before
making Vite+ configuration, migration or verification decisions. Pass the
workspace roots and members, manifests, package-manager and lockfile evidence,
Vite+ configs and version, requested commands, candidate target packages and
known dirty files. Request analysis only. `monorepo` exclusively owns the unit
map, package/task graph distinction, affected cone and general command scope;
this skill retains Vite+-specific mechanics. If `monorepo` is unavailable,
stop the affected workflow and return the collected evidence rather than
inventing a topology.

Do not call a target already active in the invocation chain or an ancestor.
Report the cycle and preserve completed analysis. Skill calls never expand
network, installation, migration or mutation authority.

Reconcile the returned topology with Vite+'s package discovery. If the package
manager, workspace declarations, lockfile and Vite+ disagree about the root or
members, surface the discrepancy before running recursive or filtered work.
Use Vite+'s environment or package-manager diagnostics and the underlying
manager's native inspection when needed; do not accept the fallback manager as
an intentional project choice when the repository forgot to declare one.

Completion condition: workspace root, selected `vp`, Vite+ version, package
manager, target packages and owning configs are explicit.

## Place configuration at the right level

Use a root `vite.config.ts` for genuinely shared Vite+ policy such as lint,
format, staged checks and task definitions. Use narrow package-path overrides
for package-specific policy. Root lint and format globs resolve from the root
config, so match workspace paths rather than paths relative to each package.
Check current merging semantics before spreading or composing override
objects; lint plugin lists and formatter option objects do not necessarily
merge identically.

Keep package `vite.config.*` files when applications, frameworks, tests,
runtime plugins, dev servers, builds or library packaging differ. Compose
plain typed configuration modules when teams need local ownership, but keep
one visible owner for each setting. Do not create a single root config whose
conditionals obscure which package receives which runtime behavior.

Package-specific commands belong in that package's scripts when their
implementations differ. Put a task in `vite.config.ts` when it needs Vite Task
dependencies, caching or explicit input, output and environment behavior. A
task name cannot simultaneously be owned by a Vite+ task and a
`package.json` script in the same resolution scope.

## Target apps without working-directory surprises

Built-in `vp dev`, `vp build`, `vp preview` and `vp pack` act on one target
application or package. At a workspace root Vite+ may select the sole eligible
package or open an interactive picker, but automation must not depend on that
interaction. Use an explicit package working directory in scripts and CI:

```bash
vp -C apps/web build
vp -C packages/ui pack
```

`-C <dir>` changes the command's working directory. A positional directory,
such as `vp dev apps/web`, keeps upstream Vite root semantics without changing
`process.cwd()`. Prefer `-C` when config, plugins, environment files or relative
paths should behave as if the command were launched inside the package.

Use root `defaultPackage` only when the repository has an intentional stable
default for bare app commands. Keep its values as static relative string
literals because Vite+ reads them without executing the config; use the object
form when different commands intentionally target different packages. An
explicit `-C` remains clearer in CI and multi-app automation.

Distinguish built-ins from scripts. `vp test` or `vp dev` invokes the Vite+
built-in; `vp run test` or `vp run dev` invokes the matching package script or
configured task. Preserve framework-specific scripts by calling them through
`vp run` instead of assuming the similarly named built-in is equivalent.

## Model workspace tasks through declared dependencies

Vite+ derives workspace ordering from internal dependencies declared in member
`package.json` files. Repair missing dependency edges at their owning
manifest; do not compensate with an arbitrary task sequence.

Choose `vp run` selection deliberately:

- no selection flag targets the package at the current working directory;
- `package#task` addresses one package task explicitly;
- `-r` runs matching tasks recursively in dependency order;
- `-t package#task` includes that package and its transitive dependencies;
- `--filter` selects by package name, directory or pnpm-compatible relation
  syntax, including dependency and dependent closures;
- `-w` selects the workspace root package;
- `--fail-if-no-match` turns an empty filter into a failing signal.

Use `dependsOn` for real task prerequisites. A same-package or explicit
`package#task` edge expresses a concrete prerequisite; a dependency-relative
edge should name which manifest dependency fields participate. Inspect the
resolved graph or verbose execution summary when transit nodes, packages
without the named task or root scripts make selection non-obvious.

`--parallel` ignores dependency ordering. Reserve it for independent or
persistent development processes whose lifecycle is managed by the caller;
cap concurrency when resource contention matters. Do not apply it to builds,
code generation or publication merely to increase throughput.

## Configure task caching for correctness

Configured Vite+ tasks are cached by default while `package.json` scripts are
not, unless current config or CLI flags change that choice. Mark watchers,
servers, deployments and mutable external actions `cache: false`.

Vite Task's [automatic data tracking](https://viteplus.dev/guide/automatic-data-tracking)
observes filesystem reads, missing-file probes, directory listings and writes.
It cannot generally observe environment-variable reads or decide that every
observed path is a stable input or restorable output. Cooperative tracking
adds Vite-aware metadata for supported built-ins such as `vp build`, but verify
support against the installed version.

For each cached task:

- put result-affecting variables in `env` so they are passed and fingerprinted;
- use `untrackedEnv` only for values that must be passed but provably do not
  affect the result;
- omit `input` and `output` to retain automatic tracking;
- include `{ auto: true }` when adding patterns without replacing automatic
  tracking;
- remember that string globs are package-relative by default and use an
  explicit workspace base for root or cross-package paths;
- declare or exclude outputs deliberately when automatic restoration would be
  incomplete or unsafe.

Run the task once uncached or on a miss, then immediately again to prove a warm
hit. Change a representative source, dependency output, config value and
fingerprinted environment value to prove invalidation. On a hit, verify that
required files are restored and usable. Use verbose or last-run details and
cache-miss reasons before considering `vp cache clean`.

Package-manager cache and Vite Task result cache are separate. In CI, install
dependencies before restoring the workspace-root task cache. The current
[GitHub Actions cache guide](https://viteplus.dev/guide/github-actions-cache)
marks cross-run Vite Task caching experimental; adopt it only after local warm
hits are stable and transfer time beats recomputation.

## Migrate the complete workspace as one change

`vp migrate` mutates dependencies, imports, scripts, shared package-manager
configuration, catalogs or overrides, lockfiles and possibly hooks, editor or
agent files. For a monorepo, run it only at the workspace root; member-only
migration cannot safely reconcile shared manager state.

Before migration:

1. record the resolved Vite+, Vite, Vitest, Node and package-manager versions;
2. inspect the current lint, format, test, build, pack, hook and CI contracts;
3. obtain a clean or clearly partitioned worktree baseline and identify files
   that migration may format;
4. read the version-matched migration rules and local `vp help migrate`;
5. separate required toolchain prerequisites from an optional upgrade, and
   request authority before downloads or dependency changes.

When authorized mutation is required, call `implementation` immediately
before running migration or editing configuration. Pass the `monorepo` scope
map, root and members, resolved versions, manager and lockfile, dirty baseline,
current tool contracts, expected migration surfaces, preserved exceptions and
verification matrix. This skill owns Vite+ migration decisions;
`implementation` exclusively owns safe execution, diff preservation and final
accounting. If unavailable, return a migration brief and stop before mutation.

Review every changed manifest, workspace config, catalog or override, source
import, script, hook, CI file and lockfile. Confirm that each package needing a
peer provider still declares the appropriate direct edge and that the resolved
workspace contains one coherent Vite/Vitest toolchain where the installed
version requires it. Preserve unrelated protocols, catalogs, manager settings,
browser providers and hook policies unless current migration rules explicitly
cover them and the user accepted the change.

Install through the selected manager, then verify root policy and every
affected app or package with explicit targets. Rerun migration to check its
documented idempotency only after the first diff is reviewed and stable. A
second unexpected diff is migration evidence to investigate, not formatting
noise to discard.

## Keep CI explicit and reproducible

Pin the Vite+ setup integration to an exact supported release or commit as the
current [CI guide](https://viteplus.dev/guide/ci) requires, and keep its Node
and package-manager selection aligned with repository declarations. Use a
frozen or immutable lockfile mode after the committed lockfile is established.

Use explicit `-C`, package task references, recursive scopes or filters in
non-interactive jobs. Add `--fail-if-no-match` where an empty selection would
hide a configuration error. Record the comparison base and history depth for
changed-package filters, and widen the run when that evidence is unavailable.

Do not treat setup integration dependency caching as proof that Vite Task
outputs were cached. Inspect both layers separately, keep task-cache artifacts
scoped to compatible operating systems and architectures, and ensure caches
cannot include credentials or host-specific absolute paths.

## Diagnose by resolution layer

Classify failures before changing config:

- wrong app or non-interactive ambiguity: inspect eligibility, `-C` and
  `defaultPackage`;
- command behaves differently from a package script: compare the built-in with
  `vp run <name>`;
- config or plugin resolves relative paths incorrectly: compare `-C` with a
  positional Vite root and inspect `process.cwd()` assumptions;
- task order is wrong or a dependency is absent: inspect member manifests and
  the resolved task graph;
- filter succeeds without work: inspect matches and require
  `--fail-if-no-match`;
- stale or incorrect cache result: rerun without cache, inspect tracked inputs,
  outputs and environment, then test invalidation;
- duplicate or incompatible Vite/Vitest internals: inspect direct dependencies,
  aliases, overrides, catalogs, peers and the lockfile against current
  migration rules;
- manager behavior differs across machines: compare declaration, selected
  manager version, Node version, lockfile mode and environment diagnostics.

Do not clear caches, flatten all config, add root dependencies to every member
or replace the package manager before locating the responsible layer.

## Verify and report

Validate the root config and representative override matches, explicit app
commands, recursive or filtered task membership, dependency ordering, package
scripts versus built-ins, cold and warm cache behavior, invalidation and output
restoration. For migrations, add install, check, test, build or pack evidence
for every affected package type plus an idempotency check when safe. Inspect
exit codes, package counts, skips and cache explanations rather than accepting
one green summary line.

Report workspace root, Vite+ and bundled tool versions, package manager and
lockfile, target packages, config ownership, package and task graph decisions,
exact commands and working directories, cache controls and observations,
migration surfaces, CI pins, changed files, checks not run and residual
version-specific uncertainty.
