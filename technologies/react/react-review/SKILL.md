---
name: react-review
description: Review React code for rendering, component API, state, Hook, effect, identity, accessibility, server-client, and maintainability defects against the project's installed version and framework.
license: MIT
---

# React review

Provide a React-specific defect lens ordered by production impact. This skill does not define generic review scope or finding format and does not repeat the full React development guide.

## Fix the target

Establish React, renderer and framework versions, resolved RSC packages, runtime boundaries, lint/compiler configuration, public consumers and nearby conventions. Review changed behavior and affected call sites, not isolated syntax. Treat canary, experimental and framework-bundled APIs as version-specific.

## Block security and trust failures first

- Flag unpatched RSC transport versions covered by official advisories when the graph can reach them.
- Treat every Server Function as an endpoint: require action-specific authentication, authorization, input validation, bounded work and safe return data.
- Trace server-only modules, credentials and private data across client graphs and serialized props.
- Flag raw HTML or URL/script sinks fed by untrusted data without an established sanitization policy.
- Treat client-side visibility, route guards and disabled controls as UX only, never authorization.

## Inspect high-risk seams

- render purity, immutable inputs and behavior under replay or interruption;
- Hook ordering, complete dependencies and version-specific `use` exceptions;
- Effects used for derived state or event work, missing cleanup, reconnect failures and stale async completion;
- controlled/uncontrolled drift and state that resets or persists under the wrong identity;
- component APIs with invalid prop combinations, shallow wrappers, hidden DOM coupling or broken composition;
- index/random keys, nested component definitions and IDs misused as keys;
- context breadth, unstable provider values and incoherent external-store snapshots;
- server/client imports, non-serializable props, hydration mismatches and browser globals during server render;
- Action pending/error/duplicate behavior and optimistic state without rollback;
- Suspense boundaries that hide too much, flicker or rely on unsupported data sources;
- error boundaries mistaken for event, async or server error handling;
- semantic HTML, accessible names, focus, keyboard behavior and prop/ref forwarding;
- manual memoization unsupported by measurement or conflicting with compiler rollout.

## Judge feature completeness

Trace at least default, loading/pending, empty, validation failure, unexpected failure, success, retry/cancel and disabled states that the feature can reach. Verify cleanup on replacement and unmount. Check that errors and pending state belong to the operation that produced them rather than a stale request. A happy-path component review is incomplete when the feature crosses server, cache, provider or portal boundaries.

Require a concrete failure path, affected consumer or measurable risk. Do not report stylistic preference as a defect, demand migration to a preferred state/data library, ban all Effects, or assume every rerender is harmful. Third-party checklists are investigation prompts; official version-matched semantics and repository evidence decide the finding.

## Report

For each finding, give location, React mechanism, trigger, consequence and smallest safe direction. Separate demonstrated defects from risks needing runtime or bundle evidence. State React/framework/compiler versions and missing environments. When composed with `review`, let it own severity and final disposition.
