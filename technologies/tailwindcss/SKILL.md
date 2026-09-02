---
name: tailwindcss
description: Design, implement, review, or diagnose Tailwind CSS utility styling, theme variables, variants, responsive behavior, class composition, custom CSS, and source-detection-safe class usage while preserving the installed major and project design contract.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED tailwindcss-build"
---

# Tailwind CSS

Work from the installed Tailwind major and the project's rendered CSS contract. This skill owns utility authoring, theme-variable use, variants, class composition and Tailwind-aware styling decisions. It does not own package installation, bundler integration, source scanning configuration, a product's token taxonomy, component behavior or generic mutation safety.

## Establish the actual Tailwind contract

Inspect the manifest and lockfile, CSS entrypoints, Tailwind imports or directives, framework, class composition helpers, browser support, theme source, component conventions and representative rendered output. Distinguish Tailwind v3 configuration from the CSS-first v4 line instead of translating syntax from memory.

As of the current documentation line, Tailwind CSS 4.3 uses CSS imports, theme variables and dedicated Vite, PostCSS and CLI packages. Tailwind v4 targets Safari 16.4+, Chrome 111+ and Firefox 128+; projects requiring older browsers should remain on the supported v3.4 line. Re-check the official [documentation](https://tailwindcss.com/docs/installation/framework-guides), [upgrade guide](https://tailwindcss.com/docs/upgrade-guide) and [releases](https://github.com/tailwindlabs/tailwindcss/releases) before encoding version-sensitive advice.

Preserve the installed major unless migration is explicitly in scope. Do not introduce a JavaScript config into a CSS-first project, delete a v3 config, add a compatibility dependency or change browser support merely to match the newest docs.

## Author statically detectable classes

Tailwind scans source as plain text. Every generated candidate must occur as a complete token in a detected source or an explicit source declaration. Map props and states to complete class strings:

```tsx
const tones = {
  danger: "bg-danger text-danger-foreground",
  neutral: "bg-surface text-foreground",
} as const
```

Do not build fragments such as ``bg-${tone}-600`` and assume Tailwind understands program logic. For truly data-driven runtime values, use a bounded CSS custom property or inline style and consume it from a statically present class. Use safelisting only for an external vocabulary that cannot be represented in source; keep it narrow and test the compiled artifact.

When a monorepo, shared package, generated template or ignored path supplies classes, verify that the CSS build scans the actual files. Tailwind v4 can set an import base with `source()`, add paths with `@source`, exclude paths with `@source not` and explicitly include candidates with `@source inline()`. These are build concerns: route changes to `tailwindcss-build`.

## Keep styling semantic and composable

- Prefer established semantic utilities and theme variables over raw palette values when the UI participates in a design system.
- Use responsive, container, state, group, peer, data and ARIA variants to express the actual condition; do not reproduce state in JavaScript only to toggle presentation.
- Keep complete state pairs visible, including default plus dark, hover, focus-visible, disabled, invalid and reduced-motion behavior when applicable.
- Use arbitrary values for genuinely isolated values. Promote repetition or product meaning into the adopted theme or component contract.
- Use logical properties and variants where directionality matters; verify both LTR and RTL rather than mirroring with ad hoc exceptions.
- Prefer a real component boundary for repeated structure. `@apply` and custom component classes do not replace a component's behavior or API.

In Tailwind v4, `@theme` variables create utilities and variants; ordinary CSS variables do not. Use `@theme inline` when a theme variable should inline another variable's value, and understand its scope before changing it. When the authoritative product token system lives elsewhere, map it deliberately instead of inventing a second semantic vocabulary.

Use `@utility` for a missing reusable utility with a stable contract. Use ordinary CSS for third-party integration, selectors or behavior that utilities do not express cleanly. Keep custom rules in an intentional cascade layer and verify how they interact with Preflight and existing CSS.

## Resolve variants and conflicts deliberately

Do not rely on the order of tokens inside a `class` attribute: when utilities conflict, generated stylesheet order decides the winner. Emit one utility per property and condition whenever possible. If consumers can extend classes, define which aspects are supported and use the project's version-compatible merge helper only at that boundary; a merge helper cannot prove visual or semantic correctness.

Use the `!` modifier or import-wide `important` only after identifying an unavoidable specificity boundary. Do not mask contradictory variants, a broken cascade layer or an overly broad third-party selector with escalating importance.

For dark mode, preserve the project's selector, data-attribute or media-query strategy. A v4 `@custom-variant dark` changes activation semantics for every `dark:*` utility, so treat it as a system decision and verify initial server render, hydration and persisted preference where a framework controls the theme.

## Preserve usable output

Utilities do not make markup accessible. Preserve semantic elements, accessible names, keyboard behavior, visible focus, target size, text zoom, contrast, forced-colors behavior and reduced motion. Responsive work must handle narrow viewports, long localized content, zoom and content growth without hiding required actions. Avoid `transition-all`, indiscriminate `will-change` and decorative motion that ignores the repository's motion policy.

Verify the result in the browser and in the compiled CSS. Exercise relevant state, breakpoint, container, color scheme and interaction combinations. Confirm expected candidates exist, obsolete classes are absent when size matters, and production output matches development.

## Delegate bounded changes

When authorized markup, component or stylesheet source must change, call `implementation` after establishing the Tailwind major, expected rendered behavior, supported states and affected files. Pass those facts and the repository verification commands. This skill retains Tailwind semantics; `implementation` exclusively owns mutation safety and diff accounting. If it is unavailable, return the styling decision and stop the mutation portion.

When installation, package versions, CSS entrypoints, plugins, PostCSS, Vite, CLI commands, source detection, migration or emitted CSS artifacts must change, call `tailwindcss-build` with the resolved major, framework, current pipeline, browser contract and expected artifact. It exclusively owns build integration while this skill retains intended styling. If unavailable, preserve the diagnosis and stop the build change.

Calls do not authorize dependency installation, network access, framework migration, deployment or broad restyling. Do not call a target already active in the invocation chain or an ancestor; report the cycle and keep collected evidence.

## Composition boundaries

- `design-system` and `design-system-tokens` own product-wide semantic decisions and token governance.
- Framework and component-library skills own component behavior, runtime boundaries and public APIs.
- `accessibility`, `performance`, `testing` and `review` provide their technology-neutral evidence when independently selected.
- `tailwindcss-build` owns toolchain and artifact mechanics; Tailwind utility authoring remains here.
