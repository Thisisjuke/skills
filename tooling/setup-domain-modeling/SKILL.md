---
name: setup-domain-modeling
description: Configure a repository once for hierarchical domain glossaries by inspecting its package structure, confirming scopes, updating root AGENTS.md, and initializing every applicable GLOSSARY.md.
license: MIT
metadata:
  skill-calls: "ALWAYS CALLS domain-modeling"
  uses-glossary: "true"
---

# Setup domain modeling

Configure the current repository for durable domain language. Run this setup
once when adopting `domain-modeling`, and rerun it when repository boundaries
change. Explore first, present a concrete proposal, and write only after human
confirmation.

This setup always calls `domain-modeling` to analyze and seed terminology. It
owns repository discovery, scope scaffolding and the root `AGENTS.md` contract;
it does not implement a second glossary-authoring workflow.

## Preconditions and boundaries

- Work only in the current repository.
- Require `domain-modeling` to be available before claiming setup can complete.
  Do not install it, reproduce it, or silently continue without it.
- Preserve existing `AGENTS.md` and glossary content.
- Do not modify package manifests, source code, build files or generated files.
- Do not create ADRs during setup unless a separate, explicit decision reaches
  the normal human-approval boundary.
- Keep calls acyclic. Do not call an already-active skill or an ancestor.

## 1. Inspect the repository

Determine the repository root and inspect enough structure and documentation to
identify real scopes. Check, when present:

- root and nested `AGENTS.md` files;
- existing `GLOSSARY.md` files;
- workspace manifests and module declarations;
- package/application manifests and independently buildable modules;
- conventional `apps/`, `packages/`, `services/`, `modules/` or equivalent
  roots;
- architecture and domain documentation that names bounded contexts;
- source trees only as needed to distinguish a domain boundary from an
  organizational directory.

Recognize monorepos across ecosystems rather than relying on one JavaScript
signal. A declared workspace/module, independently published package,
deployable application or documented bounded context is a strong boundary.
Ignore vendored dependencies, generated output, caches, build artifacts,
fixtures and examples unless the repository explicitly treats one as a real
maintained package.

Classify the repository as single-scope or multi-scope. Always include the
repository root. In a multi-scope repository, include every detected package or
domain boundary even when no vocabulary terms are known yet: the empty glossary
marks setup completion and gives later work an unambiguous nearest scope.

## 2. Present the proposal

Before writing, show:

- repository classification and the evidence used;
- every proposed scope and `GLOSSARY.md` path;
- existing glossaries that will be preserved;
- the exact managed section proposed for root `AGENTS.md`;
- uncertain directories that were excluded or need a human decision;
- files that will be created or updated.

Ask for confirmation. Incorporate additions, removals or renamed scopes before
continuing. Do not equate confirmation of the general setup idea with approval
of an undisclosed file list.

## 3. Update root AGENTS.md

Create root `AGENTS.md` if it does not exist. Preserve all existing content and
manage exactly one delimited section:

```markdown
<!-- domain-modeling:start -->
## Domain language

Read every applicable `GLOSSARY.md` from the repository root toward the files
being changed. Combine their definitions; the nearest explicit definition
takes precedence. A local glossary may override a broader term only when it
states the difference. Surface undeclared conflicts instead of guessing.

Use `domain-modeling` when project terminology is introduced, clarified,
renamed or contradicted. Glossary changes belong to the narrowest scope that
owns the meaning; genuinely shared language belongs at the repository root.
Skill calls must remain acyclic. Creating an ADR from a conversation requires
human approval before `documentation` is called.
<!-- domain-modeling:end -->
```

If the markers already exist, replace only their enclosed block. If an
unmanaged section already establishes equivalent or conflicting rules, show
the conflict during confirmation and merge deliberately rather than appending
a duplicate policy.

## 4. Create scope glossaries

For every confirmed scope, create `GLOSSARY.md` when missing. Existing files
remain intact for `domain-modeling` to update. Use the scope's established name
and this minimal form when no local format exists:

```markdown
# <Scope name> glossary

Canonical project language for `<scope path>`.

## Terms
```

An empty `Terms` section is intentional and complete: it records that the scope
has been configured without inventing terminology.

Never delete an existing glossary automatically. If a previously configured
scope no longer exists or no longer appears to be a boundary, report the stale
file for human disposition.

## 5. Call domain-modeling

After the structure exists, call `domain-modeling` with:

- the confirmed scope map;
- the repository evidence already inspected;
- the instruction to review all initialized glossaries without inventing
  terms;
- any terminology conflicts or candidate shared terms found during discovery.

Let `domain-modeling` propose and confirm canonical language using its own
workflow. One call may cover the complete confirmed map; do not recursively
call it once per directory unless scope isolation makes that necessary.

## 6. Verify

Confirm that:

- root `AGENTS.md` contains exactly one managed domain-language section;
- every confirmed scope has one `GLOSSARY.md`;
- all pre-existing glossary content remains present unless the human approved
  a domain-modeling change;
- root-to-nearest resolution is unambiguous for representative files in each
  scope;
- no ignored/generated directory received a glossary;
- the `domain-modeling` call completed or its configuration gap is clearly
  reported.

Summarize created and updated files, initialized scopes, seeded terms,
uncertainties and any follow-up requiring a human decision.

## Idempotency

On rerun, rediscover the repository and present the delta. Update the managed
`AGENTS.md` block in place, preserve existing glossaries, create glossaries only
for newly confirmed scopes and flag obsolete scopes without deleting them.
Never duplicate markers, headings or glossary files.
