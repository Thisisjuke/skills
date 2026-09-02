---
name: api-design
description: Design or evolve a consumer-facing contract by defining its boundary, semantics, failures, compatibility promise, lifecycle, and verification evidence without assuming a language or transport.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED domain-modeling"
  uses-glossary: "true"
---

# API design

Design contracts that let a provider and its consumers change independently
without guessing what the interface means. An API is any intentionally consumed
boundary: a service protocol, library surface, module interface, event or file
schema, component contract, command-line interface or configuration format.

This skill owns the shape and semantics of that boundary, its failure model,
compatibility promise, evolution path and consumer impact. It does not own
product requirements, language or transport conventions, implementation,
security analysis or generic review and testing methods.

## Establish the design context

Before choosing names, types or operations, identify:

- the provider, known consumers and the tasks they need to accomplish;
- whether the boundary is private, internal, partner-facing or public;
- its expected lifetime, release independence and compatibility obligations;
- the authoritative behavior or specification and any unresolved product
  decisions;
- trust, authorization, privacy, performance and resource constraints that
  affect the contract;
- whether this is a new interface, an intentional evolution or an assessment of
  an existing one;
- the requested artifact and whether repository mutation is authorized.

Inspect existing consumers and observable behavior before changing an interface.
Undocumented behavior is not automatically promised forever, but evidence of
consumer reliance is a migration risk that must be investigated rather than
dismissed.

If the required behavior is unclear, surface the missing product decision. Do
not invent requirements merely to complete an attractive contract.

## Start from consumer scenarios

Describe the smallest set of scenarios the boundary must support before fixing
its representation. Include the normal path and the decision-changing boundary
cases:

- absent, malformed, stale or conflicting input;
- partial success, empty results and unavailable dependencies;
- repeated, concurrent, cancelled or timed-out requests when applicable;
- authorization and data-exposure differences;
- ordering, consistency, latency or resource constraints;
- creation, update, deprecation and removal over the interface lifecycle.

Separate behavioral semantics from protocol and language mechanics. “A repeated
request must not duplicate the effect” is a contract requirement; a particular
header, database constraint or annotation is one possible implementation.

## Minimize and clarify the exposed contract

Expose only concepts consumers need. Prefer cohesive operations and explicit
domain terms over leaking storage layouts, framework objects or internal
control flow. Give each invariant and representation one clear owner.

For each applicable operation, message, type or field, define:

- purpose, preconditions and permissions;
- inputs, defaults, validation and omitted-value semantics;
- outputs, guarantees and absence or partial-result semantics;
- failure categories and what consumers may safely infer from each;
- side effects, ordering and consistency expectations;
- retry, idempotency, cancellation and timeout behavior;
- concurrency or conflict behavior;
- data ownership, sensitivity and retention expectations;
- stability level, compatibility promise and lifecycle state.

Record only dimensions that matter to the boundary. A local pure function does
not need a distributed retry protocol; a payment command cannot leave retry
semantics implicit.

Use one predictable error model within a coherent boundary. Keep stable
machine-actionable classification separate from human explanation when both are
needed. Distinguish invalid input, missing state, conflict, denied access and
temporary inability only when consumers can or must react differently. Do not
expose sensitive internals or make consumers parse incidental message text.

## Design evolution explicitly

Classify compatibility rather than calling a change simply “breaking” or
“additive.” Depending on the interface, relevant dimensions may include source,
binary, wire, data, behavior and operational compatibility.

For an existing contract:

1. Inventory known consumers, versions and observed reliance.
2. State the compatibility promise that actually applies.
3. Compare old and proposed behavior in each relevant dimension.
4. Identify consumer changes, rollout order and mixed-version states.
5. Define deprecation evidence, migration support and a removal condition.
6. Version or introduce a compatibility seam only when coordinated evolution is
   not sufficient or the promised contract requires it.

Addition is often safer than replacement, but it is not automatically
compatible. A new optional field, enum value, event variant or side effect can
break exhaustive consumers or alter meaning. Verify consumer behavior instead
of relying on the shape of the diff.

Temporary aliases, adapters and parallel versions need an owner and retirement
condition. Do not turn migration machinery into a second permanent contract by
default.

## Validate the design

Walk representative consumers through normal, boundary and failure scenarios.
Check that they can determine:

- what to send or call and which assumptions are safe;
- how to interpret every outcome that requires a different response;
- whether retrying, caching, reordering or concurrent use is safe;
- how old and new participants coexist during migration;
- which guarantees are normative and which details may change.

Use examples, schemas, type checks, contract tests, compatibility tools or
consumer fixtures where they provide credible evidence for the selected
technology. Types and generated schemas are useful evidence, but they do not
replace semantics, lifecycle policy or rationale.

Verify that names are consistent within the existing ecosystem. Do not impose
REST resource naming, casing, partial-update verbs, pagination style or a
language-specific type pattern from this generic skill.

## Keep domain language aligned

Consult applicable `GLOSSARY.md` files from the repository root toward the
target when the interface uses project-specific terms. An explicit nearer
definition takes precedence. If design work reveals an ambiguous term or
establishes canonical public vocabulary, call `domain-modeling` when it is
available and the invocation is acyclic. Obtain human agreement before
glossary mutation.

If `domain-modeling` is unavailable, report the terminology decision or
ambiguity without silently creating a glossary. Reading an existing glossary is
consultation, not a skill invocation.

## Produce the contract

Return or, when authorized, write a proportionate design containing:

```text
Boundary, provider and consumers
Consumer scenarios
Operations, messages or types and their semantics
Failure and side-effect model
Compatibility promise and consumer impact
Evolution, rollout and retirement conditions
Verification evidence
Open product or technology decisions
```

Keep the artifact concise enough for consumers to use. Separate normative
guarantees from illustrative examples. Do not edit implementation merely
because the contract appears straightforward; mutation belongs to a separately
selected implementation capability.

## Composition boundaries

- `specification` owns required product behavior and acceptance outcomes;
  `api-design` translates accepted behavior into a consumer/provider contract.
- `review` owns finding qualification and reporting; `api-design` supplies the
  boundary, compatibility and evolution lens.
- `testing` owns test strategy and oracle quality; `api-design` supplies
  contract scenarios and guarantees to verify.
- `security` owns threat analysis and security controls; `api-design` exposes
  trust, authorization, data and failure boundaries that analysis must cover.
- `refactoring` preserves an existing public contract; `api-design` owns an
  intentional change to that contract.
- Technology skills own language, transport, serialization, framework, tooling
  and platform compatibility mechanics.
- `implementation` owns authorized repository mutation and final diff
  accountability.

Except for the conditional terminology call to `domain-modeling`, this skill
does not automatically invoke another skill.
