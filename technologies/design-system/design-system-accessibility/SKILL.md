---
name: design-system-accessibility
description: Evaluate or improve design-system tokens and components for reusable accessibility semantics, keyboard and focus behavior, states, themes, consumer guidance, and regression resistance across supported platforms.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation"
---

# Design system accessibility

Evaluate accessibility responsibilities that a design system centralizes for all consumers. This skill owns reusable component semantics, token constraints, state behavior and consumer guidance. The generic `accessibility` skill owns evaluation scope and conformance methodology; platform skills own their APIs.

## Establish claims and coverage

Identify supported standards and level, browsers, platforms, input modes, assistive technologies, themes and component states. Separate behavior guaranteed by the component from behavior the consumer must supply, such as a label, heading context or error message.

For web systems, use the repository's declared target and [WCAG 2.2](https://www.w3.org/TR/WCAG22/) when that is the active baseline. Use the [ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/) to understand composite-widget patterns and keyboard models, not as proof that copied example code is conformant in the product. Prefer native HTML semantics and behavior before adding ARIA. Platform-native systems require their corresponding accessibility APIs and guidance.

## Inspect system-wide leverage

- semantic role, name, value, state and relationship exposure;
- keyboard operation, focus order, focus visibility, focus not obscured, restoration and escape behavior;
- disabled, readonly, required, invalid, loading and error semantics;
- contrast and non-color cues across themes, modes and interaction states;
- zoom, text spacing, reflow, reduced motion, forced colors and user preferences;
- target size, dragging and pointer alternatives where applicable;
- announcements for dynamic content and async state;
- guidance and examples that prevent consumers from producing inaccessible combinations.

For composite widgets, test the documented focus model, arrow-key behavior, selection versus focus, typeahead and exit behavior where applicable. For overlays, test initial focus, containment only when required, background inertness, accessible naming, Escape behavior and restoration to a valid target. Do not apply an ARIA role without implementing its required states and keyboard contract.

Token checks must cover every supported theme and state, including focus indicators, disabled content, non-text graphics and forced-colors behavior. Avoid hard-coded colors, removed outlines and motion that bypass system tokens. Test zoom and reflow with realistic content rather than assuming scalable units are sufficient.

Prefer fixing a faulty primitive or token once over documenting repeated consumer workarounds. Preserve explicit consumer duties for labels, content hierarchy, live-message wording and contextual focus targets. Do not claim conformance from automated rules, an APG-shaped implementation or one component state.

## Remediate safely

When authorized remediation requires repository changes, call `implementation` with the affected component/token, standard reference, representative states, consumer compatibility constraints and validation plan. This skill retains accessibility requirements; `implementation` exclusively owns mutation safety. If unavailable, return findings and stop before edits. The call does not authorize dependency installation, external services or broad API breaks. Do not call an active target or ancestor; report the cycle.

## Report

State system/version, standards and modes evaluated, component or token, guaranteed versus consumer-owned behavior, automated and manual evidence, assistive technologies actually exercised, affected consumers, severity or reach, remediation direction and untested combinations. When composed with `accessibility`, let it own bounded conformance wording.
