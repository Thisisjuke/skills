# Portable Agent Skill format

Read this reference when authoring optional frontmatter, composition metadata,
glossary behavior or client-specific invocation settings. The target
repository's documented contract takes precedence over this profile when it is
explicit and compatible with the consuming client.

## Standard package

A portable Agent Skill is a directory whose required entrypoint is
`SKILL.md`. Common optional resources are:

```text
skill-name/
├── SKILL.md
├── scripts/
├── references/
└── assets/
```

Additional client files are extensions, not portable requirements. Keep the
core workflow usable without them unless `compatibility` explicitly narrows the
supported environment.

Use relative file references from `SKILL.md`. Prefer one direct reference hop;
deep chains make conditional material difficult to discover.

## Standard frontmatter

The portable baseline requires:

- `name`: 1–64 lowercase ASCII letters, digits and hyphens; no leading,
  trailing or consecutive hyphen; equal to the parent directory name;
- `description`: 1–1024 characters describing both the capability and when it
  applies.

Standard optional fields are:

- `license`: a short license name or pointer to bundled terms;
- `compatibility`: concrete environment requirements, at most 500 characters;
- `metadata`: client or project extensions represented as string key/value
  pairs;
- `allowed-tools`: an experimental space-separated pre-approval field whose
  support varies by client.

Use an optional field only when its consumer and consequence are known. Never
assume experimental tool declarations or custom metadata grant authority.

```yaml
---
name: example-skill
description: Perform a specific reusable workflow. Use when the request has a concrete matching trigger.
compatibility: Requires Python 3 and local filesystem access.
---
```

Readiness, taxonomy and packaging state normally belong in a catalogue or
distribution boundary rather than portable frontmatter. A custom `status`
field cannot prevent a client from activating unfinished instructions.

## Discovery and invocation

The description is the portable discovery signal. Put concrete trigger
branches there because the body is normally loaded only after selection.

Implicit versus explicit-only invocation, UI labels and default prompts are
client policies. Configure them only through a target client's documented
extension. Fields such as `disable-model-invocation` are not part of the
portable Agent Skills baseline and must not be copied between ecosystems by
assumption.

## Skills-factory V1 metadata profile

When the target repository explicitly adopts the skills-factory V1 contract,
only these project metadata keys are permitted:

```yaml
metadata:
  skill-calls: "ALWAYS CALLS implementation; CALLS WHEN NEEDED domain-modeling"
  uses-glossary: "true"
```

Both values are strings. Omit an unused key and omit `metadata` when neither
applies.

### Skill calls

`skill-calls` is a summary for people and validators. The body remains the
operational contract.

- `ALWAYS CALLS <skill-name>` means the advertised workflow cannot complete
  without the target.
- `CALLS WHEN NEEDED <skill-name>` means a body-defined condition triggers the
  target.
- Separate multiple entries with `; `.
- Use installed skill names, not repository paths.
- User-selected composition is not a call and needs no edge.

For every call, the body must define:

1. condition and call point;
2. evidence passed to the target;
3. responsibility retained by the caller;
4. responsibility owned by the callee;
5. behavior when the target is unavailable;
6. approval and authority boundaries;
7. an active-chain cycle guard.

A call never installs its target, copies the absent workflow or expands user
authority. A mandatory missing target blocks the affected advertised outcome.
A conditional missing target leaves valid partial results intact and is
reported as a configuration gap.

### Glossary access

Set `uses-glossary: "true"` only when the skill directly reads, creates or
modifies `GLOSSARY.md`. Calling `domain-modeling` without direct file access
does not require the flag.

The body must state the access mode and resolution:

1. read applicable glossaries from repository root toward the target path;
2. combine broader definitions before narrower ones;
3. accept a nearer meaning only when it explicitly specializes or overrides;
4. surface undeclared contradictions;
5. resolve each target path independently in multi-package work.

Metadata is discoverability, not permission. Canonical term changes belong to
the designated terminology owner, initial structure to the setup owner, and
consulting skills remain read-only unless separately authorized.

## Resource boundaries

- `SKILL.md`: workflow and information needed on every invocation.
- `references/`: facts or procedures needed only for named branches.
- `scripts/`: deterministic operations whose repeated reimplementation would
  be fragile; dependencies and failure behavior must be explicit.
- `assets/`: templates and material intended to be copied or transformed into
  output, not hidden instructions.

Adding a resource is not progressive disclosure unless `SKILL.md` states when
to use it. A resource with no caller is unused packaging.
