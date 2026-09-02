---
name: storybook-testing
description: Design, implement, run, or diagnose Storybook story tests, play interactions, accessibility checks, portable-story reuse, and browser or CI execution against the installed Storybook toolchain.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED storybook-build"
---

# Storybook testing

Turn supported stories into trustworthy component-test evidence. This skill owns Storybook test APIs, story isolation, interaction execution and Storybook-specific CI mechanics. It does not own generic test strategy, renderer semantics or visual baseline approval.

## Establish the test stack

Inspect Storybook and framework versions, addons, preview annotations, runner or Vitest projects, browser provider, play functions, tags, accessibility parameters and CI commands. Storybook testing APIs change across releases; use imports and configuration matched to the lockfile.

For current Storybook releases, prefer the official Vitest browser integration when it supports the installed framework and builder because it executes portable stories and `play` functions in a real browser context. Preserve a supported test-runner or repository integration on older lines unless migration is requested. A DOM emulator is not equivalent evidence for browser layout, focus, pointer or accessibility behavior.

## Test the story contract

- Smoke-render affected stories and fail on unexpected render or console errors according to project policy.
- Query within the story canvas by role, name and user-visible state.
- Use the context-provided interaction utilities, await user actions and assertions, and group causal phases with the supported step API so asynchronous failures remain attributable.
- Reuse args, loaders, decorators and portable-story annotations rather than rebuilding a different fixture.
- Make time, randomness, network, storage and module mocks deterministic. Restore them with the installed per-story lifecycle and prove stories pass independently and in reordered or sharded execution.
- Test loading, resolved, empty, validation, error, retry, disabled and long-content states where they belong to the component contract. Test an interaction outcome, not merely handler call counts.
- Configure accessibility behavior deliberately: disabled checks need a reason, `todo` records non-blocking debt, and `error` can gate CI on supported versions. Automated rules cover only part of accessibility; retain keyboard, focus, zoom/reflow and assistive-technology evidence where risk requires it.
- Keep visual snapshots scoped and stable; animation, fonts, data and viewport must be controlled before approving a baseline.
- Use tags or project selection to express test scope, not hidden conditionals inside stories. Keep retries low and investigate flakiness instead of treating repetition as coverage.

Module mocks are evaluated through the preview module graph. Mock stable adapters, use spies when the real module can safely run, and do not assume a mock can be installed after the dependent module has already been evaluated. A mock proves story behavior against the substitute, not the real service contract.

Use `storybook-visual-testing` when capture coverage, browser or mode matrices, deterministic pixels, baseline history, diff triage or visual approval is the primary outcome. This skill retains only the guard that a visual layer must start from stable, isolated stories.

## Delegate edits and configuration

For authorized story, interaction or fixture changes, call `implementation` with the failing state, test command and allowed files; this skill retains test semantics and `implementation` owns safe mutation. When runner packages, addons, preview annotations, browser setup or scripts must change, call `storybook-build` with the installed versions and required test seam; it exclusively owns Storybook build configuration. If a target is unavailable, preserve runnable analysis and report the missing portion. Calls do not authorize installs, browsers with external access or CI changes. Never call an active target or ancestor; report the cycle.

## Verify and report

Run the smallest story selection before the suite, then the affected browser/project matrix and static build when CI consumes it. Report browser and provider, framework, test integration, selected stories, assertions, isolation order, accessibility or visual layers, retries, warnings and skipped environments. A passing story suite proves only the configured states and mocks.
