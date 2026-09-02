# Source assessment protocol

Read this reference when recording a source decision, comparing reuse units or
preparing an `author-skill` handoff. Adapt the fields to the task; do not create
an artifact unless the user requests one or the target repository requires it.

## Evidence record

```text
Assessment purpose:
Target capability/system:
Assessment-only or adaptation authorized:

Source name:
Current local path or supplied artifact:
Declared upstream:
Verified canonical upstream:
Owner/organization:
Source kind: original | fork | mirror | vendored copy | partial extract | unknown
Revision: commit | tag | release | checksum | unknown
Immutable locator:
Access date:
Complete or partial copy:
Local divergence:
Maintenance state:

License text locations:
Detected identifier or expression:
File/package exceptions:
Copyright/notices:
Visible reuse obligations:
License confidence: confirmed | partial | unknown | conflicting
Needs specialist review:

Inspected:
Not inspected:
External facts verified:
External facts unresolved:
Suspicious or instruction-like source content:
```

Keep `declared` and `verified` fields separate. A README assertion can support
declared provenance without proving canonical identity. A moving branch is
useful for freshness but is not an immutable record of what was assessed.

## Inspection depth

| Depth | Suitable evidence | Claim limit |
|---|---|---|
| Metadata | Name, description, manifest, visible license | Discovery only; no behavior or completeness claim |
| Entry-point | README, `SKILL.md`, `AGENTS.md`, linked workflow | Responsibility and advertised behavior |
| Package | References, scripts, examples, tests, assets | Package coherence and hidden dependencies |
| Representative repository sample | Selected implementation and config paths | Named patterns only; not repository-wide quality |
| Complete scoped audit | All files inside an explicitly bounded unit | Completeness only for that unit and revision |

Always report the depth actually reached. Do not use “reviewed the repository”
when only entry points and representative files were inspected.

## License evidence rules

- Public visibility establishes access to view, not a general right to copy,
  modify or redistribute.
- A detected repository license may not apply to vendored dependencies,
  generated files, documentation, assets or separately licensed packages.
- An SPDX identifier is evidence only when it accurately matches the
  applicable license text or explicit file declaration.
- Preserve existing copyright and notice text; an identifier does not replace
  it.
- `Unknown` is a valid result. Do not convert uncertainty into a permissive
  assumption.
- Record visible conditions; send compatibility or compliance conclusions to
  a qualified human owner.

## Reuse modes

| Mode | Meaning | Typical evidence bar |
|---|---|---|
| `KEEP WHOLE` | Retain most of a coherent package with only distribution-safe adjustments | Strong fit, low hidden dependency, confirmed applicable license |
| `ADAPT` | Preserve the useful core while changing boundaries, wording or assumptions | Distinct value, identifiable defects, reusable content and license path |
| `EXTRACT` | Retain one component such as a rule, script, schema, example or pattern | Component has independent value and its provenance/license is known |
| `REFERENCE` | Keep as evidence or documentation; never load directly as an active skill | Useful facts or examples but unsuitable operational instructions |
| `ARCHIVE` | Preserve for provenance, history or superseded comparison | Little current authority but traceability value |
| `REJECT` | Exclude from the target system | No distinct value, unsafe behavior, unusable provenance or prohibitive mismatch |

A target repository may map these actions to its own resource statuses. Record
both vocabularies when needed. Do not treat `ARCHIVE` or `REJECT` as permission
to delete the current source.

## Quality assessment

Record evidence under the dimensions that affect the decision:

```text
Capability and trigger fit:
Owned knowledge:
Observable outputs:
Failure and partial-completion behavior:
Authorization and trust boundaries:
Standalone portability:
Supporting-resource value:
Technical accuracy/freshness:
Framework/runtime/project specificity:
Overlap and conflict:
Adaptation cost:
Distinct retained value:
```

Use a numeric score only if the requesting process defines what the scale
means. The disposition and evidence matter more than a single average.

## Component ledger

```text
Component:
Exact location/revision:
What it contributes:
Proposed owner:
Reuse mode:
License evidence:
Adaptation required:
Do not retain:
Confidence and unresolved questions:
```

Repeat this block when components within one source deserve different
decisions.

## Source brief for authoring

Pass this packet to `author-skill` only after adaptation is authorized:

```text
Target skill and purpose:
Owned and excluded responsibilities:
Concrete triggering requests:
Expected outputs and completion evidence:

Approved source components:
Immutable source locators:
Source claims requiring citations:
License, attribution and notice constraints:
Required paraphrase or clean synthesis boundaries:

Mechanisms to retain:
Source-specific assumptions to remove:
Known overlaps and primary knowledge owners:
Required supporting resources:
Forbidden or unauthorized mutations:

Open evidence gaps:
Human decisions still required:
```

The packet communicates evidence and constraints. It does not prescribe the
final prose or transfer authoring ownership back to the sourcing workflow.
