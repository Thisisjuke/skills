---
name: implementation
description: Apply a bounded repository change safely by inspecting context, preserving scope and existing work, validating proportionately, and accounting for the final diff.
license: MIT
metadata:
  uses-glossary: "true"
---

# Implementation

Apply an authorized repository change with a small, trustworthy mutation loop.
Use this skill alone for a focused correction or compose it with a specialized
implementation skill for feature- or spec-level work.

## Establish the change boundary

Identify the requested outcome, affected surface, acceptance evidence,
constraints and authorized mutations. Read applicable repository instructions,
nearby conventions and root-to-nearest `GLOSSARY.md` files.

Resolve consequential uncertainty from repository evidence when practical.
Otherwise disclose a reversible assumption or ask when alternatives materially
change behavior, scope or irreversible cost.

Implementation authority does not imply permission to commit, push, open a
pull request, deploy, migrate external data or coordinate other agents.

## Inspect before editing

Trace the current behavior through its real entry points, callers, tests,
configuration and data boundaries. Read enough surrounding code to preserve
local invariants without turning a focused task into an exhaustive audit.

Inspect the working tree first. Preserve unrelated and pre-existing changes;
do not overwrite, revert, reformat, move or clean them up.

## Make one coherent change

Choose the smallest change that completely satisfies the current boundary.
Follow existing architecture and extension points where they fit. Prefer direct
code over speculative abstractions and avoid mixing unrelated refactoring,
dependency upgrades or formatting churn.

Preserve behavior outside the requested change. Handle affected failure,
cleanup, partial-state and boundary paths. Note useful adjacent work without
implementing it unless leaving it untouched makes the requested result
incorrect or unsafe.

## Validate proportionately

Use the cheapest trustworthy evidence for the changed behavior: focused tests,
compilation, static checks, a build, runtime inspection or an applicable manual
scenario. Follow repository-native commands and selected technology guidance.

Verify both the intended outcome and plausible neighboring behavior. Read exit
codes, selected tests, skips and tool errors. If a check cannot run, state the
reason and residual risk; do not replace missing evidence with confidence.

When selected, `testing` owns test design, `performance` owns measurement
validity and technology skills own stack mechanics.

## Account for the result

Inspect the final status and diff, accounting for every changed path. Remove
task-created probes and temporary artifacts without disturbing prior work.
Check for accidental scope growth, stale callers or docs, debug output,
incomplete cleanup and a mismatch between the claimed outcome and actual diff.

Do not commit or perform an external action unless separately authorized. A
verified working-tree change is a valid implementation result.

Report:

```text
Outcome implemented
Important choices
Changed surfaces
Validation and observed result
Checks not run and residual risks
Assumptions and unrelated observations
```

## Ownership boundary

`implementation` owns mutation safety, scope preservation, proportionate
verification and diff accountability. It does not own feature completeness,
spec traceability, planning, test strategy, review reporting, documentation
lifecycle or technology rules.

The skill does not call its specializations. This keeps the call graph
directional and acyclic.
