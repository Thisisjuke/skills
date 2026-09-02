---
name: react-testing
description: Design, implement, run, or diagnose React component and Hook tests through the project's actual renderer, runner, interaction, accessibility, scheduling, and integration contracts.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED react-build"
---

# React testing

Create test evidence for observable React behavior under its actual renderer and scheduler. This skill owns component, Hook, provider, hydration, Action and cleanup mechanics. It does not own generic test strategy, production behavior, build topology, browser automation strategy or framework-specific end-to-end workflows.

## Establish the test contract

Inspect the installed React version, compiler mode, test runner, DOM/native/real-browser environment, rendering library, setup files, fake timers, mocks and repository commands. Decide which public seam is under test: rendered UI, custom Hook, component API, provider/store adapter, hydration boundary or Server Function integration. Preserve the current stack unless migration is requested.

Prefer a supported renderer and user-level library. `react-test-renderer` and shallow-rendering patterns are deprecated in modern React; do not introduce them into new coverage. Use direct `act` only when the chosen helpers do not already wrap the operation, and use its async form because scheduling can cross async boundaries.

## Test behavior through supported surfaces

- Assert what a user or consumer observes: roles, names, content, focus, state transitions, callbacks and public Hook results.
- Render through the real public provider composition needed by the contract; keep a shared test renderer instead of rebuilding application internals in every case.
- Drive interactions with the project's user-level event mechanism and await observable completion rather than timers or arbitrary flush calls.
- Cover loading, empty, validation error, unexpected error, success, retry, disabled and cancellation states that the feature exposes.
- Verify controlled/uncontrolled contracts, state preservation and intentional key-based reset.
- Exercise Effect setup and cleanup under replacement, unmount and development replay; assert resource outcomes rather than Hook call counts.
- For external stores, test snapshot changes and subscription cleanup through the public store seam.
- Use implementation-detail assertions only when that detail is a supported consumer contract.

## Cover React 19 and concurrent behavior

For Actions, assert pending feedback, double submission, ordering, validation results, thrown failures, optimistic reconciliation and rollback. Do not make tests pass by forcing internal scheduler queues. For Transitions and Suspense, assert the visible old, pending, fallback and revealed states permitted by the contract; never depend on exact internal render counts.

For `Activity`, test whether hidden content retains state and whether its Effects disconnect/reconnect as promised. For `useEffectEvent`, test the external synchronization outcome with latest values, not the identity of the returned function. Run compiler-enabled tests through the same transform used by the application when compiler behavior is relevant.

Hydration requires server markup plus client hydration in a capable environment. Fail on unexpected hydration diagnostics; do not suppress mismatches to make a test green. RSC and Server Functions belong in a framework-provided integration fixture because a DOM-only test cannot reproduce their protocol or security boundary.

## Keep doubles at trust boundaries

Mock network, clock, randomness, storage and third-party systems at stable adapters. Avoid mocking React, internal Hooks or child components merely to observe calls. Reset global state and fake timers between tests. A mock of a Server Function does not prove authentication, authorization, validation or patched transport dependencies; keep a framework integration test for that boundary.

Keep renderer, `act` and hydration warnings visible. Treat leaked timers/listeners, updates after cleanup, duplicate React copies and environment-specific DOM behavior as contract failures rather than suppressing them.

## Delegate changes

When tests or fixtures must be edited, call `implementation` with the target behavior, fixtures, runner command and failure evidence. This skill retains React test mechanics; `implementation` owns safe mutation. When dependencies, JSX transforms, setup files or test scripts must change, call `react-build` with the current build evidence and desired test seam; `react-build` exclusively owns that configuration and may delegate mutation. If either target is unavailable, keep valid test analysis and report the missing capability before its portion. No call authorizes installs, network services or shared fixtures. Do not call an active target or ancestor; report the cycle.

## Verify and report

Run the narrow test first, then affected suites and a representative production-like browser or hydration lane when required. Report React/compiler versions, environment, renderer, runner, seam, visible states, warnings, deterministic cleanup, commands and skipped integration surfaces. Passing tests prove only the exercised behavior.
