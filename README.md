# Juke's Agent Skills

Reusable playbooks for coding agents — built from workflows I actually use.

Hey, I'm Arthur — Juke online 👋

This repository is my personal collection of **Agent Skills**: small packages
of instructions, references and tools that give coding agents a repeatable way
to perform a specific kind of work.

Think of them as reusable pieces of expertise you can plug into an agent:

```text
review
+ java
+ java-me
+ java-me-review
```

instead of maintaining one enormous prompt that tries to know everything.

You can install a single skill, combine several of them, adapt them to your own
workflow, or simply browse the repository for ideas.

Everything here is **MIT licensed**, designed to be portable, and comes from
workflows I actually want to reuse.

> **Behind the scenes**
>
> I don't maintain this repository by hand. Skills are authored, evaluated and
> selected in a private skill factory before being published here.
>
> [Read how and why →](./HOW-ITS-MADE.md)

---

## What can I use these skills for?

A few examples:

| I want my agent to...                          | Start with                               |
| ---------------------------------------------- | ---------------------------------------- |
| Turn a fuzzy request into something actionable | [`interview-me`](./common/interview-me/) |
| Explore, challenge or refine an idea           | [`idea-refine`](./common/idea-refine/)   |
| Review code systematically                     | [`review`](./foundations/review/)        |
| Research a technical question                  | [`research`](./common/research/)         |
| Explore the shadcn ecosystem before adopting   | [`discover-shadcn`](./discover/discover-shadcn/) |
| Prepare work for another agent or session      | [`handoff`](./common/handoff/)           |
| Understand Java-specific constraints           | [`java`](./technologies/java/)           |
| Work safely on Java ME / CLDC / MIDP code      | [`java-me`](./technologies/java-me/)     |

Some skills work perfectly on their own.

Others become more useful when combined.

---

## Quick start

### Install with the Skills CLI

```bash
npx skills add Thisisjuke/skills
```

The CLI lets you choose which skills to install and where.

You don't need to install the whole repository.

For example, you might start with:

```text
interview-me
idea-refine
review
handoff
```

Then ask your coding agent to use the relevant skill for the task.

For example:

```text
Use the idea-refine skill to challenge this feature idea before we start
implementing it.
```

or:

```text
Use review together with the Java skills to review this module.
```

---

## Manual installation

No special installer is required.

Every directory containing a `SKILL.md` is an independently copyable skill
package.

1. Pick the skill you want.
2. Copy its complete directory into a skill location supported by your agent.
3. Keep its accompanying files together:

   - `references/`
   - `scripts/`
   - `assets/`
   - and any other resources included in the package.

4. Check whether the skill declares another skill it may need.
5. Run any one-time setup skill named by the package before relying on the
   repository workflow it configures.

Don't copy only `SKILL.md` if the package contains additional resources.

### Portable doesn't mean tested everywhere

The packages use a portable `SKILL.md`-based format and keep their runtime
resources inside their own directories. That makes them straightforward to
copy between compatible agents.

It does not mean that every skill has been tested with every coding agent or
that every client supports declared skill calls in the same way. Check your
agent's skill discovery rules and composition support before relying on a
specific setup.

---

## Skills are deliberately small

I prefer focused skills over giant prompts.

A skill should have a clear job:

```text
review
```

knows **how to review software**.

```text
java
```

knows **what changes when the software is Java**.

```text
java-me
```

adds the constraints of **CLDC / MIDP and constrained devices**.

```text
java-me-review
```

adds checks that are specifically useful when **reviewing Java ME software**.

Together:

```text
review
+ java
+ java-me
+ java-me-review
```

becomes a fairly specialized review agent without any of those skills having
to own the entire workflow.

That's the main idea behind this repository:

> **compose expertise instead of growing one giant prompt.**

---

## A few composition recipes

### Review a project

```text
review
+ <technology>
+ <technology>-review
```

Example:

```text
review
+ java
+ java-review
```

### Review a constrained Java ME project

```text
review
+ java
+ java-me
+ java-me-review
```

Each layer adds something different:

- `review` → review method
- `java` → Java language and runtime guidance
- `java-me` → CLDC/MIDP platform constraints
- `java-me-review` → Java ME-specific review checks

### Go from a vague idea to work another agent can execute

```text
interview-me
→ idea-refine
→ specification
→ planning
→ handoff
```

### Discover something before adopting it

```text
need
→ discover-<subject>
→ qualified choice
→ <technology>
→ implementation
```

Discovery skills are for fast-moving ecosystems where choosing the right
component, registry or tool deserves a repeatable qualification workflow. They
produce a small, evidence-backed shortlist and stop before installation.

The `discover-<subject>` family lives separately from generic `research` and
technology usage. Not every technology needs a matching discovery skill. The
first published example is
[`discover-shadcn`](./discover/discover-shadcn/).

You don't have to use any of these exact chains.

Skills are building blocks, not a mandatory framework.

---

## Browse the collection

The repository is organized by **what kind of knowledge a skill adds**, not by
the order in which you must use it.

```text
.
├── foundations/
├── common/
├── discover/
├── technologies/
└── tooling/
```

### 🧱 [`foundations/`](./foundations/)

Reusable methods for core software work.

Typical topics include:

- specification
- planning
- implementation
- testing
- debugging
- review
- documentation
- accessibility
- performance

These skills are usually technology-neutral.

Use them when you want to improve **how the agent works**.

### 🧰 [`common/`](./common/)

Reusable product and engineering workflows.

Examples include:

- refining an idea
- interviewing the user
- researching a question
- designing an API
- modelling a domain
- managing releases
- assessing security
- assessing observability
- handing work to another agent

These skills generally solve a specific problem that appears across many kinds
of projects.

### 🔭 [`discover/`](./discover/)

Subject-specific workflows for exploring a fast-moving external ecosystem
before adopting anything from it.

A `discover-<subject>` skill starts from a concrete need, qualifies candidates
for fit, freshness, provenance, dependencies and maintenance cost, and returns
a small shortlist. It stops before installation or project mutation.

Use [`discover-shadcn`](./discover/discover-shadcn/) when you want to compare
shadcn/ui registries, components, blocks, themes or adjacent tools before
choosing one.

### ☕ [`technologies/`](./technologies/)

Technology and platform-specific guidance.

For example:

```text
technologies/
├── java/
│   ├── SKILL.md
│   ├── java-build/
│   └── java-review/
└── java-me/
    ├── SKILL.md
    └── java-me-review/
```

Nested skills represent increasing specialization.

A general method can therefore stay generic while the technology skills
provide the constraints that actually matter for the project.

### 🛠️ [`tooling/`](./tooling/)

Skills for working on **skills themselves** and preparing the repositories that
use them.

This includes workflows for things such as:

- setting up project-owned skill infrastructure
- finding good source material
- authoring portable skills
- reviewing skills
- evaluating skill quality

Use these if you want to build or improve your own skill collection.

---

## How skills interact

Some skills can explicitly delegate part of their work to another skill.

Two relationships may appear in their metadata:

```text
ALWAYS CALLS
```

The other skill is a required part of the workflow.

```text
CALLS WHEN NEEDED
```

The skill delegates only when a particular situation occurs.

The body of the skill explains:

- when the call should happen,
- what information should be passed,
- what is expected back,
- and what to do if the other skill isn't available.

These relationships describe runtime behaviour.

They are **not package-manager dependencies** and they don't prevent you from
combining independent skills however you want.

### Run one-time setup when required

Some skills use project infrastructure that is deliberately not bundled with
this repository. In particular, `uses-glossary: "true"` means a skill may read
or manage `GLOSSARY.md` files owned by the project using it.

For a new repository that will use hierarchical domain language, install and
run [`setup-domain-modeling`](./tooling/setup-domain-modeling/) once. It
inspects the repository, proposes the exact `AGENTS.md` and glossary scopes,
and asks for confirmation before writing. Run it again when package or domain
boundaries materially change.

Setup skills are selected separately. Ordinary skills don't install or invoke
them silently, and their documented fallback applies when setup is absent.

---

## Machine-readable catalogue

[`skills.json`](./skills.json) contains the complete published collection in a
machine-readable format, including each package's description and declared
calls.

Use it if you're building tooling around this repository or want to inspect
the available skills programmatically.

For humans, the examples and directories above are usually easier.

---

## What I care about

This repository isn't an attempt to collect as many prompts as possible.

I care much more about skills being:

- **focused** — one clear responsibility;
- **portable** — the package carries what it needs;
- **composable** — skills can work together without becoming one giant prompt;
- **explicit** — boundaries and relationships should be understandable;
- **replaceable** — improving one part shouldn't require rewriting everything;
- **reusable** — the workflow should be worth using again;
- **tested before publishing** — not just an idea dropped into a folder.

Some skills are broad.

Some are extremely specific.

That's intentional.

If something deserves reusable expertise, it can probably deserve a skill.

---

## How this repository is made

The public repository is generated from a private workspace where I develop
and evaluate skills.

That separation lets me experiment freely while keeping this repository
relatively clean and predictable.

The short version:

```text
research
→ author
→ evaluate
→ compare
→ select
→ publish
```

The longer version — including why I use this structure and how publication
works — is here:

👉 [HOW-ITS-MADE.md](./HOW-ITS-MADE.md)

---

## Contributions and feedback

Feel free to:

- use the skills as they are;
- adapt them to your own setup;
- borrow ideas for your own skills;
- report something that behaves badly;
- suggest a better approach;
- propose a new use case.

Because this repository is generated from the private skill factory, published
files aren't the canonical authoring source. Durable changes must be integrated
in the factory before they can appear in a future release.

Issues are therefore the easiest place to start. Pull requests are also useful
as concrete proposals or patches, but an accepted change is reapplied in the
factory and regenerated rather than reverse-synced from the public repository.

- [Open an issue](https://github.com/Thisisjuke/skills/issues)
- [Open a pull request](https://github.com/Thisisjuke/skills/pulls)

---

## License

MIT.

Use the skills, modify them, combine them, or build your own collection from
them.

See [LICENSE](./LICENSE).
