---
name: fairygui-runtime
description: FairyGUI runtime and framework coding standards across Unity C#, Unity Puerts/TS, Cocos Creator TypeScript, LayaAir TypeScript/JavaScript, ThreeJS, and custom runtimes. Use when writing, reviewing, or troubleshooting package loading, GRoot/UIPanel setup, generated bindings, UIObjectFactory extensions, lists and virtual lists, events, transitions, windows, lifecycle, memory, batching, or runtime performance.
---

# FairyGUI Runtime Skill

## Workflow

1. Confirm target engine, language, and load model: Unity C#, Unity Puerts, Cocos Creator, LayaAir, ThreeJS, or custom.
2. Identify the runtime task: root setup, package loading, generated binding, extension class, list rendering, events, transition control, window lifecycle, memory, or performance.
3. If the task needs deeper details, read the relevant reference file:
   - Runtime setup: references/runtime-setup.md
   - Package loading: references/loading-packages.md
   - Component binding and extensions: references/component-binding.md
   - Lists and scrolling: references/lists-scrolling.md
   - Events and animation: references/events-animation.md
   - Engine-specific runtime patterns: references/engine-patterns.md
   - Unity condensed examples: references/unity-patterns.md
   - Cocos Creator condensed examples: references/cocoscreator-patterns.md
   - LayaAir condensed examples: references/layaair-patterns.md
   - Performance and lifecycle: references/performance-lifecycle.md
4. Ground advice in the editor contract: package names, published file names, item URLs, child names, controllers, transitions, and generated binder order.
5. Provide code-level steps with lifecycle placement and cleanup.

## Output expectations

- Provide minimal, working code snippets.
- Note init order, async load, package lifetime, object disposal, and event unsubscription when relevant.
- Register generated binders or `UIObjectFactory` extensions before creating UI.
- Prefer the condensed engine pattern reference files when they match the target engine.
