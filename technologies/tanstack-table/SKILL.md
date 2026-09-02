---
name: tanstack-table
description: Design, implement, review, migrate, or diagnose TanStack Table data grids across supported adapters, with version-matched columns, features, state, row models, client or server data operations, selection, pagination, virtualization boundaries, and headless rendering.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation"
---

# TanStack Table

Treat TanStack Table as a headless state and row-model engine. This skill owns its column model, registered features, table state, row-processing pipeline and client/server data-operation contract. It does not own visual components, markup styling, backend query implementation, data fetching, virtualization engines or generic mutation safety.

## Bind guidance to the installed major

Inspect the manifest and lockfile, framework adapter, exact TanStack Table version, renderer version, column definitions, table factory or hook, registered features, controlled state and data source. Follow documentation for that exact major.

TanStack Table v9 became stable in August 2026 and replaced important v8 setup APIs. V8 commonly uses `useReactTable` with `get*RowModel` options; v9 uses `useTable`, explicit `tableFeatures()` registration and `create*RowModel()` factories on the feature definition. Never mix examples across those majors or upgrade as a side effect of feature work. Re-check the official [overview](https://tanstack.com/table/latest/docs/overview), adapter guide, [migration guide](https://tanstack.com/table/latest/docs/framework/react/guide/migrating) and [releases](https://github.com/TanStack/table/releases) before version-sensitive changes.

The table core supports several framework adapters. Keep core data semantics distinct from adapter reactivity. React memoization advice, Vue refs, Svelte runes and other adapter mechanisms are not portable substitutes for one another.

## Define the table contract before its configuration

List the row type, stable domain identifier, column meanings, display-only columns and supported operations. For each operation—sorting, filtering, grouping, aggregation, expansion, selection, pagination, pinning, sizing or visibility—define:

- whether it is available and where the user controls it;
- its initial state and reset behavior;
- whether the client or server owns the complete operation;
- how it composes with other operations and permissions;
- how it is represented in URL, cache or application state when persistence is required;
- loading, empty, partial, stale, error and retry behavior.

Do not start with every stock feature. In v9, register only the features, row-model factories and named filter, sort or aggregation functions the table uses. In v8, preserve the installed configuration model. Headless APIs do not decide product behavior or accessible markup for you.

## Keep data and column identity stable

Use a durable `getRowId` based on domain identity when selection, expansion, editing or reconciliation must survive sorting, filtering, pagination or refreshed data. Default positional row IDs are not stable business identifiers.

Give columns stable and unique IDs. Accessor values used by built-in sorting or filtering should be primitives with defined null, locale and case behavior; use explicit functions when domain ordering differs. Display columns do not pretend to be data accessors. Keep column metadata typed and bounded to real renderer needs.

Respect the adapter's reference and reactivity contract for data, columns and options. In React, avoid recreating data or columns in a feedback loop and preserve identities where subscriptions or memoization rely on them. Do not cargo-cult memoization around every value; prove which identity is semantic or performance-sensitive.

## Own state at one clear boundary

Control only the state that another application boundary needs. When a state slice is controlled, pass both its current value and the corresponding change handler; a handler without the state value can freeze or desynchronize behavior. Avoid simultaneously setting the same value as initial and controlled state.

In v9, state is backed by TanStack Store and can be read or subscribed to through the APIs supported by the adapter. Subscribe to the narrowest useful slice when render cost matters. Hoisting all rapidly changing table state into a broad application store can create more work and a harder consistency contract than internal state.

Define reset rules when filters, sorting, data set, permissions or page size changes. Server request keys must include every server-owned table state input. Debounce only inputs whose product semantics allow it, cancel or supersede stale requests, and prevent older responses from replacing newer state.

## Keep the row pipeline coherent

The client row-model pipeline conceptually processes core rows through enabled filtering, grouping, sorting, expansion and pagination stages. Add a client row model only when the browser owns that complete operation over the available dataset.

For server-owned sorting, filtering, grouping or pagination, keep the feature state and APIs but use the installed major's manual-operation contract and omit the corresponding client processor. TanStack Table describes the requested state; the backend and fetching layer must implement it. Never filter only the current server page, sort only loaded infinite pages or report counts as global unless that is the explicit product behavior.

Provide trustworthy row or page counts so navigation and selection summaries are honest. Under manual pagination, selected-row state may contain IDs not present in the current data, while selected-row models can only materialize loaded rows; keep bulk operations based on explicit IDs or a server-side selection contract.

## Separate processing from rendering scale

Pagination reduces the displayed or fetched row set according to its owner. Virtualization reduces mounted DOM for rows already available to the client. TanStack Table does not include virtualization; the official examples pair it with TanStack Virtual. Use it only when measurements justify the added scroll, measurement, focus and testing complexity.

When virtualizing, index into the current visible row and column lists after table processing, keep stable keys, preserve scroll and focus semantics, and recompute against sorting, filtering, grouping or visibility changes. Virtualization cannot make an unbounded dataset cheap to fetch or process.

## Render accessible, product-owned UI

Choose semantic table markup for genuinely tabular relationships and a grid pattern only when its richer keyboard interaction is fully specified. Render headers, captions, sort state, selection labels, row actions, focus, loading and empty states through the chosen UI layer. TanStack Table's headless model provides no accessible DOM by itself.

Keep destructive or privileged row actions authorized at the operation boundary, not merely hidden by column visibility. Bound exported or bulk-selected data to the user's actual permissions.

## Delegate mutation safely

When authorized table configuration, adapter code, tests or call sites must change, call `implementation` after capturing the installed major and adapter, row and column contracts, feature set, ownership of each operation, state persistence, stable IDs, expected markup behavior and verification commands. This skill retains TanStack Table decisions; `implementation` exclusively owns mutation safety and diff accounting. If unavailable, return the design or diagnosis and stop mutation.

Calls do not authorize package installation, major migration, backend changes, network access, data export, deployment or broad UI redesign. Do not call `implementation` if it is active in the invocation chain or an ancestor; report the cycle and preserve evidence.

## Verify and report

Exercise operation combinations, not only isolated toggles: filtering plus pagination, sorting plus selection, refreshed data plus controlled state, permissions plus row actions and server errors plus retries. Verify stable identities, request ordering, counts, reset rules, keyboard and screen-reader output, narrow layouts and representative data scale. Profile before claiming a performance improvement.

Report the exact package and adapter versions, v8/v9 API family, registered features and processors, client/server ownership map, state and ID contracts, data scale, tests and runtime evidence, and any backend, browser or accessibility path not exercised.

## Composition boundaries

- `shadcn-data-table` owns integration with local shadcn/ui Table primitives.
- Framework skills own renderer reactivity, server/client boundaries and routing.
- Data-fetching libraries and the backend own transport, caching and query execution.
- `performance`, `accessibility`, `testing` and `security` add broader evidence when independently selected.
