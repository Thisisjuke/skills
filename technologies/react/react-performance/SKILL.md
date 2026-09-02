---
name: react-performance
description: Analyze React rendering, scheduling, bundle, loading, interaction, and runtime performance with version-matched profiling and before-and-after evidence.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED react-build"
---

# React performance

Attribute user-visible delay before optimizing React. This skill owns React render, commit, scheduling, state propagation, hydration and component-boundary evidence. Generic workload validity belongs to `performance`; bundle mechanics belong to `react-build`.

## Establish representative conditions

Record React/compiler/framework versions, development versus production mode, device/browser, route, data size, cache state, interaction and target metric. Capture a reproducible baseline tied to a user action: startup, navigation, input, reveal or update. Development Strict Mode, profiling builds and synthetic throttling change observed work; label them and confirm important results in a production build.

## Diagnose the mechanism

Use React DevTools Profiler and React Performance Tracks where supported, browser traces, framework diagnostics, network waterfalls and bundle evidence. Correlate React commits with browser tasks and user timing. Distinguish:

- frequent renders from expensive renders and from DOM/paint cost;
- render work from commit, layout, style, paint, network and unrelated JavaScript;
- necessary propagation from unstable identities, broad context or external-store subscriptions;
- server response, streaming, hydration and client navigation;
- module transfer from parse/evaluation, initialization and runtime cost;
- one slow sample from a stable distribution under representative conditions.

## Choose the intervention from evidence

Attack critical waterfalls before micro-optimizing renders: start independent work together, defer awaits until their branch needs them, and place Suspense boundaries where progressive reveal improves the experience without causing fallback churn. Do not make requests concurrent when ordering, load or side effects forbid it.

Reduce state and Effect cascades before adding memoization. Narrow subscription scope, split context by update frequency, keep transient non-rendering values in refs and virtualize or use `content-visibility` only when long-list evidence warrants it. Stable keys preserve work; bad keys defeat memoization and corrupt state.

React Compiler 1.0 changes the memoization baseline. Confirm whether it processes the file and use its diagnostics before layering manual `memo`, `useMemo` or `useCallback`. Retain manual memoization when it is a semantic identity contract or measurement still proves value. Never trade stale closures, incorrect dependencies or behavior for a faster trace.

Transitions improve responsiveness by deprioritizing work; they do not reduce total work. `Activity` can avoid remount cost while retaining state, but hidden trees still consume memory and their lifecycle must fit the feature. Code splitting helps only when a boundary removes meaningful initial transfer/evaluation without creating a navigation waterfall.

For SSR/RSC, minimize client serialization and duplicated data, parallelize independent server reads under framework cache semantics, and measure time-to-first-byte, stream reveal, hydration and interaction separately. Never introduce shared mutable module state to cache request data. Authenticate Server Functions before expensive work as well as for correctness.

## Intervene through the right owner

For authorized component or state changes, call `implementation` with the causal profile, target metric, preservation constraints and reproduction steps. This skill retains the performance hypothesis; `implementation` owns mutation safety. For chunking, dependency, source-map or bundler changes, call `react-build` with bundle evidence and intended artifact change; it exclusively owns build mechanics. If unavailable, report the supported diagnosis without performing that change. Calls do not authorize installs or deployment. Do not call an active target or ancestor; report the cycle.

## Prove the result

Repeat the same workload and compare distributions or representative traces, not one favorable sample. Check correctness, pending behavior, memory, bundle/network trade-offs and low-end devices. Report environment, cache state, profiler overhead, causal chain, before/after evidence and unmeasured production differences. Do not claim a React optimization when the gain came from changed data, cache or network conditions.
