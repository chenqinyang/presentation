# React Journey Federation — Visual System Design Templates

Four English HTML explainers describe a **React Host UI composing multiple Journey UIs with Rsbuild Module Federation**. Onboarding, Checkout and Account are example Journey names.

## Open the collection

Open `index.html` in your browser, then choose a template. Every HTML contains its own styles, diagrams and teaching interactions. No installation, build step, server or internet connection is required to use the explainers. Official documentation links open external websites only when selected.

The pages explain an architecture; they are **not a runnable React microfrontend project**. Their scenario controls simulate behavior locally and do not fetch or execute remote modules. Integration snippets illustrate the configuration and application code you would add to actual projects.

## Choose a visual approach

| Template | Main question | Diagram and interaction |
| --- | --- | --- |
| [01 Architecture](01-architecture.html) | What lives where? | Browser boundary, Host shell, Journey slot, remote assets and shared dependencies. Select a Journey or inspect a component. |
| [02 Runtime loading](02-loading-sequence.html) | How does a route become a running Journey? | Seven sequence lanes, step-through controls, first visit, remote failure and cached revisit scenarios. |
| [03 Composition contracts](03-composition-contracts.html) | How do the UIs cooperate? | Host-to-Journey props, callbacks, routing and state ownership, with an interactive composition model. |
| [04 Release and resilience](04-release-resilience.html) | How are changes and failures contained? | Independent release paths, browser composition, failure scenarios and fresh-session versus existing-session behavior. |

Use 01 for orientation, 02 for runtime behavior, 03 for team integration contracts, and 04 for rollout or failure reviews. The visual language of each page can be reused independently.

## Architecture assumptions

- **Client-side rendering:** one React root, a persistent local Host shell and a Host-owned top-level router. Embedded Journeys export components instead of calling `createRoot` again. A separate standalone development entry may mount its own root.
- **Example routes:** `/onboarding/*`, `/checkout/*`, `/account/*`. Each provider exposes `./Journey`, a default React component. The Host consumes aliases such as `checkout/Journey`.
- **Enhanced Rsbuild integration:** `pluginReact()` and `pluginModuleFederation()` from `@module-federation/rsbuild-plugin`. Do not also register the native federation configuration for the same container.
- **Manifest-based remotes:** a Host entry points to `name@https://cdn.example.com/name/release/mf-manifest.json`. These are illustrative URLs. Metadata resolves to executable remote entry and chunk assets; JSON itself is not a UI component.
- **Explicit demand loading:** the example chooses `shareStrategy: 'loaded-first'` and lazy route imports. The default `version-first` policy has different initialization behavior, so do not reuse the loading or availability claims without checking the chosen strategy.
- **Shared runtime policy:** Host and remotes use aligned, tested React and ReactDOM versions and singleton configuration. Audit subpath imports such as React JSX runtimes and `react-dom/client`. The architecture page includes a `sharedRuntime` helper example. Package sharing does not establish a shared application store or guarantee compatibility.
- **Application contract:** the Host supplies route context, locale, theme, a session summary and explicit callbacks/capabilities. Journey-local form state is separate from Host state; durable business data belongs to backend services. Backend authorization remains necessary.
- **Failure scope:** Suspense covers pending lazy code. A route Error Boundary handles rejected lazy imports and descendant render failures. API failures, event handlers and other asynchronous errors require their own handling. Remotes share the page; federation is not a CSS, CPU or security sandbox.
- **Retry scope:** React caches a lazy loader's Promise. Resetting an Error Boundary alone does not retry a rejected import. A fresh page load or a deliberate loader/runtime recovery design is required.
- **Release selection:** the shown Host configuration pins immutable release paths. Changing a compiled remote URL requires rebuilding and deploying the Host. A dynamic registry is optional application infrastructure. Already-open tabs may retain modules; rollback does not automatically replace in-memory code.

Release identifiers in diagrams are illustrative; the releases page uses several versions to explain promotion and rollback. Component boundaries, Journey names, contracts and operating policies are example design choices, not a claim about your existing system.

## Where to edit

| File | Search in source | Replace together |
| --- | --- | --- |
| `01-architecture.html` | `EDITABLE CONFIG`, `journeys`, `components` | Names, paths, module aliases, diagram labels and component explanations. Also update the configuration snippets. |
| `02-loading-sequence.html` | `CONFIG`, `SCENARIOS` | Participants, step messages, branch behavior and related annotations. |
| `03-composition-contracts.html` | `EDITABLE JOURNEYS`, `EDITABLE DEMO CONFIG`, `JOURNEYS`, `CONFIG` | Journey definitions, Host inputs, callbacks and ownership explanations; update the displayed contract too. |
| `04-release-resilience.html` | `EDITABLE SCENARIOS`, `EDITABLE DIAGRAM` | Release nodes, selected paths, scenario copy, capability rows and retained-session behavior. |
| `index.html` | `gallery`, `chooser` | Template summaries, preview diagrams and navigation. |

Copy a template before editing. Keep the arrows, labels, application assumptions and interaction feedback consistent. Long text should wrap outside the main diagram; complex diagrams use a labeled internal scroll region on narrow screens.

## Prompt for future reuse

Copy the following and attach the HTML template you want to preserve:

```text
Create an English standalone HTML system-design explainer using the attached
file as the visual and interaction reference.

System: [system name]
Audience: [engineers / architects / product / onboarding]
Main question: [one question the diagram should answer]
Known components: [Host, Journeys, platform capabilities, APIs, delivery]
Ownership boundaries: [teams, state, routes, contracts]
Runtime behavior: [loading strategy, composition, important lifecycle steps]
Failure or release scenario: [one concrete case]
Unknowns: [label these as assumptions rather than inventing facts]

Preserve the reference's visual grammar, reading order and useful interaction
style. Rebuild the explanation for the new system instead of replacing nouns.
Use a dominant, labeled diagram with real arrows and clear boundaries.

Distinguish build time, code delivery, runtime composition, application state
and business API requests. Explain which behavior is provided by the framework
and which is an application convention. Include failure and recovery limits.
Do not invent performance results, automatic guarantees or production details.

Keep all labels, ARIA text, comments and instructions in English. Use one
self-contained HTML file with embedded CSS, SVG and JavaScript. No external
runtime dependencies or network requests. Controls must visibly update the
model and explanation. Support keyboards, reduced motion and narrow screens.

Include editable configuration markers, concise reuse guidance and links to
verified official documentation. State what was actually validated.
```

## Validation performed

JavaScript syntax, local navigation targets, unique IDs, English copy and absence of external runtime dependencies are checked. Main interactive states are exercised through a local DOM simulation. This validates program behavior, not browser rendering. Browser screenshot review was unavailable because the current browser policy blocks local-file previews; no screenshot or responsive-rendering pass is claimed.

## Official references

- [Rsbuild integration](https://module-federation.io/integrations/build-tool/rsbuild)
- [Remote aliases and entry locations](https://module-federation.io/configure/remotes.html)
- [Manifest configuration](https://module-federation.io/configure/manifest.html)
- [Shared dependencies and subpaths](https://module-federation.io/configure/shared)
- [Sharing strategy](https://module-federation.io/configure/shareStrategy.html)
- [React lazy](https://react.dev/reference/react/lazy)
- [React Error Boundaries](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary)
