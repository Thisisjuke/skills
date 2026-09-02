---
name: compare-screenshots
description: Compare two visual captures against an intended target, using aligned evidence and optional image metrics to locate meaningful differences without assuming that either capture is correct.
license: MIT
---

# Compare screenshots

Compare captures to determine which is closer to the intended result, whether
both are wrong, or whether the available evidence is inconclusive. A baseline
is a previous observation, not automatic ground truth. Pixel similarity locates
change; it does not establish correctness.

## Establish the comparison contract

Before interpreting differences, state:

- the visual target or requirement;
- the surface, state and behavior being judged;
- the aspects expected to change and those expected to remain stable;
- the evidence that can establish correctness;
- any unresolved product or aesthetic decision.

If there is no defensible target, describe the ambiguity and request the
missing decision. Do not silently choose the baseline or the newest image.

## Make captures comparable

Confirm the capture conditions that can materially affect the result:

- viewport, dimensions and device-pixel ratio;
- route, data, locale, theme and application state;
- fonts, assets and loading completion;
- animation frame, clock, random seed or camera where relevant;
- browser, renderer or device when platform differences matter.

If conditions differ, recapture when practical. Otherwise restrict the
conclusion to unaffected regions and state the limitation. Never crop away a
relevant discrepancy merely to improve a score.

## Inspect from broad to precise

1. View both full captures at the same scale. Check framing, completeness,
   hierarchy and large layout shifts.
2. Inspect aligned crops for small or dense features. Keep the full image as
   context.
3. Use overlays, flicker, side-by-side views, grayscale, edge maps or diff
   heatmaps when they help localize a discrepancy.
4. For each meaningful difference, describe the visible fact before judging
   it: missing content, displacement, clipping, contrast, typography, color,
   layering, blur, inconsistent state or artifact.
5. Judge that fact against the comparison contract, not against image age.

Treat generated metrics as telemetry. MAE, RMSE, mismatch ratios, edge-energy
changes, luminance deltas and region coverage can reveal where images diverge,
but they are symmetric or proxy measurements. A lower distance may preserve a
bad baseline; a higher distance may be the intended improvement. Thresholds
must be calibrated to the capture process and must not become unexplained
acceptance gates.

For a single image, inspect absolute warning signals such as unexpectedly low
content density, contrast or edge density, dominant blank color, transparency,
clipping and off-frame subjects. These signals prompt inspection; minimal,
dark or empty-state designs may trigger them legitimately.

## Control bias

When the decision is disputed, subtle or consequential, obtain an independent
inspection if that capability is available. Provide neutral labels, the target,
the captures and only the context required to interpret them. Do not disclose
which image is newer, preferred or expected to win.

An independent opinion is additional evidence, not a vote. Reconcile it with
the visible artifacts and the declared target.

## Report

Return:

- the comparison target and capture conditions;
- whether the captures are sufficiently comparable;
- material differences, each tied to a region or artifact;
- metrics used and what they can and cannot establish;
- a verdict: candidate closer, baseline closer, both wrong, or inconclusive;
- the next evidence or decision needed when inconclusive.

Separate visible facts from interpretations and preferences. Do not claim
pixel-perfect equivalence unless the method and tolerance actually establish
it.
