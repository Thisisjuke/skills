---
name: accessibility
description: Evaluate, specify, or improve accessibility across user-facing software and content using an explicit standard, representative interaction modes, layered evidence, and clearly bounded conformance claims.
license: MIT
---

# Accessibility

Make a user-facing experience perceivable, operable and understandable across
the interaction modes in scope. Produce evidence about barriers and supported
behavior without reducing accessibility to a visual checklist or an automated
score.

This skill owns technology-neutral accessibility intent, evaluation scope,
evidence strategy and claim boundaries. It does not own framework APIs,
platform semantics, tool commands or legal certification.

## Establish the accessibility contract

Before evaluating or changing the experience, identify:

- the surface, content and complete user tasks in scope;
- the people and access needs the work must support;
- relevant input, output and assistive interaction modes;
- target platforms, devices and supported configurations;
- the applicable standard, version, level and project policy, if any;
- the requested outcome: requirements, audit, implementation guidance,
  remediation or verification.

Do not silently turn a general accessibility request into a conformance claim.
If the governing standard or jurisdiction matters and is unspecified, report
that ambiguity. Use the version selected by the project; otherwise identify a
current suitable reference and state the choice rather than writing timeless
claims from memory.

Scope complete tasks, not only isolated screens. A step can appear accessible
while the overall flow remains blocked by authentication, timeout, validation,
state changes or an inaccessible adjacent step.

## Model barriers before techniques

Reason from what a person needs to perceive, understand and operate. Inspect
the applicable dimensions:

- **Perception:** text alternatives, captions or transcripts, adaptable
  structure, contrast, scaling, reflow, non-color cues and controllable media.
- **Operation:** keyboard or non-pointer access, focus order and visibility,
  target activation, alternatives to complex gestures, timing, motion and
  freedom from traps.
- **Understanding:** clear language, predictable behavior, descriptive labels,
  instructions, error identification, recovery and prevention for consequential
  actions.
- **Compatibility:** programmatically exposed name, role, value, state and
  relationships that remain meaningful through supported assistive technology.

Select checks from the actual task and standard. Do not universalize one HTML
technique, one device convention or one assistive technology. A technique is a
possible implementation of a requirement, not necessarily the requirement
itself.

Prefer native platform semantics and controls when they satisfy the behavior.
Added accessibility metadata does not create missing interaction behavior;
custom roles and widgets must fulfill the complete contract they advertise.

## Build layered evidence

Use the smallest combination of evidence that can answer the accessibility
contract:

1. Inspect source, structure and programmatic semantics for likely violations.
2. Use automated checks for rules they can deterministically evaluate.
3. Exercise representative tasks manually with relevant input modes, including
   focus movement and recovery from errors or interruptions.
4. Inspect output through applicable assistive technologies or accessibility
   APIs when their interpretation is part of the claim.
5. Check important variants: viewport or zoom, orientation, theme, locale,
   reduced motion, content length and loading, empty, error or disabled states.

Record the tested configuration: artifact or version, platform, viewport,
tools, assistive technology and relevant settings. Do not report a tool score
as proof of accessibility. Automated checks cover only a subset, manual
inspection uses judgment, and one assistive-technology combination does not
represent every user or platform.

When direct execution is unavailable, provide a static risk assessment and
name the runtime evidence still required. Never report an unperformed check as
passing.

## Handle findings and remediation

For each barrier, connect evidence to a user consequence:

```text
Requirement or user need
Affected task, state and interaction mode
Observed barrier and reproduction
User impact and reach
Applicable criterion or policy, when known
Confidence and evidence limits
Remediation direction
Verification needed after remediation
```

Prioritize loss of access to critical tasks, inability to perceive or operate
essential information, traps, destructive mistakes and broad reusable defects
over cosmetic or speculative concerns. A standards identifier supports the
finding but does not replace an explanation of impact.

Preserve behavior across modalities when remediating. Do not fix one mode by
breaking another, replace meaningful visible text with assistive-only text, or
add labels that contradict the visible control. Re-test the affected task and
neighboring states after a correction.

An audit is read-only unless the request authorizes changes. If implementation
is requested, follow repository instructions and selected implementation
guidance; this skill continues to own the accessibility outcome and evidence.

## Bound the conclusion

Report:

- scope and target requirements;
- configurations and interaction modes evaluated;
- findings ordered by user impact;
- checks performed and their results;
- untested modes, pages, states or assistive technologies;
- residual risks and the next evidence needed.

Use `no findings in tested scope`, `findings present`, or `inconclusive` unless
the evidence supports a more formal result. Do not claim full conformance,
certification or legal compliance from a sampled review, screenshots, automated
tools or a single assistive-technology pass.

## Composition boundaries

- `review` owns generic scope control, finding qualification, severity and
  review reporting; `accessibility` supplies the accessibility requirements,
  barriers and evidence.
- `testing` owns durable test strategy, observation seams, oracles and signal
  quality; `accessibility` defines the access behavior and representative modes
  those tests need to protect.
- `screenshot-critique` owns unprimed inspection of visible presentation;
  `accessibility` owns non-visual semantics, operation and assistive-technology
  evidence as well as applicable visual accessibility requirements.
- A technology accessibility skill owns platform APIs, native semantics,
  supported tools, assistive-technology combinations and implementation
  techniques. Its narrower platform constraints refine this skill without
  weakening the selected accessibility outcome.

These skills remain independently selectable. `accessibility` does not
automatically invoke them, and no future technology specialization should copy
this method wholesale.
