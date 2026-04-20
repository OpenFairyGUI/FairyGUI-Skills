# Runtime Architecture

Use this file when designing the runtime UI system around FairyGUI: layers, screens, windows, package ownership, managers, and boundaries between UI code and gameplay/application code.

Derived from:
- FairyGUI runtime documentation for GRoot, Window, UIPanel, package loading, and runtime component creation.
- Official Unity, Cocos Creator, and LayaAir examples that separate package loading, screen creation, extension registration, and root-level overlays.

## Architecture Goals

- Keep FairyGUI UI creation predictable: load package, register binders/extensions, create view, bind events, refresh data, dispose.
- Keep package lifetime separate from view lifetime.
- Keep visual state in FairyGUI components/controllers where possible; keep business state in application models/services.
- Keep engine-specific integration at the edges: Unity GameObject/UIPanel, Cocos Component, Laya stage/root setup.

## Host Pattern Selection

Use direct `GRoot` children when:
- The screen is fully code-owned.
- The UI is a normal 2D screen, overlay, menu, HUD, or temporary effect.
- The view does not need to be attached to a host-engine scene object.

Use `Window` when:
- The UI needs modal behavior, stacking, show/hide lifecycle, close handling, or show/hide animation.
- Runtime code benefits from `OnInit`, `OnShown`, `OnHide`, or equivalent hooks.
- The panel is logically a dialog rather than a permanent screen section.

Use Unity `UIPanel` when:
- The UI must attach to a GameObject lifecycle.
- The UI is scene-authored, world-space, HUD anchored to an object, or inspected in Unity.
- Render mode, sorting order, hit test mode, or native child ordering must be configured through Unity.

Use a plain `GComponent` when:
- The object is a reusable view, subview, item renderer, screen root, or container inside a larger FairyGUI UI.
- Lifecycle is owned by another view/window/manager.

## Manager Boundaries

Use a `UIManager` for:
- Bootstrapping FairyGUI runtime config.
- Owning top-level layers and screen/window open APIs.
- Routing view creation through package and lifecycle policies.

Use a `PackageManager` or asset service for:
- Loading/unloading packages.
- Resolving package dependencies.
- Integrating AssetBundle, Addressables, Cocos resources, Laya resources, or remote assets.

Use a `WindowManager` or layer stack for:
- Modal stacking.
- Overlay order.
- Back navigation.
- Blocking input while async UI opens.

Avoid:
- Letting feature views unload shared packages directly.
- Letting gameplay systems call `UIPackage.CreateObject` everywhere.
- Letting view classes own global package policy.

## Layer Model

Typical layers:
- `Background`: persistent scenic UI or non-interactive backdrops.
- `Main`: normal screens.
- `HUD`: gameplay HUD and floating UI.
- `Window`: modal and non-modal windows.
- `Popup`: context menus, dropdowns, tooltips, pickers.
- `Guide`: tutorial masks and click blockers.
- `Toast`: transient feedback.
- `Loading`: blocking loading indicators and modal waiting.

Rules:
- Give layers stable names and sorting order policy.
- Keep popups/tooltips short-lived.
- Keep guide/loading layers explicit because they often intercept input.

## Package Strategy

Use startup preload for:
- Common controls, fonts, scrollbars, popup menu, button sounds, and small global UI packages.

Use feature-load on enter for:
- Large feature screens.
- Rarely used packages.
- Packages with heavy atlases or models.

Use lazy-load for:
- Optional popups.
- Feature-specific subviews.
- Remote/dynamic icon sets.

Avoid:
- Unloading common packages on every screen transition.
- Loading a package from inside every row/item renderer.
- Depending on package side effects without documenting them.

## Cross-Runtime Shape

Unity:
- Put package/config/extension registration in `Awake`.
- Bind `UIPanel.ui` and populate views in `Start`.
- Keep AssetBundle ownership explicit.

Cocos Creator:
- Create `GRoot` once per scene entry.
- Load packages asynchronously and create views inside callbacks.
- Dispose owned views in `onDestroy`.

LayaAir:
- Add `fgui.GRoot.inst.displayObject` to `Laya.stage` during boot.
- Ensure Laya resource loading finishes before `addPackage` when loading manually.

## Architecture Checklist

- Which object owns `GRoot` initialization?
- Which service owns package lifetime?
- Which object owns this view's disposal?
- Are binders/extensions registered before creation?
- Are common packages resident?
- Are async open/close races handled?
- Are visual state and business state separated?
