---
name: documentation
description: Create and maintain durable documentation for behavior, decisions, constraints, interfaces, and operational knowledge.
license: MIT
metadata:
  uses-glossary: "true"
---

# Documentation

Create durable documentation that preserves knowledge a reader cannot recover
reliably from code, configuration or history alone. Explain behavior,
constraints, interfaces, operations and the reasons behind significant
decisions without building a second, stale copy of the repository.

Write only when the request or a calling skill authorizes documentation
changes. Otherwise inspect and propose the smallest useful update.

## Establish the documentation contract

Before writing:

1. Identify the audience, question, scope and expected lifetime of the
   information.
2. Read applicable repository instructions and `GLOSSARY.md` files from the
   repository root toward the target, with an explicit nearer definition
   taking precedence. Then read the source, tests, configuration and existing
   docs that establish current truth.
3. Find the repository's established documentation locations, formats, naming,
   indexes and generators. Preserve them instead of introducing a parallel
   convention.
4. Choose one canonical home. Update or link to it rather than repeating the
   same meaning across README files, comments and guides.
5. State uncertainty when the available evidence conflicts or cannot establish
   the behavior being documented.

Use canonical project terminology. Consulting a glossary does not authorize
editing it. If terminology is unresolved, report the ambiguity to the caller or
human; do not call `domain-modeling` when it is already an ancestor, and do not
invent a definition.

## Decide what deserves documentation

Keep information whose value survives ordinary code changes:

- intent and rationale that implementation cannot reveal;
- stable behavior, public contracts and compatibility expectations;
- constraints, invariants and non-obvious failure consequences;
- operational procedures and recovery knowledge that must be followed;
- boundaries, ownership and pointers to authoritative sources;
- significant decisions and why credible alternatives were rejected.

Avoid caches of cheap repository lookups:

- exhaustive file, class, component, command or flag inventories;
- exact constants, counts and line numbers already owned by code or config;
- prose that merely restates an implementation;
- speculative examples presented as supported behavior;
- conversation history, temporary task state or a narrative changelog of the
  work just performed.

When a precise command, schema or API must be copied for usability, identify
its authoritative source and verify the copy. Prefer a durable pointer when a
reader can obtain the truth more safely with one file or command.

## Maintain the documentation tree

Make the relevant documentation reachable from its existing hub. A root
README or documentation index should describe what kind of knowledge each
sub-document owns and link to it; it should not transcribe the sub-document.

When documents overlap:

1. identify the canonical owner;
2. move any unique durable meaning there;
3. replace other copies with concise pointers where useful;
4. preserve intentional audience-specific explanations without duplicating
   normative rules.

Do not perform broad documentation cleanup merely because nearby material is
stale. Keep edits within the authorized scope and report adjacent problems.

## Record architecture decisions

An ADR records why a consequential decision was made. It is appropriate when
the choice is meaningfully costly to reverse, surprising without context, and
the result of a real trade-off. A call from `domain-modeling` should include the
human's approval and the candidate decision evidence; verify that the record
has enough context, but do not repeat the discovery interview. If approval is
not explicit in the call, ask before writing the ADR.

### Follow the repository convention

Inspect existing ADRs, instructions and ADR tooling first. Match their:

- directory and scope;
- numbering and filename pattern;
- markup and section headings;
- status vocabulary and supersession method.

When several conventions conflict, surface the conflict. Do not silently start
a second series. If no convention exists, use `docs/adr/NNNN-short-title.md`,
incrementing the highest existing number, with this lightweight shape:

```markdown
# Short decision title

Status: Accepted
Date: YYYY-MM-DD

## Context

What forced a choice, including the decisive constraints.

## Decision

What was chosen and why.

## Consequences

Important benefits, costs, risks and follow-up constraints.
```

Add rejected alternatives only when their rejection is non-obvious or likely
to be reconsidered. A short decision may remain short; do not fill sections
with generic prose merely to satisfy the template. Use `Proposed` instead of
`Accepted` when the user requests a draft for a decision not yet approved.

### Preserve decision history

Use the repository's lifecycle when present. Otherwise use `Proposed`,
`Accepted`, `Deprecated` and `Superseded by NNNN`.

- Correct factual or editorial mistakes in place when rationale is unchanged.
- When the decision changes, create a new ADR and mark the old one superseded.
- Do not delete an old ADR merely because it is no longer current.
- Keep current documentation aligned with the active decision and link to the
  ADR for rationale rather than copying it.

## Keep neighboring artifacts separate

- `GLOSSARY.md` owns canonical domain terms; `domain-modeling` changes them.
- A specification owns desired behavior and acceptance boundaries for a
  change.
- A plan owns sequencing, dependencies and execution state.
- A handoff owns temporary continuation context.
- Git history owns the mechanical narrative of edits.

Reference these artifacts where useful, but do not absorb them into general
documentation or an ADR.

## Verify the result

Before finishing:

- check that every behavioral and operational claim matches current evidence;
- verify links, referenced paths and examples;
- ensure one canonical owner remains for each normative fact;
- confirm new ADR numbering, status and supersession links;
- remove placeholders, copied implementation inventories and unsupported
  claims;
- review the document as a reader without conversation history.

Report files created or changed, their intended audience and canonical role,
verification performed, unresolved conflicts and any follow-up deliberately
left outside scope.
