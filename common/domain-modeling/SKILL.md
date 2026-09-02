---
name: domain-modeling
description: Establish and maintain project-specific domain language in hierarchical GLOSSARY.md files while detecting ambiguities and durable decisions during collaborative work.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED documentation"
  uses-glossary: "true"
---

# Domain modeling

Maintain a precise shared language while work is being discussed. Treat the
applicable `GLOSSARY.md` files as the durable source of truth for project and
domain terminology, not as a transcript of the conversation.

## Resolve the applicable language

Determine the repository root and the files or package involved in the current
work. Find every `GLOSSARY.md` from the repository root toward that target and
read them from broadest to narrowest.

- Combine definitions from all applicable levels.
- A nearer glossary may specialize or override a broader definition only when
  it explicitly names the overridden term and explains the difference.
- Surface an undeclared contradiction and ask which meaning is intended.
- When work spans several packages, keep each package's language separate and
  identify any genuinely shared terms that belong at the root.

If no applicable glossary exists, explain the missing project setup and ask
before creating one at the nearest genuine domain or package boundary. This
skill remains usable without `setup-domain-modeling`; it does not invoke setup
or silently create a repository-wide structure.

## Sharpen language during the work

Watch for terminology that is vague, overloaded, contradictory, or newly
agreed. When a term needs attention:

1. State the ambiguity using the concrete meanings currently in play.
2. Check existing glossaries, project documentation, APIs, types, tests and
   representative code for evidence of established usage.
3. Test the candidate definition with a concrete scenario or boundary case
   when that could expose a hidden distinction.
4. Propose a canonical term and a short definition.
5. Treat an explicit user statement as confirmation; otherwise obtain
   confirmation before changing the glossary.
6. Update the narrowest glossary that owns the meaning. Move the definition to
   the root only when it is truly shared across scopes.

Do not interrupt for ordinary words whose meaning is already clear. The goal is
decision-changing precision, not an exhaustive dictionary.

When a confirmed definition conflicts with scenarios, public interfaces, types
or tests, identify which artifact is authoritative for the affected behavior.
Do not silently rewrite the glossary to match accidental code or relabel code
to conceal a requirements conflict. Report the contradiction and route the
behavioral or implementation correction to its actual owner.

## Write glossary entries

Match an established local format when one exists. Otherwise use:

```markdown
## Terms

### Consumer

An application or package that uses the public contracts of this scope.

**Avoid:** client, caller
```

Keep each definition concise and implementation-independent. Include only
project-specific meanings. `Avoid` is optional and records synonyms that have
caused or are likely to cause ambiguity.

For an intentional local specialization, make the relationship explicit:

```markdown
### Consumer

Within this package, a deployed application that imports the UI package.

**Overrides:** root `Consumer`, which also includes internal packages.
```

Preserve unrelated entries and human formatting. Do not replace a curated
glossary wholesale. When renaming or consolidating terms, update affected
definitions and references coherently, and report anything that could not be
resolved safely.

## Keep artifacts separate

Put only canonical domain language in `GLOSSARY.md`:

- implementation behavior and acceptance criteria belong in specifications or
  maintained project documentation;
- task state and next actions belong in plans or handoffs;
- architectural rationale belongs in an ADR;
- facts already obvious from code or configuration remain in those sources.

Do not use the glossary as a scratchpad, conversation log, architecture map or
list of every class and component.

## Detect decisions that may need an ADR

A decision may merit an ADR when all three conditions hold:

- reversing it later would be meaningfully costly;
- a future contributor would find it surprising without the rationale;
- it resolves a real trade-off between credible alternatives.

When these conditions appear, summarize the decision, alternatives, rationale
and expected consequences. Ask the human whether to record an ADR. After
approval, call `documentation`, which owns discovery of the repository's ADR
convention, location, format, status and supersession lifecycle.

Do not write the ADR yourself or duplicate the documentation workflow. If
`documentation` is unavailable, report the missing capability and preserve the
approved decision in the current response so it is not silently lost.

## Invocation safety

Before calling another skill, verify that it is not already active in the
current invocation chain. Never call this skill, the current caller, or another
ancestor. If a call would create a cycle, stop the delegation and report it.

Reading an applicable glossary is consultation, not a skill invocation.

## Completion

Report:

- glossaries consulted and changed;
- terms added, changed, moved or left conflicted;
- explicit local overrides;
- ADR candidates accepted, declined or awaiting documentation;
- remaining ambiguities that need a human decision.
