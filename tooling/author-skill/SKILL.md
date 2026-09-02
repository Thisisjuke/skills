---
name: author-skill
description: Create or revise portable Agent Skills from real domain evidence. Use when defining a skill boundary, writing SKILL.md, improving discovery metadata, adapting source material, or deciding what belongs in instructions, references, scripts, or assets.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED validate-skill-system"
---

# Author skill

Create a coherent operational capability that remains understandable when its
directory is copied independently. Preserve the user's intended workflow and
the target repository's contract. Do not turn one project incident, preferred
harness or current file layout into a universal rule.

This skill owns skill boundary design, instruction writing and resource
selection. It does not own domain expertise, source licensing decisions,
installation, repository-wide routing or independent behavioral evaluation.

## 1. Establish the target contract

Before editing, inspect the applicable repository instructions, skill format,
catalogue, neighboring skills and validators. Determine whether the task is a
new skill, a focused revision or a proposed split.

Identify the evidence that makes the skill useful:

- a real task and the corrections required to complete it;
- authoritative domain documentation or specifications;
- project runbooks, schemas, incident reports or recurring review findings;
- representative code and tests;
- existing skills whose useful mechanisms or overlaps must be understood.

Do not fill a missing domain model with generic “best practices.” Obtain the
necessary evidence, narrow the promised capability or report the gap. Record
provenance and license constraints when source material may be adapted; prefer
new synthesis over copying substantial source prose.

Completion criterion: the target format, intended requests, evidence base and
authorization boundary are explicit.

## 2. Define one responsibility

Write a compact design note before the skill body:

- purpose and concrete triggering requests;
- inputs and observable outputs;
- knowledge and decisions the skill owns;
- adjacent responsibilities it explicitly leaves elsewhere;
- side effects and approvals;
- useful compositions and genuine delegations;
- completion evidence.

Create a distinct skill only when the responsibility is coherent and useful on
its own or needs independent discovery. Do not split merely to make files
small, and do not combine unrelated branches merely to reduce the skill count.
Nesting and taxonomy do not imply invocation.

When revising, preserve working behavior and unrelated resources. Search for
callers, links and packaging entries before renaming or removing anything.

Completion criterion: another author could tell what belongs in this skill and
what does not without seeing the proposed prose.

## 3. Choose the smallest useful package

Every package needs `SKILL.md`. Add a supporting resource only when it changes
execution:

- keep essential workflow, constraints, gotchas and routing in `SKILL.md`;
- put substantial branch-specific facts or schemas in `references/`;
- put repeatable fragile or deterministic operations in tested `scripts/`;
- put templates or materials intended for generated output in `assets/`;
- add client-specific interface files only when the target client requires
  them.

Each reference needs a pointer that says when to read it. Keep references
shallow and focused. Scripts must document real runtime dependencies, fail with
actionable messages and avoid gaining authority from being bundled.

Do not create placeholder directories, duplicated quick references, a README,
changelog or examples that the workflow never uses.

Completion criterion: every proposed file has one execution-time consumer.

## 4. Write discovery metadata

Use the required portable baseline:

```yaml
---
name: lowercase-hyphen-name
description: What the skill does and the concrete situations in which it applies.
---
```

The name must match its directory. Make the description discriminating enough
to select the skill from metadata alone; include separate trigger branches,
not a list of synonyms or body summary.

Read [the portable format reference](references/portable-format.md) before
adding optional fields, custom metadata, skill calls, glossary access or
client-specific invocation controls. Apply the target repository's contract,
not conventions remembered from another collection.

Completion criterion: metadata is valid for the target contract and neither
under-triggers nor claims unrelated work.

## 5. Write operational instructions

Assume the agent already has general reasoning ability. Include only material
that changes its decisions: non-obvious procedure, domain constraints, source
priority, failure behavior, permissions and proof.

- Use direct actions and decision criteria rather than an explanatory essay.
- Match precision to risk: fixed order for fragile operations, judgment where
  several approaches are valid.
- Keep definitions beside the rules that depend on them.
- Give each workflow stage a checkable completion condition.
- State mutation, network, credential, destructive-action and human-approval
  boundaries where they matter.
- Make output and truthful completion claims explicit.
- Keep early findings visible; workflow order must not delay information that
  changes a human decision.

Examples should expose a recurring input shape, failure mode or output
contract. Avoid embedding a one-off fix, current line number, private path or
implementation history as reusable guidance.

Prune duplication, generic advice, stale setup notes and rules discoverable
cheaply from the environment. Use progressive disclosure when a branch does
not apply to every invocation, but keep safety-critical gotchas visible before
the risky action.

Completion criterion: a fresh agent can execute the advertised workflow using
the package alone and can distinguish success, partial completion and a blocked
configuration.

## 6. Verify the authored package

Inspect the final directory as a standalone unit:

1. parse and validate frontmatter with the target ecosystem's validator;
2. confirm the name, folder, links and pointers agree;
3. execute every added or modified script against representative success and
   failure inputs;
4. check that optional resources are reachable only from meaningful branches;
5. scan for placeholders, source-specific assumptions and hidden repository
   dependencies;
6. compare the finished package with its responsibility note;
7. identify realistic behavioral scenarios for the future independent
   evaluator without pretending that author self-review is independent proof.

When the target distribution uses the compatible portable contract and
`validate-skill-system` is available, call it after edits with the exact roots
that are intended to coexist. Pass those roots and whether the run represents
a complete or deliberately partial distribution. `author-skill` retains
ownership of content and repairs; the validator owns structural contract and
call-graph findings. Its call does not authorize rewrites or installation.

Before that conditional call, check the active invocation chain. Do not call
`validate-skill-system` if it is already active or is an active ancestor;
report the cycle instead. If it is unavailable, run any native structural
validator that exists, report the missing contract gate and do not claim full
structural validation. Preserve results that remain valid without the call.

Completion criterion: structural checks pass, scripts have direct evidence,
known behavioral evaluation remains clearly pending, and the handoff lists
changed files, sources, validations and unresolved risks.
