---
name: screenshot-critique
description: Critique screenshots with a deliberately fresh visual inspection that identifies concrete presentation defects, confidence, affected regions, and missing evidence without being primed by implementation history.
license: MIT
---

# Screenshot critique

Use a fresh inspection to challenge visual work before it is accepted. This
skill evaluates what is visibly present in one or more captures. It does not
measure pixel similarity, prove usability with users, certify accessibility or
decide an unresolved product preference.

## Prepare truthful evidence

Use current captures from the surface and state under review. When a report
includes a particular viewport, zoom, theme, locale, data state or camera,
reproduce that framing as closely as practical. Additional probe views may
supplement it but do not replace it.

Include:

- full captures for context;
- focused crops when scale would hide the relevant detail;
- all materially different states and viewports in scope;
- deterministic still frames for animation when a specific frame matters;
- the intended outcome or evaluation criteria, without revealing an expected
  verdict.

Verify that the supplied capture is current. When assessing a claimed visual
change, establish that the before and after artifacts actually differ before
attributing an improvement to the change.

## Run an unprimed pass

Prefer an independent reviewer that has not seen the implementation history.
Give it neutral context: the captures, relevant crops, the surface's purpose
and the dimensions to inspect. Do not name the suspected defect, preferred
answer or implementation technique.

Ask it to distinguish:

- concrete visible defects;
- likely defects that need another view or state;
- intentional-looking choices that may still need product confirmation;
- observations unsupported by the pixels.

Useful inspection dimensions include hierarchy, alignment, spacing, clipping,
overflow, typography, contrast, color consistency, layering, responsive state,
empty/loading/error states, focus and selection indicators, icon readability,
blur, artifacts and missing content. Select only dimensions relevant to the
surface.

If no independent reviewer is available, perform an adversarial second pass:
for each important feature, state the strongest pixel-grounded case that it is
wrong before deciding whether the evidence supports that case.

## Reconcile findings

Inspect every high-confidence or high-impact observation directly. Agreement
between independent and primary inspection strengthens the finding; novelty
is a reason to investigate, not automatic proof. A clean fresh pass does not
replace direct inspection or existing regression checks.

Keep visual fidelity distinct from other owners:

- use `compare-screenshots` when pairwise telemetry or aligned differences are
  needed;
- use accessibility expertise for semantic, keyboard or assistive-technology
  evidence;
- use UX research or audit methods for task success and user friction.

## Report

For each finding, provide:

- visible observation;
- image, state and region where it appears;
- impact on reading, interaction or fidelity;
- confidence and missing evidence;
- suggested correction or next capture when appropriate.

Conclude with `accept`, `revise`, or `inconclusive`, plus the evidence needed to
change that verdict. Do not declare a reported visual defect fixed solely from
the implementer's primed inspection.
