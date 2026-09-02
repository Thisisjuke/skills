---
name: storybook-visual-testing
description: Configure, design, run, or diagnose visual regression testing for Storybook stories; use for capture coverage, deterministic rendering, viewports, themes, browsers, baselines, pixel-diff triage, approvals, flaky snapshots, or visual-test CI policy.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED storybook-build"
---

# Storybook visual testing

Turn stable Storybook stories into reviewable visual-regression evidence. This skill owns Storybook-specific capture coverage, baseline meaning, diff triage and approval policy. It does not own visual design decisions, story semantics, generic screenshot comparison, component behavior, Storybook build mechanics or deployment.

## Establish the capture contract

Inspect the resolved Storybook version and framework, visual provider or local runner, browser and operating-system image, viewport and device scale, preview annotations, fonts and assets, story parameters, current baselines, branch model, CI command, retries and approval roles. Use the installed version's official [visual testing documentation](https://storybook.js.org/docs/writing-tests/visual-testing) and the provider's versioned documentation. Do not install or assume Chromatic merely because Storybook documents its maintained integration.

Record the unit of comparison. A useful baseline identifies at least the story, story inputs, global or mode values, viewport, browser engine, capture configuration and an approved source revision. Provider configuration or browser-image changes can alter that identity even when component code is unchanged.

## Select risk-based coverage

Start from meaningful executable stories. Include states whose appearance can regress independently: content extremes, validation and error states, loading or skeletons, overlays, interaction results, responsive breakpoints and system themes. Add browsers, locales, text directions, density, zoom or media preferences only when the support contract or risk justifies them.

Use a small named set of modes for valuable combinations such as theme, viewport and locale. Avoid the full cartesian product. Preserve independent baselines when modes represent different contracts, and document why a story or mode is excluded instead of silently disabling unstable coverage.

A visual test proves only captured pixels in its configured environment. It does not prove keyboard behavior, accessible names, DOM semantics, business results, performance or compatibility with an uncaptured browser. Pair it with the appropriate behavioral or accessibility evidence rather than inflating its claim.

## Make captures deterministic

Before approving any baseline, control inputs that can change pixels:

- replace live network and mutable production data with stable fixtures;
- freeze time, timezone, locale and randomness where they affect output;
- reset storage, caches, generated identifiers and cross-story mutable state;
- serve versioned images and fonts reliably, then wait for required assets and layout to settle;
- disable or deliberately pause CSS, SVG, video and JavaScript animation at a named frame;
- set viewport, browser, device scale, color scheme, reduced motion and forced colors explicitly where relevant;
- drive interactions through the story's supported play or preparation seam and capture only after the observable state is ready.

Do not use an arbitrary long delay as the primary readiness signal. Wait for a user-visible condition or provider-supported lifecycle. Mask or ignore a region only when its variability is intentional, bounded and recorded; never hide an unknown diff to make CI green.

When a failure is intermittent, reproduce the same story and capture tuple repeatedly, then vary one suspected source at a time. Late fonts, animation frames, unstable resources, random data, environment drift and incomplete interaction setup are different causes and need different fixes.

## Treat baselines as approved evidence

The first successful capture is a candidate, not automatically a correct baseline. Review it against the component and design contract, including clipped content, overflow, focus state, overlays and asset loading. Store or reference baselines through the selected provider's supported branch and history model.

For each changed snapshot, classify the result:

1. intended product or system change;
2. unintended visual regression;
3. capture-environment or fixture instability;
4. missing, obsolete or newly required state;
5. inconclusive evidence requiring another browser, viewport or human review.

Approve only intended output after reviewing all affected modes and browsers. Fix regressions in their owning source; repair unstable capture inputs before recapturing; add missing stories through the story-authoring workflow. Never bulk-accept because a large token, font or dependency change produced many diffs. Baseline approval is a review decision separate from code mutation and does not authorize merging or release.

## Design a safe CI policy

Run the smallest affected selection for fast feedback when the provider can prove dependency impact, but retain a full periodic or merge-boundary run to detect graph and global-style changes. Define branch-baseline inheritance, behavior for new and deleted stories, required checks, retries, failure artifacts and who may approve changes.

Keep provider tokens in the CI secret store and out of stories, preview bundles, logs and static artifacts. Pin actions and capture dependencies according to repository policy. A skipped build, missing baseline, infrastructure error and reviewed visual change are distinct outcomes; do not collapse them into a passing status.

## Delegate bounded changes

When authorized components, stories, fixtures, interactions or tests must change to express or repair a visual state, call `implementation` with the failing story and capture tuple, expected appearance, affected paths and reproduction command. This skill retains coverage and baseline decisions; `implementation` exclusively owns mutation safety and diff accounting.

When addons, preview annotations, builders, static assets, Storybook scripts or capture integration must change, call `storybook-build` with the installed versions, provider contract, required artifact and verification command. It exclusively owns Storybook build and addon mechanics; this skill retains capture and approval policy. If either target is unavailable, preserve valid diagnosis and stop only its affected mutation or configuration portion.

Calls grant no install, external-browser, network, credential, baseline approval, CI mutation, publication or deployment authority. Do not call a target already active in the invocation chain; preserve evidence and report the cycle.

## Verify and report

Run a single changed story first, then its risk-relevant modes and browsers, then the required CI selection. Confirm direct story rendering and the same static or hosted artifact CI captures. Re-run suspected flakes without accepting changes and inspect provider logs, screenshots and diffs.

Report Storybook, framework, provider and browser versions; story and mode coverage; deterministic controls; baseline source and branch; diff classification; approvals actually obtained; commands, retries and artifacts; skipped combinations; and what behavioral, accessibility or human visual evidence remains outside the result.

## Composition boundaries

- `storybook` owns story authoring, args, decorators, loaders and play semantics.
- `storybook-testing` owns interaction, render and accessibility-test execution in the Storybook toolchain.
- `storybook-build` owns framework, builder, addon and static-artifact mechanics.
- Visual-review skills may compare or critique images, but they do not own Storybook baseline history or approval policy.
