# Editor Workflows

Use this file for FairyGUI editor authoring standards: project layout, packages, components, lists, controllers, relations, transitions, adaptation, and data exposed to runtime.

Derived from:
- FairyGUI editor documentation for packages, components, controllers, relations, lists, transitions, adaptation, and design-time data.
- Official FairyGUI example UI projects for Unity and LayaAir-style package/component organization.

## Project and package rules

- Treat a package as the smallest editor-side UI module and publishing unit. Group UI by feature or shared foundation package, not by screen count alone.
- Keep reusable controls in shared packages such as `Basics`; keep feature screens in feature packages such as `Bag`, `Guide`, `VirtualList`, or `Transition`.
- Use stable package names because runtime creation uses the package name, while loading may use the published file name.
- Keep package resource URLs stable (`ui://Package/Item`) once runtime code or generated bindings reference them.
- Use package references intentionally. If a component in package A uses assets from package B, ensure the runtime loads package B before A when the target SDK does not auto-load dependencies.

## Component authoring

- Use components as the default composition unit. Prefer component nesting over assembling many unrelated runtime objects in code.
- Set component size, pivot, overflow, and relations in the editor whenever the layout is visual or resolution-dependent.
- Use `initial name` for framework components that must be named by convention, such as a window frame named `frame`.
- Use `custom data` for small runtime-readable metadata. Do not use editor custom properties as a runtime contract; they are for editor inspection.
- For full-screen panels, set relations in the editor or call `MakeFullScreen` and add a size relation at runtime. Do not position many separate panels manually in the host engine.

## Naming conventions

- Give runtime-bound children semantic names. Avoid relying on generated `n1`, `n2` names in code unless the component is generated and stable.
- Respect extension component conventions: common built-in names include `title`, `icon`, `button`, `bar`, `grip`, `list`, and `frame`, depending on component type.
- Use controllers with semantic names such as `c1`, `button`, `IsRead`, or a clearer project-specific name. Runtime code often reads controllers by name.
- Name transitions by role (`show`, `hide`, `t0`, `open`, `select`) and add frame labels when runtime code needs hooks or dynamic values.

## Controllers and relations

- Use controllers for mutually exclusive UI states, visual variants, tab pages, and list/page synchronization.
- Bind list selection or page controllers in the editor when the relationship is structural.
- Use relations for responsive placement and sizing. Prefer relations over hard-coded runtime coordinates for screen edges, center alignment, and stretched panels.
- When runtime code sets positions after creation, preserve editor-defined relations unless the object is deliberately detached from adaptive layout.

## Lists

- Choose list layout and overflow together. A single-row list with vertical scrolling is usually a design mismatch.
- Set a default item resource in the editor when runtime code will use `AddItemFromPool`, `SetVirtual`, or `numItems`.
- Use `publish time auto clear` for editor-only preview data so placeholder items do not ship in package definitions.
- Use object pooling for dynamic lists: create items with pool APIs and remove them with pool APIs. Do not mix `AddChild` with `RemoveChildrenToPool`.
- Use virtual lists for hundreds or thousands of rows. Requirements: scrolling enabled, default item set, `itemRenderer` defined, and virtual mode enabled before setting `numItems`.
- Call `EnsureBoundsCorrect` when code needs immediate item positions after list content changes.

## Transitions

- Build visual timing and curves in the editor; use runtime code for trigger logic, data injection, hooks, and playback control.
- Use labels for frames that runtime code must hook, retarget, or update with `SetValue`, `SetHook`, or equivalent APIs.
- Avoid relying on an unchecked first key value for repeated playback unless the current runtime value is intended to be reused.
- Add a final keyframe that restores state if the UI must return to its starting state after playback.
- For nested or looping transitions, remember completion callbacks wait for all nested content that can finish.

## Hit testing, masks, and click-through

- Use `click-through` on full-screen or overlay components that should not block underlying UI in empty areas.
- Prefer rectangular overflow masks for performance. Custom masks can break batching and have runtime-specific limitations.
- Use graph hit areas for polygon shapes and pixel hit tests only when the irregular shape is necessary.
- Keep pixel hit-test images in the same package as the component; do not replace them with loader content.

## Adaptation and design assets

- Use design images for alignment aid only; they are not published.
- Use branches, high-resolution suffixes, and adaptation settings consistently with the target runtime publish paths.
- For multi-resolution assets, align publish settings with preview/testing settings so editor preview and runtime package output match.
