# Engine-Specific Runtime Patterns

Use this file when advice needs to differ by Unity, Unity Puerts, Cocos Creator, LayaAir, or ThreeJS.

Derived from:
- FairyGUI runtime documentation for Unity, Cocos Creator, LayaAir, and ThreeJS-style integrations.
- Official runtime demos for Unity C#, Cocos Creator TypeScript, and LayaAir TypeScript.
- Condensed examples in `unity-patterns.md`, `cocoscreator-patterns.md`, and `layaair-patterns.md`.

## Unity C#

- For concrete examples, read `references/unity-patterns.md`.
- Use `Awake` for package loading, config, generated binders, and extension registration.
- Use `Start` after `UIPanel.ui` is available to bind view events and populate lists.
- Use `UIPanel` for scene/GameObject-attached UI and dynamic `GRoot` components for code-owned screens.
- For AssetBundle packages, load bundles through Unity first, then call the correct `UIPackage.AddPackage` overload.
- Use `Application.targetFrameRate = 60` in demos only; production frame rate should follow project policy.

## Unity Puerts

- Add `FAIRYGUI_PUERTS` to Unity scripting define symbols.
- Generate FairyGUI binding/glue code as part of the Puerts setup.
- Bridge virtual callbacks with `__onConstruct`, `__onInit`, `__onShown`, and `__onHide`.
- Register extensions with factory functions when TypeScript classes are created by the runtime.

## Cocos Creator TypeScript

- For concrete examples, read `references/cocoscreator-patterns.md`.
- Create `GRoot` in scene startup.
- Load packages asynchronously through `fgui.UIPackage.loadPackage`.
- Use Cocos component lifecycle for creation and disposal.
- Use FairyGUI event constants for UI input because FairyGUI handles click-through and custom hit areas.
- Keep UIProject outside `assets`; publish output goes under `assets/resources`.

## LayaAir TypeScript/JavaScript

- For concrete examples, read `references/layaair-patterns.md`.
- Add `fgui.GRoot.inst.displayObject` to `Laya.stage`.
- Use `Laya.Handler.create(this, callback, null, false)` for list renderers to avoid one-shot handlers.
- Let Laya load resources first when using manual `addPackage`.
- Disable compressed descriptions when mini-game rawinflate compatibility is a problem.

## ThreeJS

- Install and use `fairygui-three`.
- Build and test from the official/example project shape when starting from scratch.
- Keep guidance high-level unless the target repository contains a concrete ThreeJS FairyGUI integration.
