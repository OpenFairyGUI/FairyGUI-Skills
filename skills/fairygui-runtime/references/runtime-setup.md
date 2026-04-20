# Runtime Setup

Use this file for runtime bootstrapping, GRoot/UIPanel setup, screen fitting, and high-level UI architecture.

Derived from:
- FairyGUI runtime setup documentation for Unity, Cocos Creator, and LayaAir.
- Official demo entry points that show GRoot/UIPanel setup, package loading, global close buttons, and screen switching.

## Runtime architecture

- Treat FairyGUI as a package-driven UI system. Load packages, create components, attach to `GRoot`, a `Window`, a `UIPanel`, or another component.
- Keep the editor project outside the runtime asset folder unless the target engine requires published output under a resource directory.
- Build a small UI entry point that initializes GRoot/config, loads common packages, registers extensions/binders, and owns top-level screen/window lifecycle.
- Keep feature screens self-contained: package load, view creation, event binding, list renderer setup, cleanup.

## Unity setup

- Use `UIPanel` when a FairyGUI component should be placed in a Unity scene or attached to a GameObject lifecycle.
- Use dynamic creation when UI is driven entirely by code: `UIPackage.AddPackage`, `UIPackage.CreateObject`, `GRoot.inst.AddChild`.
- If a `UIPanel` references packages outside `Resources`, call `UIPackage.AddPackage` in `Awake` before `UIPanel` creates `ui`.
- Read `UIPanel.ui` after creation and keep it as a `GComponent`.
- For full-screen panels, call `MakeFullScreen` and add a size relation to `GRoot.inst` when dynamically created.
- Set `UIConfig` once during boot or before package UI uses shared controls: default font, scroll bars, popup menu, button sound.

## Cocos Creator setup

- Call `fgui.GRoot.create()` once per scene before creating FairyGUI content.
- Put published UI packages under `assets/resources` when using `fgui.UIPackage.loadPackage`.
- Use `fgui.UIPackage.createObject("Package", "Component").asCom`, then `makeFullScreen` and `fgui.GRoot.inst.addChild`.
- Use Cocos component lifecycle: create UI in `onLoad`/load callback and dispose/remove in `onDestroy`.
- Use persistent nodes only for UI that must survive scene changes, and close/remove it before switching scenes when needed.

## LayaAir setup

- Add `fgui.GRoot.inst.displayObject` to `Laya.stage` during boot.
- Configure `fairygui.js` as a plugin and include `rawinflate.min.js` only when compressed descriptions are used.
- Ensure Laya resources are loaded before `fgui.UIPackage.addPackage` when using manual loading.
- Use `fgui.UIPackage.loadPackage("resources/ui/Package", Handler...)` for package-owned loading.

## Screen and layer rules

- Prefer editor relations and full-screen root components over host-engine layout code.
- Use `sortingOrder` for top-level overlay order. In Cocos/Laya examples, close buttons use very high sorting order for global overlays.
- In Unity screen-space `UIPanel`, sorting order greater than the 2D `GRoot` layer appears above normal root UI.
- Avoid enabling input on non-interactive panels. Unity `UIPanel.Touch Disabled` can improve hit-test cost for HUD-like UI.
