---
name: react-actions-and-forms
description: Design, implement, review, or diagnose React Action and form workflows; use for action props, useActionState, useOptimistic, useFormStatus, progressive enhancement, pending and error states, duplicate submissions, ordering, cancellation, or optimistic reconciliation.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED react-server-components"
---

# React Actions and forms

Model a submission as a user-visible transaction with explicit input, pending, result and recovery semantics. This skill owns React Action, form Action and optimistic UI behavior. It does not own remote authorization, Server Function security, framework routing or revalidation, generic mutation safety, or backend transaction design.

## Establish the installed contract

Inspect the resolved React and renderer versions, framework, client/server boundary, form markup, Action sources, validation, state ownership, error boundaries and tests before advising or editing. Use version-matched official references for [`useActionState`](https://react.dev/reference/react/useActionState), [`useOptimistic`](https://react.dev/reference/react/useOptimistic), [`useFormStatus`](https://react.dev/reference/react-dom/hooks/useFormStatus), [`<form>`](https://react.dev/reference/react-dom/components/form) and [`useTransition`](https://react.dev/reference/react/useTransition).

Do not retrofit React 19 APIs into an older line, or assume a framework implements progressive enhancement, request ordering, navigation or cache updates exactly as React documents its primitives. Preserve the repository's supported form and data libraries unless replacing them is explicitly in scope.

## Choose the correct submission seam

Distinguish the mechanisms before combining them:

- An Action is a function executed in a Transition, including a function supplied to an Action prop. It coordinates non-urgent work and pending UI; it is not necessarily remote.
- A Server Function is a framework-created remote reference. It can be invoked by an Action, but must be treated as an endpoint with its own trust boundary.
- A function passed to `<form action>` or a submitter's `formAction` receives `FormData` and runs as an Action. A successful function Action resets uncontrolled fields; controlled values and `useActionState` state need an explicit reset contract.
- `useActionState` is appropriate when each Action result becomes the next state. Calls are queued sequentially, receive the previous result, and are not a latest-request-wins abstraction.
- `useOptimistic` overlays temporary state while an Action is pending. Its reducer or updater must be pure, and the authoritative value determines the result after success or failure.
- `useFormStatus` observes the nearest parent form, not a form rendered by the same component or one of its descendants. Place the status consumer inside the form and scope feedback to the submission it actually observes.
- Use `useTransition` directly when a custom Action boundary is required. Controlled input updates remain urgent, and state updates after an `await` need the version-documented Transition handling.

Prefer native form semantics when the operation is a submission. Retain `onSubmit` when direct submit-event control or an older React contract requires it. Do not wrap every asynchronous click in an Action merely to obtain a spinner.

## Define the transaction lifecycle

Write the observable contract before implementation:

1. identify the operation, authoritative input and submitter, including distinct `formAction` branches;
2. define idle, pending, validation, conflict, success, unexpected failure, retry and cancellation states that actually apply;
3. decide whether repeated intent queues, coalesces, replaces, cancels or runs independently;
4. define the authoritative result and how optimistic state reconciles or rolls back;
5. define focus, announcement, field preservation and reset behavior for every terminal result;
6. state what still works before hydration or without JavaScript when progressive enhancement is promised.

Treat `FormData`, captured identifiers and client validation as untrusted input at a remote boundary. Client validation improves feedback but never replaces server validation and authorization. Disabled controls may be absent from submitted data; read the actual successful controls and submitter contract rather than assuming every visible value is present.

Use stable operation identities where concurrent optimistic items must reconcile independently. Preserve user input on recoverable failures unless the product contract says otherwise. Avoid a global pending flag when separate forms or rows can submit independently, and do not disable unrelated navigation or reading while non-urgent work is pending.

## Handle order, cancellation and failure explicitly

Do not infer request ordering from completion timing. `useActionState` queues calls; a thrown Action cancels its remaining queue, while custom async Actions can resolve out of order unless the implementation guards them. Choose a policy from product semantics, then implement sequence identifiers, an abort signal, idempotency or serialization at the layer that can actually enforce it.

Cancellation must stop or disregard all affected work, clean optimistic state and leave a truthful final message. An `AbortController` only cancels operations that consume its signal; it is not proof that a remote side effect did not occur.

Return expected validation and domain conflicts as structured state. Throw unexpected faults to the applicable error boundary and observability path without exposing secrets. A visual rollback alone is insufficient if the remote result is unknown; reconcile from authoritative data or surface the uncertainty.

## Preserve accessible form behavior

Keep native labels, field names, descriptions, validation relationships and submit semantics. Associate field errors with their controls, focus the most useful recovery target without trapping the user, and announce asynchronous status changes without repeatedly reading the whole form. A pending appearance must remain perceivable without color; disabled controls must not be the only explanation of what is happening.

Progressive enhancement is an executed behavior, not a property inferred from using a Server Function. Verify the exact permalink or navigation target, the same form identity on the destination, pre-hydration submission replay, and useful server-rendered error or success output.

## Delegate bounded changes

When authorized component, Hook, form, fixture or test source must change, call `implementation` after defining the state, ordering, reset and evidence contract. Pass the affected paths, installed versions, transaction matrix and verification command. This skill retains Action and form semantics; `implementation` exclusively owns mutation safety and diff accounting. If it is unavailable, preserve the analysis and stop before mutation.

When an Action invokes or exposes a Server Function, call `react-server-components` before endorsing or changing that remote seam. Pass the resolved React/framework versions, function reference, arguments, serialized results, authentication context and expected mutation. This skill retains the client-visible lifecycle; `react-server-components` exclusively owns the server/client boundary, remote validation and authorization requirements. If it is unavailable, continue only the local lifecycle analysis, leave the remote portion unresolved and do not claim the workflow secure.

Calls install nothing and grant no dependency, network, credential, destructive, publication or deployment authority. Do not call a target already active in the invocation chain; preserve collected evidence and report the cycle.

## Verify and report

Exercise the real renderer and transport where claimed. Cover keyboard and pointer submission, each submitter, rapid repeats, queued or replaced intent, optimistic success and rollback, validation, unexpected failure, reset, cancellation, unmount or navigation, and pre-hydration or no-JavaScript behavior when promised. Assert visible state and authoritative results rather than scheduler internals or callback counts alone.

Report React, renderer and framework versions; the chosen Action and form seams; transaction and concurrency policy; client/server ownership; states exercised; commands and environments; progressive-enhancement evidence; and unresolved transport, security or accessibility risk.

## Composition boundaries

- `react` owns general component, Hook, rendering and concurrent UI semantics.
- `react-server-components` owns Server Function trust and serialization boundaries.
- Framework skills own routing, cache invalidation, request transport and post-mutation navigation.
- `testing`, `accessibility`, `performance` and `security` add their technology-neutral methods when independently selected.
