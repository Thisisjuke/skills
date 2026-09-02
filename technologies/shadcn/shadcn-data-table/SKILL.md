---
name: shadcn-data-table
description: Design, implement, review, migrate, or diagnose a shadcn/ui data table that integrates locally owned Table primitives with the installed TanStack Table major, including columns, state, server operations, selection, pagination, responsive rendering, accessibility, and feature-specific reuse.
license: MIT
metadata:
  skill-calls: "ALWAYS CALLS shadcn; ALWAYS CALLS tanstack-table; CALLS WHEN NEEDED implementation"
---

# shadcn data table

Build a product-specific integration between local shadcn/ui Table source and TanStack Table's headless engine. This skill owns the seam: component composition, feature boundary, state-to-control wiring and end-to-end table behavior. It does not own shadcn primitives, TanStack row-model semantics, backend queries, generic mutation safety or the product's visual direction.

The official shadcn [Data Table guide](https://ui.shadcn.com/docs/components/data-table) is intentionally a build-your-own pattern, not an installable universal `DataTable` component. Every table can have different data, operations and sources. Extract reuse only after identifying a stable shared contract.

## Map the integration before writing JSX

Resolve the exact application workspace, shadcn base/style and local `Table` source, Tailwind major, React/framework boundary and installed TanStack Table major. Do not assume that a current shadcn v9 example matches a project still using TanStack Table v8.

Define the table's row type, stable row ID, column inventory, operations, data size and data source. For sorting, filtering, pagination, selection, visibility, grouping, expansion, editing and row actions, name the client or server owner and the user-visible controls. Specify loading, empty, partial, stale, error, retry, permission and destructive-confirmation states.

Choose the reuse level deliberately:

- keep domain column definitions and actions near the feature;
- extract rendering controls such as sortable headers or pagination only when their behavior is truly shared;
- create a generic table shell around stable mechanics, not around every possible TanStack option;
- do not turn a feature table into an application-wide abstraction before a second credible consumer exists.

## Obtain both owning contracts

After the boundary map and before implementation or review conclusions, always call `shadcn` with the exact workspace, local Table and control components, selected base/style, Tailwind version, expected DOM and local divergences. Request analysis only at this stage. `shadcn` exclusively owns the local component APIs, composition and base-specific behavior; this skill retains the table integration. If unavailable, stop the affected workflow and report the mandatory configuration gap.

Also always call `tanstack-table` with the installed package/adapter version, row and column types, requested features, state ownership, client/server operation map, data scale and stable-ID needs. Request analysis only at this stage. `tanstack-table` exclusively owns version-matched feature, state and row-model semantics; this skill retains control wiring and product behavior. If unavailable, stop the affected workflow and report the mandatory configuration gap.

Do not reproduce an absent target's full workflow as a fallback. Reconcile the two results at their seam and surface contradictions before mutation.

## Wire one coherent data pipeline

Define columns with stable IDs, typed accessors and explicit display columns for selection or actions. Use the installed major's APIs: v8 `useReactTable` examples and v9 `useTable` plus registered features are not interchangeable. Keep data and column identities stable according to React and the adapter contract.

For client processing, pass the complete dataset required for every enabled operation and register only needed row models. For server processing, control the relevant state, use manual operation modes, include every state input in the query/cache key, provide trustworthy totals and prevent stale responses from replacing current results. Never combine server pagination with client-only filtering or sorting unless the UI clearly promises page-local behavior.

Use stable domain row IDs so selection and actions survive ordering and refreshed data. Define whether “select all” means visible rows, filtered loaded rows, all loaded pages or the complete server result. For the last case, use an explicit server selection model rather than materializing an unbounded ID set in the browser.

Reset or clamp page state when filters, page size, data availability or permissions change. Preserve state in the URL only when navigation, sharing or restoration requires it; parse and bound external values before giving them to the table.

## Render the behavior through local primitives

Use the local shadcn `Table`, buttons, inputs, menus, checkboxes and feedback components through their actual APIs. Render header groups and visible cells from TanStack models, and keep domain formatting in explicit cell renderers. Do not bypass semantic tokens with raw colors or override shared primitive behavior for one table.

Provide a meaningful caption or surrounding accessible name, header associations, sort direction, selection labels, row-action names and status announcements. A sortable header must be an operable control; a visual chevron alone is insufficient. Preserve focus when menus close, data refreshes or pages change. Avoid nested interactive controls and ensure row click behavior does not steal checkbox, link or menu activation.

Choose semantic `<table>` behavior by default. Adopt an ARIA grid only when spreadsheet-like navigation is a product requirement and the complete keyboard model is implemented. For narrow screens, prefer deliberate overflow, priority columns or an alternate compact presentation; hiding columns must not hide the only route to required information or actions.

Model skeleton/loading rows without presenting them as real data. Distinguish no results from no data and from a failed request. Keep pagination and selected-count labels accurate for the data actually known.

## Scale only from evidence

Profile row processing, React rendering, DOM/layout and network transfer separately. Client pagination, server pagination and virtualization solve different constraints. Add TanStack Virtual or another virtualization engine only after evidence shows mounted DOM is the bottleneck and specify row measurement, overscan, focus, scrolling and test behavior. It remains a separate dependency and is not supplied by TanStack Table.

Avoid rendering unstable inline component types or changing keys on each table update. Narrow table-state subscriptions where the installed major permits it. A large generic `DataTable` prop surface is not a performance strategy and usually increases invalid combinations.

## Delegate the combined mutation

When authorized source, tests or call sites must change, call `implementation` only after both mandatory domain calls have returned and the integration contract is coherent. Pass their version/base findings, target files, row/column and state contracts, expected DOM and states, preserved local behavior and verification commands. This skill retains cross-library decisions; `implementation` exclusively owns mutation safety and complete diff accounting. If unavailable, return the integration design and stop mutation.

Calls do not authorize dependency installation, TanStack major migration, registry addition, backend changes, network access, data export, overwrite, deployment or baseline approval. Do not call a target already active in the invocation chain or an ancestor; report the cycle and preserve all completed boundary evidence.

## Verify and report

Test real combinations with representative data: sorting/filtering/pagination, selection across changes, URL restoration, delayed and out-of-order server responses, loading/empty/error recovery, permissions, long localized content, narrow layout, keyboard use and screen-reader output. Verify the production bundle and rendering path when features or dependencies change.

Report workspace, shadcn base/style and local component versions, Tailwind and TanStack versions, feature registration, client/server operation map, stable-ID and selection semantics, reusable versus feature-owned pieces, commands and runtime evidence, and any backend, browser, scale or accessibility path not exercised.

## Composition boundaries

- `shadcn` owns local primitives, base-specific composition and component updates.
- `tanstack-table` owns headless features, state and row-model correctness.
- `tailwindcss` owns non-trivial utility and theme semantics when separately selected by `shadcn`.
- Framework, fetching and backend skills own their respective runtime, cache and query contracts.
