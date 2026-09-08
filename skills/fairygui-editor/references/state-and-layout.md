# State and Layout

Use this file when designing FairyGUI editor state systems, responsive layout, relations, controllers, gears, branches, and scroll behavior.

Derived from:
- FairyGUI editor documentation for controllers, gears, relations, adaptation, groups, components, scroll panes, and branches.
- Runtime examples where editor-defined layout and state reduce host-engine code.

## Core Principle

Put visual state and adaptive layout in the editor when the rule is structural, visual, or designer-owned. Put state orchestration in runtime code when it depends on game data, async loading, permissions, or business logic.

## Controllers

Use when:
- A component has mutually exclusive visual states.
- Tabs, pages, button states, read/unread flags, fetched/unfetched flags, or view modes should switch multiple child properties together.
- A list selection or page index should drive another visual state.
- A business flow or scene mode has finite states such as loading/empty/content/error or normal/locked/claimed.

Avoid when:
- State is purely data value display, such as setting a text number.
- There are too many dynamic states to model as pages.

Rules:
- Name controllers by role when production code reads them.
- Use controller pages as a visual contract; runtime code should set selected index/page, not mutate every child manually.
- Name each independent state dimension and give its changing properties a clear owner. Do not enumerate every combination in one Controller or split components solely because they have several Controllers.
- Preserve existing page IDs, actions and bindings when modifying a state model. Controller/Gear, Relations, Transition and runtime writes should not compete over the same property.

## Gears

Use when:
- A child changes position, size, color, text, icon, visibility, animation, or other properties based on a controller.
- You want state changes to stay editable and previewable in the editor.

Avoid when:
- Runtime code must compute the property from live data every frame.
- The relationship is not tied to a finite controller state.

Rules:
- Prefer gears for visual variants rather than duplicating full components.
- Gear support depends on object type and SDK. Use supported Gear tweening for a simple state-driven change; use a Transition for a timed sequence with its own targets and playback behavior.
- Audit gear bindings after renaming controllers or moving children.

## Relations

Use when:
- Children should stay pinned, centered, stretched, or proportionally positioned as parent size changes.
- Full-screen screens should adapt to `GRoot` or parent panel size.
- Overlay buttons or close controls must remain attached to screen edges.

Avoid when:
- Runtime code intentionally takes full ownership of position/size.
- The object is part of a list layout; let the list layout own item positioning.

Rules:
- Prefer relations over host-engine coordinates for UI layout.
- Set initial positions, sizes and text autosizing first. Relations preserve offsets or size differences when targets change; they do not establish initial alignment.
- Add size relations for full-screen dynamic views.
- Keep relation chains simple; complex circular layout dependencies are hard to debug.
- Check representative widths and longer text; use width/height changes for layout and Scale for visual effects.

## Groups

Use when:
- You need to show/hide, move, or organize sibling objects together.
- The group is a design-time helper or layout grouping.

Avoid when:
- You need containment, clipping, scrolling, custom data, hit testing, or runtime extension.

Rule:
- If you catch yourself looking for children inside a group at runtime, it probably should be a component.

## Overflow and ScrollPane

Use component/list overflow scrolling when:
- Content can exceed the viewport.
- The UI needs drag scrolling, inertia, bounce, page mode, or scrollbars.

Avoid when:
- You only want to hide a decorative overflow; use hidden overflow/mask instead.
- Content is a list of repeated rows; use `List` with scrolling rather than a generic component full of manual children.

Rules:
- Match list layout direction with overflow direction.
- Use page mode only when each page is a deliberate UX unit.
- For edge effects in Unity, remember edge softness is runtime-specific.

## Adaptation

Use when:
- UI must work across aspect ratios and resolutions.
- Branches or high-resolution assets differ by target device.

Rules:
- Keep adaptive layout in the editor where possible.
- Use full-screen root components for screens and relate children inside them.
- Align publish settings with adaptation testing, especially high-resolution suffixes and branch paths.

## Branches

Use when:
- Assets or layout variants differ by platform, language, resolution class, or product channel.
- Shared content can stay in the main resources and only the differences need branch resources.

Avoid when:
- A controller can express the state inside one package cleanly.
- A text substitution or small layout adjustment already covers the actual difference.

Branch publication may include all branches for runtime selection or merge the active branch. Confirm the editor publish mode and SDK support, and select the branch before creating affected UI; do not assume changing it updates existing objects. UI branches are not Git branches. See the [official branch documentation](https://www.fairygui.com/docs/editor/branch).

## Layout Decision Checklist

- Can a designer preview and tune this rule? Put it in editor relations, controllers, gears, or transitions.
- Does the rule depend on live runtime data? Keep orchestration in code and bind through named children/controllers.
- Does the component represent a user task or business state clearly enough that runtime code can drive it by controller/page instead of patching children?
- Is this a data collection with shared selection, scrolling or refresh behavior? Consider List/Tree; repeated decorative layers alone do not establish that need.
- Is the content reusable? Use a component.
- Is the component full-screen? Use full-screen sizing plus relations, not host-engine manual placement.
