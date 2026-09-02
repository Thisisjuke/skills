---
name: design-system-visual-review
description: Review rendered design-system output for visual consistency, hierarchy, token application, variants, interaction states, themes, responsive behavior, and regressions using bounded visual evidence.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED screenshot-critique; CALLS WHEN NEEDED compare-screenshots"
---

# Design system visual review

Judge whether rendered output expresses the intended system consistently across a bounded state matrix. This skill owns design-system-specific interpretation of visual evidence. It does not own generic screenshot capture, product art direction, accessibility conformance or component implementation.

## Establish the matrix

Identify the authoritative reference, component and system versions, viewport, density, platform, browser, fonts, themes, brands, content lengths and interaction states. Require captures or a runnable surface for claims about rendering; code inspection alone cannot prove appearance.

Sample combinations by risk rather than rendering a blind cartesian product. Always include changed states and representative default, hover, focus-visible, active, disabled, loading, invalid, empty and overflow states when applicable.

Stabilize capture conditions before comparison: viewport and device scale, browser, font loading, locale and direction, theme, reduced-motion and forced-color settings, animation time, data, clock and network. A pixel difference under mismatched conditions is not yet a component regression.

## Inspect system consistency

- semantic token application rather than coincidental matching raw values;
- typography, spacing, sizing, alignment, radius, elevation and icon metrics;
- hierarchy and grouping across related components;
- stable layout under realistic and extreme content;
- state differentiation across light/dark/high-contrast themes, brands and user preferences;
- responsive behavior, localization and bidirectionality, clipping, wrapping, density and touch layouts;
- focus indicators, forced colors, contrast and reduced motion as visual evidence, without overstating accessibility coverage;
- regressions repeated across primitives versus isolated consumer overrides.

Compare relational rules as well as pixels: spacing rhythm, aligned baselines, component density, hierarchy and consistent state emphasis can fail even when no single raw value looks obviously wrong. Conversely, an intended token change can produce broad pixel churn without being a regression; trace repeated differences back to the authored decision.

## Delegate generic visual evidence

When a deliberately fresh, unprimed inspection is needed, call `screenshot-critique` with the ordered captures, scope and missing-evidence constraints. This skill retains design-system interpretation; `screenshot-critique` exclusively owns the fresh presentation-defect pass. When an intended target and actual capture must be aligned, call `compare-screenshots` with both artifacts and known capture conditions; it exclusively owns the aligned comparison and optional metrics. If a target is unavailable, continue only with directly supportable system observations and report the missing evidence lane. Neither call authorizes capturing new private surfaces or editing files. Do not call an active target or ancestor; report the cycle.

## Report

Return component/state/theme/viewport and capture conditions, evidence inspected, expected system rule, observed deviation, likely layer, affected variants or consumers, confidence and missing captures. Separate target mismatch, cross-component inconsistency and before/after regression. Group repeated symptoms under their shared primitive or token cause. When composed with `review`, let it own severity and disposition.
