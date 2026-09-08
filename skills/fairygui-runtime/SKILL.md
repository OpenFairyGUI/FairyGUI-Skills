---
name: fairygui-runtime
description: Archived FairyGUI runtime reference guidance for Unity C#, Unity Puerts, Cocos Creator, LayaAir, ThreeJS and custom integrations. Use when writing or reviewing package loading, bindings, data refresh, lists, events, transitions, lifecycle or performance code. Verify examples against the installed SDK; this Skill bundles no tools.
---

# FairyGUI Runtime Skill

## Maintenance status

Final edition `v1.1.0` (2026-09-08). This repository is archived and no longer maintained. Runtime examples remain historical reference material; verify APIs and lifecycle behavior against the installed engine/SDK before applying them.

[OpenFairyGUI](https://github.com/OpenFairyGUI/OpenFairyGUI) maintains project SDK/CLI/MCP automation; [FairyGUI-Maker](https://github.com/OpenFairyGUI/FairyGUI-Maker) provides Agent authoring and preview workflows. Neither fully replaces this Skill's multi-engine runtime coverage. Use the capabilities already available for the requested task; following these links does not authorize installation or imply a working tool connection. Discussion and review remain read-only.

## Workflow

1. Identify the engine/SDK version, language and load model from the project and installed dependencies: Unity C#, Unity Puerts, Cocos Creator, LayaAir, ThreeJS or custom. Ask only when a material choice remains unknown.
2. Identify the runtime task: architecture, root setup, package loading, ownership, data refresh, generated binding, extension class, list rendering, events, transition control, window lifecycle, memory, or performance.
3. If the task needs deeper details, read the relevant reference file:
   - Runtime setup: references/runtime-setup.md
   - Runtime architecture and UI system organization: references/runtime-architecture.md
   - Package loading: references/loading-packages.md
   - UI lifecycle, ownership, disposal, and async race boundaries: references/ui-lifecycle-and-ownership.md
   - Component binding and extensions: references/component-binding.md
   - Data binding, refresh, model-to-view rules, and stale async guards: references/data-binding-and-refresh.md
   - Lists and scrolling: references/lists-scrolling.md
   - Events and animation: references/events-animation.md
   - Engine-specific runtime patterns: references/engine-patterns.md
   - Unity condensed examples: references/unity-patterns.md
   - Cocos Creator condensed examples: references/cocoscreator-patterns.md
   - LayaAir condensed examples: references/layaair-patterns.md
   - Performance and lifecycle: references/performance-lifecycle.md
4. Ground advice in the editor contract: package names, published file names, item URLs, child names, controllers, transitions, and generated binder order.
5. Prefer business-state-driven UI updates: runtime code should drive named controllers, button states, loaders, and list data instead of manually patching many children.
6. Provide code-level steps with lifecycle placement and cleanup.

## Output expectations

- Provide minimal, working code snippets.
- Note init order, async load, package lifetime, object disposal, and event unsubscription when relevant.
- Register generated binders or `UIObjectFactory` extensions before creating UI.
- Prefer the condensed engine pattern reference files when they match the target engine.
