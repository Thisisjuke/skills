---
name: react-server-components
description: Design, implement, review, or diagnose React Server Component and Server Function features; use for server/client boundaries, directives, serialization, streaming, hydration, secure mutations, RSC dependency advisories, or framework integration.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED react-build"
---

# React Server Components

Make the server/client execution boundary explicit before changing an RSC feature. This skill owns React Server Component and Server Function semantics, boundary placement, serialized data exposure, secure mutation requirements and RSC-specific evidence. It does not own framework routing, caching, revalidation, transport internals, generic build mechanics, deployment or generic mutation safety.

## Establish version and framework authority

Inspect the lockfile, resolved `react`, renderer, framework and `react-server-dom-*` packages, framework configuration, route or entry boundary, directives and production deployment model. Use the matching framework documentation for supported RSC integration and the official [React Server Components](https://react.dev/reference/rsc/server-components) and [Server Functions](https://react.dev/reference/rsc/server-functions) references for React semantics.

React 19 treats the component and function APIs as stable, but the APIs used to implement an RSC bundler or framework may change between React minors. Framework authors and direct transport integrators therefore pin the compatible React line; application authors preserve the versions selected by their supported framework. Treat canary behavior as version-specific and never copy it into a stable contract by default.

Determine whether the feature renders at build time, per request or through a hybrid framework path. Record which cache, request, authentication and deployment behavior comes from the framework instead of attributing it to React.

## Draw the execution and disclosure boundary

Map each affected module and data flow before editing:

- Server Components may read server-owned resources and may render Client Components, but they do not own browser state, event handlers or effects.
- A `'use client'` directive establishes a client module boundary. Everything reachable through that client import graph must be safe and viable in the client bundle.
- A Client Component must not directly import a Server Component implementation. Pass server-rendered elements or serializable data through a framework-supported composition boundary.
- Values crossing from server to client follow the RSC serializer supported by the installed versions; do not reduce this to “JSON only” or assume an arbitrary class, closure or privileged object is safe.
- Send the minimum data the client needs. Serialization is also a disclosure, transfer and hydration-cost boundary.
- Keep secrets, database clients, filesystem access and server-only modules out of client-reachable graphs. Enforce this with the framework's supported server-only guard when one exists.

Choose Client Components at the smallest coherent interactive boundary, not at every leaf and not around an entire route for convenience. Moving a directive can change bundle size, state ownership, serialization requirements and available APIs; review the whole reachable graph.

## Render and stream deliberately

Distinguish the RSC render from HTML server rendering. RSC produces a serialized component payload; SSR produces HTML, and hydration attaches client behavior to the Client Component portions of that HTML. A framework may combine and stream these phases, but Server Components themselves do not become interactive through hydration.

Start independent server reads without accidental waterfalls and await them where ownership and reveal order require. Use framework cache and request-deduplication semantics only after reading their versioned contract; never introduce shared mutable module state as a request cache.

Place Suspense boundaries around meaningful reveal units. Define fallback, empty, partial, error and retry behavior, and distinguish server render time, streamed reveal, HTML hydration and client interaction. A fast first chunk does not prove that the feature becomes usable promptly.

Treat hydration mismatches as correctness failures. Keep time, randomness, locale, browser-only globals and request-specific data deterministic across any server/client render pair, or isolate them behind an intentional client boundary.

## Secure every Server Function

Treat a Server Function reference as a remotely invocable endpoint, including when the UI does not visibly expose it:

- authenticate the caller and authorize the specific resource and operation inside the function;
- validate, normalize and bound all arguments, including identifiers captured from surrounding scope;
- protect non-idempotent effects against replay or duplicate submission where the operation requires it;
- define pending, validation, conflict, failure, retry and success results without returning private data or implementation details;
- apply the framework's CSRF, origin, body-size, rate-limit and request-context controls rather than inventing transport assumptions;
- log and observe failures without recording credentials, secrets or sensitive payloads.

`'use server'`, encrypted closures, hidden controls and client-side route guards provide no authorization. Check current React and framework advisories against the resolved dependency graph before feature work. RSC vulnerabilities have affected applications that supported Server Components even when they declared no application Server Function.

Use `react-actions-and-forms` when the client-visible pending, optimistic, ordering, cancellation or progressive-enhancement lifecycle is a primary outcome. Keep remote authorization and validation in the Server Function. Cache invalidation and post-mutation navigation follow the installed framework contract.

## Verify the real integration

Use unit tests for extracted pure domain behavior, then a framework-provided integration fixture for the RSC protocol. Exercise direct unauthorized and malformed requests as well as the intended UI. Verify:

- server/client graph and absence of server-only code or secrets in client artifacts;
- supported serialization and hydration without suppressed diagnostics;
- streamed loading, empty, partial, error and recovery states;
- authentication, resource authorization, validation, duplicate execution and safe error disclosure;
- behavior before hydration or without client JavaScript when progressive enhancement is promised;
- current patched RSC and framework versions.

A DOM-only mock of a Server Function does not prove transport, security, cache or integration behavior. Report React/framework versions, execution boundary, serialized data, security controls, commands and environments actually exercised, and any framework behavior left unverified.

## Delegate bounded changes

When authorized component or Server Function source must change, call `implementation` after establishing the boundary, threat conditions and verification seam. Pass the affected modules, server/client map, preserved framework contract and evidence; this skill retains RSC and security decisions while `implementation` exclusively owns mutation safety and diff accounting. When directives, RSC dependencies, bundler integration, entrypoints or client/server artifacts must change, call `react-build` with the resolved versions, graph evidence and expected artifact; it exclusively owns React build mechanics and may delegate its mutation. If a target is unavailable, preserve the boundary analysis and stop only the affected mutation or build portion. Calls grant no dependency installation, network, credential, publication or deployment authority. Do not call a target already active in the invocation chain; report the cycle.

## Composition boundaries

- `react` owns general component, Hook and rendering semantics.
- `react-actions-and-forms` owns the client-visible Action and form transaction lifecycle.
- `react-testing`, `react-review` and `react-performance` add their specialized evidence without replacing this boundary model.
- Framework skills own routing, cache keys, revalidation, request transport and deployment.
- `security` owns a broader threat assessment when the feature requires one.
