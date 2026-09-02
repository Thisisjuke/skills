---
name: security
description: Assess or harden a system, design, or change by modeling threats, tracing trust boundaries, qualifying exploitable risk, selecting proportionate controls, and verifying residual exposure without assuming a technology.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation"
---

# Security

Reduce credible harm by understanding what must be protected, how an adversary
or failure can cross a boundary, which controls interrupt that path, and what
risk remains. Focus on concrete assets, capabilities and data flows rather than
applying a generic checklist to every system.

This skill owns technology-neutral threat modeling, security risk analysis,
control objectives and residual-risk reporting. It does not own generic review
format, implementation safety, test design, operational instrumentation,
privacy or legal certification, or language- and framework-specific controls.

Security analysis is read-only by default. Do not probe live systems, access
real sensitive data, rotate credentials, change permissions or mutate a
repository unless those actions are explicitly authorized.

## Establish the security contract

Determine:

- the system, change, interface or incident in scope;
- the assets and security properties at stake, such as confidentiality,
  integrity, availability, authenticity, accountability or user control;
- expected users, administrators, services and plausible adversaries;
- deployment, tenancy, privilege and data-sensitivity context;
- applicable project policy, standard, regulation or risk model;
- whether the request is threat analysis, security review, control design,
  verification or authorized hardening;
- evidence and environments available, including explicit limits on testing.

Read applicable instructions, specifications, architecture decisions,
glossaries and existing security controls. Do not claim compliance with a law
or standard unless its version, scope and required evidence are known.

## Model the attack surface

Draw or describe the minimum useful model of components, actors, data stores,
privileged operations and data flows. Mark every place where data, identity,
authority, code or control crosses between different trust assumptions.

For each relevant boundary, ask:

- Can an actor be mistaken for another actor?
- Can data, code, state or configuration be modified without authorization?
- Can a consequential action occur without attributable evidence?
- Can sensitive information cross to an unintended consumer?
- Can bounded resources be exhausted or critical service denied?
- Can a less privileged actor gain a capability it should not possess?

These questions are a lens, not proof of completeness. Adapt the threat model
to the system: a local library, mobile application, build pipeline, public
service and AI-enabled workflow expose different boundaries.

Write concrete abuse cases that connect:

```text
actor and capability
→ entry point or trust boundary
→ required preconditions
→ vulnerable behavior or missing control
→ affected asset and consequence
```

Treat user, file, network, dependency, configuration, persisted, generated and
model-produced data according to its actual provenance. “Internal” is not a
trust guarantee: compromised dependencies, stale data, confused callers and
cross-tenant paths can make an internal boundary security-relevant.

## Assess existing controls and exposure

Trace each credible abuse path through the real design or code. Identify the
controls already present and verify where they are enforced. Distinguish:

- prevention, such as authorization or safe interpretation;
- containment, such as least privilege and isolation;
- detection, such as security-relevant audit signals;
- recovery, such as revocation, restoration or safe rollback.

Do not infer a control from a function name, framework default, type annotation
or UI restriction. Confirm the authoritative enforcement point and whether an
alternate path bypasses it.

Examine the risk areas that actually apply:

- identity proof, credentials, sessions and delegated authority;
- authorization at object, operation and tenant boundaries;
- validation, interpretation, injection and unsafe output use;
- secrets, sensitive data, minimization, exposure and retention;
- cryptographic purpose, key lifecycle and integrity guarantees;
- concurrency, replay, ordering and state-transition abuse;
- resource exhaustion, amplification and degradation behavior;
- external services, callbacks, redirects and outbound requests;
- dependencies, build inputs, artifacts and update provenance;
- configuration, deployment privilege and insecure fallback behavior;
- security event evidence without leaking sensitive content.

When applicable, include uploaded files and model-generated content as active
input surfaces: validate type and size independently of names, isolate storage
and interpretation, constrain downstream tools, and prevent retrieved content
from gaining instruction authority. Treat privacy obligations as scoped policy
requirements requiring their own legal or organizational evidence, not as a
claim inferred from technical controls.

Technology-specific guidance supplies concrete APIs, algorithms, protocol
settings and scanner commands. Verify time-sensitive recommendations against
the selected platform's authoritative current documentation.

## Qualify security risk

Report a vulnerability only when evidence supports a reachable abuse path or a
violated security requirement. Otherwise present a hypothesis, missing control
or verification gap with the assumptions needed to resolve it.

For each risk, capture:

- affected asset and security property;
- attacker capability, exposure and preconditions;
- concrete path and enforcement gap;
- plausible impact, scope and reversibility;
- likelihood or feasibility under the stated environment;
- existing mitigating controls and possible bypasses;
- confidence and evidence still missing;
- proposed control objective and residual risk.

Use the project's risk model when one exists. Do not assign universal response
deadlines from a generic Critical/High/Medium/Low label. When composed with
`review`, let `review` own finding severity, blocking status and report format;
provide the security-specific exploitability and impact evidence it needs.

Avoid destructive proof of concept, real credential use, persistence,
denial-of-service load or access beyond the authorized scope. Prefer static
tracing, controlled fixtures, isolated environments and the smallest safe
demonstration that distinguishes vulnerable from protected behavior.

## Select proportionate controls

Choose controls that interrupt the demonstrated path at an authoritative
boundary. Prefer controls that are:

- enforced server-side or by the trusted component, not merely requested of an
  untrusted caller;
- deny-by-default and least-privileged where denial is safe;
- complete across equivalent entry points and object instances;
- explicit about identity, ownership, tenancy and delegated authority;
- safe under malformed, repeated, concurrent and partially failed operations;
- independent from secrets embedded in source, logs or client-visible state;
- observable enough to detect abuse without exposing sensitive content;
- recoverable when credentials, data or dependencies are compromised.

Use layered controls when one failure would expose a high-impact asset, but do
not add ceremonial controls that do not change an abuse path. Preserve product
behavior and accessibility unless the accepted security requirement requires a
trade-off; surface that trade-off instead of silently degrading either.

For dependencies and externally maintained components, separate known-advisory
status from actual reachability and from provenance risk. Do not automatically
execute an install, scanner, forced upgrade or audit remediation. Inspect the
repository's actual dependency boundary and pinned toolchain, then obtain any
needed authorization before commands that fetch or execute third-party code.

## Verify the control

Define evidence before declaring a risk resolved:

- the abuse case fails at the intended enforcement point;
- the legitimate path still succeeds with the least required authority;
- sibling objects, tenants, roles and alternate entry points cannot bypass it;
- malformed, oversized, repeated, concurrent and failure cases are handled as
  required by the threat model;
- sensitive values remain absent from outputs, logs and unintended storage;
- relevant security events are detectable and recovery remains possible;
- technology-specific checks run with the correct scope and version.

Tests and scanners support a conclusion but do not replace threat coverage or
manual reasoning. Record exactly what was inspected or executed, its result,
and what remains unverified.

## Apply authorized hardening

For an authorized repository change, call `implementation` if it is available
and the invocation is acyclic. Pass the accepted abuse case, control objective,
scope, invariants and verification evidence; do not duplicate its mutation,
dirty-tree or final-diff workflow.

If `implementation` is unavailable, report the configuration gap before
mutation. Security analysis, threat modeling and control recommendations remain
standalone and do not require that call.

Changes to identity, authorization, secrets, sensitive-data collection,
external integrations or production controls may require separate human or
organizational approval. This skill identifies that boundary; it does not infer
authority from the security task itself.

## Report

Return a proportionate security assessment:

```text
Scope, assets and security objectives
Actors, data flows and trust boundaries
Abuse cases and assumptions
Existing controls
Risks or verification gaps with evidence
Control objectives and authorized changes
Verification performed and outcomes
Residual risk and decisions requiring an owner
```

Do not claim the system is secure because no issue was found. State the
examined surface, evidence limits and residual exposure.

## Composition boundaries

- `review` owns generic findings, severity, blocking disposition and verdict;
  `security` supplies threat, exploitability, impact and control evidence.
- `api-design` owns interface semantics and evolution; `security` owns trust,
  authorization, data-exposure and abuse constraints on that interface.
- `testing` owns test strategy and signal quality; `security` supplies abuse
  cases and control objectives to verify.
- `observability` owns durable logs, metrics, traces and alerts; `security`
  identifies security events, sensitive fields and detection needs.
- `accessibility` owns access needs and barrier evidence; security controls must
  preserve them or make an explicit, accepted trade-off.
- Technology skills own platform threats, secure APIs, protocol settings,
  dependency tools and implementation mechanics.
- `implementation` owns authorized mutation and final diff accountability.

Except for the conditional call to `implementation` for authorized hardening,
this skill does not automatically invoke another skill.
