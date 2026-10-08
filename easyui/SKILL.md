---
name: easyui
description: Discover, integrate, and adapt EasyUI animated React components using the official catalog, component APIs, and registry. Use when the user requests EasyUI, an EasyUI component, or integration of EasyUI into a React interface. Do not apply to unrelated UI libraries or non-React projects unless the user explicitly requests a port.
license: MIT
---

# EasyUI

Implement the requested interface using relevant EasyUI components, while preserving the project's architecture and design. This skill is instruction-only: use the current harness's available browsing, file editing, terminal, and preview tools. It requires no named connector, proprietary tool, or harness-specific syntax.

## Official sources

Use these entry points to retrieve only the information needed for the task:

- [LLM index](https://www.easyui.site/llms.txt): discover current documentation and component URLs.
- [Component catalog](https://www.easyui.site/components): compare components and inspect demos.
- [Quick start](https://www.easyui.site/docs/quick-start): installation and project setup.
- [Architecture](https://www.easyui.site/docs/architecture): styling and registry conventions.
- [Motion system](https://www.easyui.site/docs/motion-system): animation conventions and reduced motion.
- [Registry](https://www.easyui.site/registry.json): resolve component items and dependencies.
- [Source repository](https://github.com/Surajmaurya1/easyui): inspect implementations, repository documentation, and license notices when the site is incomplete.

The index is a discovery aid, not implementation code. Retrieve the chosen component's documentation or source before relying on its props, exports, dependency versions, installation command, or behavior. Do not embed a fixed catalog or assume a component count.

## Inspect the host project

Read the existing package manifest, lockfile, relevant UI files, and project instructions. Identify the React framework, rendering boundaries, Tailwind version and setup, animation library, path aliases, component locations, and existing design tokens.

Use the project's package manager and established conventions. EasyUI's upstream website dependencies are not a required dependency list for every consuming app. Add only what the selected implementation needs. Do not upgrade React or Tailwind, replace the design system, or introduce a second animation runtime merely to match the upstream site.

If the project is incompatible, explain the specific mismatch and propose the smallest viable adaptation. A non-React port must be described as an adaptation rather than a drop-in EasyUI installation.

## Choose and verify components

Map the requested user interactions to a small set of relevant catalog components. Prefer components that serve the task; add decorative motion only when it fits the requested design.

Read the selected component page and inspect its usage, source, API, installation instructions, and required styles. Check whether it depends on other registry items or local utilities. Use the live implementation to resolve export names, import paths, and animation APIs; do not invent them from the component title.

If a documentation URL fails, follow the current index or catalog links. If that still fails, use the official registry or source repository. Inspect actual directory listings before selecting source paths. If live sources are unavailable, use inspected local code or user-provided source and state that freshness could not be verified. Do not claim that an unverified implementation was installed from EasyUI.

## Integrate

Use the installation method verified for the selected component and compatible with the host project. Review existing target files before allowing a generator or CLI to overwrite them. Inspect the resulting changes and dependencies after installation.

For manual integration, copy the required implementation and its supporting utilities, styles, and dependencies. Preserve applicable upstream copyright and license notices. Avoid copying the entire demo application or unrelated components.

Adapt colors, typography, spacing, radii, theme behavior, and layout to the host project. Keep animation parameters coherent with its existing motion conventions. Check the component's actual imports before choosing between Framer Motion and other Motion packages.

In frameworks with server and client components, put hooks, animation, and browser APIs behind the appropriate client boundary. Avoid browser-only work during server rendering and unstable initial values that cause hydration mismatches.

Connect callbacks and state to the application's real behavior. Demo authentication, upload, payment, telemetry, and notification simulations are examples, not working service integrations. When service integration is outside scope, make the demonstration state explicit.

## Validate the result

Run the host project's relevant type, lint, build, and behavior checks when available. Inspect the rendered interface at representative narrow and wide viewports if preview tools are available.

Check the behaviors relevant to the chosen components:

- Keyboard operation, visible focus, accessible names, and focus restoration for overlays.
- Touch interaction for controls that otherwise depend on hover or dragging.
- Reduced-motion behavior that preserves functionality and suppresses unnecessary movement.
- Loading, disabled, empty, error, and success states where the interaction needs them.
- Layout stability, overflow, and long content in the project's supported themes.
- Cleanup and performance for animation loops, listeners, timers, canvas, and WebGL when present.

Do not infer accessibility or production readiness from catalog descriptions. Verify the integrated behavior. If visual or runtime checks cannot run, report that limitation without claiming they passed.

## Report completion

Briefly identify the components used, where they were integrated, any added dependencies or material adaptations, and the validation performed. Include the official source links used for component-specific decisions and any unresolved integration limitation.

Skill invocation authorizes work within the user's requested scope. Publishing, deployment, and unrelated account or configuration changes still depend on that scope and the harness's permission rules.
