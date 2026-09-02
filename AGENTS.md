# Public skills distribution instructions

This repository is a generated, MIT-licensed collection of portable Agent
Skills. It is a distribution surface, not the canonical authoring checkout.

## Use the collection

- Browse [`README.md`](./README.md) for entry points and `skills.json` for the
  complete machine-readable inventory.
- Treat each directory rooted at `SKILL.md` as one package. Copy the complete
  directory, including its references, scripts and assets.
- Read a package's own `SKILL.md` before applying it. That file owns the
  workflow, authorization boundaries, completion conditions and behavior when
  another skill is unavailable.
- A declared skill call uses an already installed target. It never installs a
  dependency or expands user authority automatically.
- Confirm that the target agent supports the package layout and any advertised
  composition before claiming compatibility.

## Instruction scope

These repository instructions describe how to handle the distribution; they
do not replace a package's operational instructions or become a hidden
dependency when that package is copied alone.

Some packages consult project-owned glossary files or require a separate
one-time setup skill. Follow the selected package and the setup guidance in
`README.md`; this distribution does not provide a repository glossary as a
substitute for the target project's terminology.

## Changes

Published files are generated elsewhere and should not be edited directly in
this repository. Propose changes through
[GitHub issues](https://github.com/Thisisjuke/skills/issues) or a pull request.
Inclusion confirms reviewed publication and an MIT declaration, not
compatibility with every agent, project or task.
