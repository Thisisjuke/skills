---
name: react
description: Apply React component, state, rendering, composition, lifecycle, accessibility, and ecosystem constraints when designing, implementing, debugging, or analyzing React applications and libraries; preserve the installed React and framework contract.
license: MIT
---

# React

Make React decisions from the installed version, renderer and framework, then preserve React's concurrent rendering and ownership contracts. This skill owns component, Hook, state and rendering semantics. It does not own generic implementation, build configuration, test strategy, performance methodology, routing, data infrastructure or native-platform APIs.

## Use version-matched authority

Read the lockfile and framework release first. Use the current major's official React reference and release/security posts for version-sensitive claims; use framework documentation for its Server Component, Server Function, bundling and cache integration. Treat canary APIs, proposals and third-party rules as conditional evidence, never as stable defaults.

As of the 19.2 documentation line, `Activity`, `useEffectEvent`, Actions and React Performance Tracks are documented, [React Compiler 1.0](https://react.dev/blog/2025/10/07/react-compiler-1) is stable, and View Transitions remain canary-only. Re-check [React versions](https://react.dev/versions) before encoding a claim because the copied skill must outlive this snapshot.

## Establish the runtime contract

Inspect manifests, lockfiles, imports, renderer, framework configuration, JSX/compiler settings, lint rules and adjacent components. Record only facts relevant to the task:

1. React and renderer versions, including framework-bundled or canary builds;
2. browser, server, native or custom-renderer execution;
3. client/server component and hydration boundaries;
4. compiler and `eslint-plugin-react-hooks` configuration;
5. state, data, styling, error and accessibility conventions;
6. public consumers and supported browsers or runtimes.

If sources disagree, stop version-sensitive advice and report the mismatch. Do not upgrade React, adopt a framework or add state/data libraries merely to reach a familiar solution.

## Keep render replay-safe

- Keep components and Hooks pure and idempotent for the same inputs. Render may be restarted, abandoned or repeated.
- Treat props, state, context, Hook arguments and values passed to JSX as immutable snapshots.
- Never start mutations, subscriptions, timers, analytics, navigation or imperative DOM work during render.
- Invoke components through JSX. Call Hooks only where the installed Rules of Hooks allow; `use` has version-specific exceptions that do not generalize to other Hooks.
- Define components at module scope unless a genuinely dynamic component type is required; recreating a component type resets its subtree.
- Tie keys to stable domain identity. Index or random keys corrupt state association when items reorder, insert or delete.
- Use `useId` for hydration-stable accessibility identifiers, not list keys or persisted business identifiers.

Strict Mode development replays expose unsafe work; do not suppress the signal or confuse development-only replay with a production duplicate request. Make the underlying operation safe or move it to its real owner.

## Design deep component interfaces

Expose the smallest semantic interface that lets consumers express supported behavior. Prefer composition, slots or explicit variant components when boolean combinations create invalid states. Do not expose internal DOM shape, styling-engine details or state machinery unless consumers must rely on them.

For controlled inputs, the rendered value comes from the controlling prop and every user change reports through the callback. For uncontrolled inputs, `defaultValue` or `defaultChecked` seeds state rather than controlling later updates. Do not switch modes during a component lifetime.

Keep state at the nearest stable owner. Store the minimum independent state and derive the rest during render. Use reducers for related transitions and invariants, not merely to move setter calls. Context is appropriate for cross-tree dependencies with a deliberate provider boundary; split unrelated update frequencies and stabilize provider values only when evidence requires it.

Changing a type, key or tree position changes state identity. Use that mechanism intentionally for whole-subtree resets; do not synchronize redundant state with an Effect when derivation or identity expresses the contract.

## Synchronize only at external seams

Use Effects to synchronize with browser APIs, subscriptions, connections or imperative widgets. User-caused work belongs in its event or Action, and render-derived values belong in render. Every Effect must define:

- the external resource or invariant it synchronizes;
- complete reactive dependencies;
- symmetrical setup and cleanup;
- cancellation or stale-result handling across replacement and unmount;
- behavior under reconnect or development replay.

Use `useEffectEvent` only on supported versions to read latest values from an Effect without making them reactive. It is not a dependency escape hatch and its result is called only from Effects. Use `useSyncExternalStore` for stores whose changes occur outside React so concurrent renders receive a coherent snapshot.

Refs hold imperative handles or values not used to choose rendered output. Do not read or write them during render except for supported initialization. A ref mutation must never substitute for state when the screen should update.

## Model async and concurrent UI

Separate urgent input from non-urgent rendering. A Transition can keep input responsive but does not make slow work cheaper, replace debouncing, or authorize stale commits. Suspense boundaries own fallback placement and reveal behavior; only supported Suspense data sources may suspend reliably.

For React 19 Actions, define pending, success, validation failure, exception, retry and duplicate-submission behavior. `useOptimistic` requires a deterministic optimistic projection plus reconciliation or rollback. `useActionState` actions may queue; check ordering and cancellation needs instead of assuming latest-wins behavior. Use `Activity` only when hidden content should retain state while Effects are managed by React; unmount when retained state or resources are not desired.

Use `react-actions-and-forms` when the primary outcome is an Action or form transaction, `useActionState`, `useOptimistic`, `useFormStatus`, progressive enhancement, ordering or cancellation. Keep the minimum lifecycle checks here because ordinary React work can still introduce a submission boundary.

Error boundaries handle render-tree failures within their documented scope. They do not replace errors returned from event handlers, async work, server operations or observability. Model recoverable domain errors as data and unexpected faults through the framework's error boundary and logging contracts.

## Guard client/server trust boundaries

Treat every Server Function as a remotely callable endpoint: authenticate, authorize the specific action, validate and bound untrusted arguments, protect side effects against replay where required, and return only data safe for the client. A `'use server'` marker grants no security property. Never rely on hidden UI, a disabled button or closure capture as authorization.

Keep secrets, privileged clients and server-only modules out of client-reachable graphs. Props crossing a server/client boundary are a serialization and disclosure boundary. Avoid raw HTML; when unavoidable, require a trusted sanitization policy appropriate to the content source. For projects using RSC packages, check the official [React security advisories](https://react.dev/blog/2025/12/03/critical-security-vulnerability-in-react-server-components) and the resolved patch version before feature work; framework mitigation is not proof that dependencies are patched.

Use `react-server-components` when the task's primary outcome involves RSC boundary placement, Server Functions, streaming, serialized data or an RSC migration. Keep these trust checks here because they remain required during ordinary React work even when that specialization is not selected.

## Use compiler and memoization deliberately

React Compiler can automatically memoize supported code, but adoption is a build and rollout decision. Keep code within the Rules of React, use its lint diagnostics, gate incrementally in existing apps and verify behavior before removing manual memoization. Without the compiler, add `memo`, `useMemo` or `useCallback` only for a demonstrated identity or computation cost. Their dependency arrays are correctness contracts, not tuning suggestions.

## Preserve accessible platform output

Choose native elements before ARIA, preserve accessible names, descriptions, relationships and form semantics, and keep keyboard and focus behavior intact through wrappers and portals. Forward IDs, refs and event props only as promised by the component API. Loading, disabled, invalid and live states must remain perceivable without color alone. React correctness does not prove accessibility; compose `accessibility` for a bounded evaluation.

## Verify and report

Run repository-native lint, type and behavioral checks, then exercise the changed states in the actual renderer. Include hydration/server-client behavior, Strict Mode diagnostics, cleanup, interrupted work, optimistic rollback, error recovery and accessible output when applicable. Report version, framework, renderer, compiler status, evidence, unsupported environments and unresolved security advisories.

## Composition boundaries

- `implementation`, `testing`, `review`, `performance`, `security` and `accessibility` own technology-neutral methods.
- `react-build`, `react-testing`, `react-review`, `react-performance`, `react-server-components` and `react-actions-and-forms` own focused React mechanics.
- Framework skills own routing, data protocols, cache semantics, Server Function transport and deployment.
- Storybook and design-system skills consume React components without becoming hidden dependencies.
