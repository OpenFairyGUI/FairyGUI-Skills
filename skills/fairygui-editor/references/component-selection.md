# Component Selection

Use this file when choosing FairyGUI editor object types or reviewing whether a UI design uses the right component for its job.

Derived from:
- FairyGUI editor documentation for images, loaders, text, groups, components, controls, lists, trees, popups, windows, and transitions.
- Common production mistakes seen when visual editor decisions leak into runtime code.

## Selection Matrix

| Need | Prefer | Avoid |
| --- | --- | --- |
| Static package atlas image | Image | Loader |
| Runtime-swapped icon/avatar/content | Loader | Image |
| Simple vector shape or hit area | Graph | Imported bitmap |
| Reusable composed UI | Component | Group as a container |
| Visual state variants | Controller + gears | Duplicated components |
| Repeated rows or grids | List | Manual child creation |
| Huge data set | Virtual List | Full physical list |
| Hierarchical data | Tree | Nested manual lists |
| Modal lifecycle | Window | Raw component with ad-hoc state |
| Context menu or temporary picker | Popup | Permanent panel |
| Numeric progress | ProgressBar | Hand-updated masks |
| Numeric input by dragging | Slider | Custom drag math |
| Visual feedback motion | Transition | Host-engine tween only |

## Image

Use when:
- The asset is static and belongs to the package atlas.
- The image is selected by controller/gears rather than arbitrary runtime data.
- Atlas packing and draw-call friendliness matter.

Avoid when:
- The displayed texture comes from a URL, CDN, inventory item, Addressable asset, or user avatar.
- The slot must display arbitrary package URLs at runtime.

Prefer `Loader` for dynamic external or cross-package content.

## Loader

Use when:
- Runtime code chooses the displayed image, component, movie clip, or external content by URL.
- A slot can point to another package item.
- UI needs dynamic icons, avatars, item previews, or optional content.

Avoid when:
- The image is fixed in the design and should be packed into the component's atlas.
- Designers need precise direct editing of the visual asset as part of the component.

## Graph

Use when:
- The shape is simple: rectangle, rounded rectangle, polygon, line, pie, or custom hit area.
- You need a mask or hit-test area that is easier to maintain as geometry than as a bitmap.
- Runtime code may procedurally adjust shape data.

Avoid when:
- The visual requires detailed art, gradients, or texture-specific styling.
- A graph is being used as a fake container; use `Component` for containment.

## MovieClip

Use when:
- The animation is frame-based art exported with the UI package.
- The animation should be controlled as a UI asset, including inside transitions.

Avoid when:
- The behavior is better represented as a timeline transition over existing objects.
- The content is a skeletal animation, model, or particle effect handled by a runtime extension.

## Text, Rich Text, and Input Text

Use `Text` when:
- Plain label content is enough.
- The field needs normal localization, controller text changes, or runtime assignment.

Use `Rich Text` when:
- Inline colors, links, icons, UBB-like markup, or mixed content are required.

Use `Input Text` when:
- The user edits text at runtime.

Avoid when:
- Text is converted to images only for convenience. Prefer real text for localization, accessibility of data, and runtime updates.

## Group

Use when:
- You need to move, show/hide, or apply layout behavior to a set of siblings.
- The grouped children still belong structurally to the parent component.

Avoid when:
- You need a reusable container, clipping, scrolling, custom data, hit testing, or nested composition.

Prefer `Component` for reusable UI and structural containment.

## Component

Use when:
- The UI is reusable, nested, has its own size, relations, custom data, controllers, transitions, or runtime class.
- You need a container that can clip, scroll, mask, expose properties, or be extended.

Avoid when:
- You only need to visually align a few sibling objects; `Group` may be enough.

## Button

Use when:
- The component is clickable and has states such as up, over, down, selected, or disabled.
- The design follows FairyGUI conventions like `title`, `icon`, and `button` controller.

Avoid when:
- The object is only a static decoration with no interaction.

## ComboBox

Use when:
- The user chooses one option from a compact set.
- Options can be represented as text/icon items and a popup list.

Avoid when:
- The option set is large, searchable, hierarchical, or needs custom filtering. Prefer a dedicated popup/window with list logic.

## ProgressBar

Use when:
- A value fills or changes a bar-like visual.
- Runtime code updates numeric progress frequently.

Avoid when:
- The visual is not value-driven and should be a transition instead.

## Slider

Use when:
- The user directly manipulates a numeric value.
- The value range maps cleanly to a grip/bar control.

Avoid when:
- The control is only displaying progress. Use `ProgressBar`.

## ScrollBar

Use when:
- A custom scroll pane or list needs branded scroll controls.
- Shared scrollbar resources should be configured through `UIConfig`.

Avoid when:
- You are trying to manually implement scrolling. Use component/list overflow scrolling.

## List

Use when:
- The UI shows repeated items in rows, columns, flow, or pages.
- Items should use pooling, selection mode, item renderer, or virtual list behavior.

Avoid when:
- You need only a handful of fixed decorative children.
- Each child has unrelated structure and lifecycle.

## Tree

Use when:
- Data is hierarchical and users expand/collapse nodes.
- The UI naturally behaves like a tree view.

Avoid when:
- The hierarchy is shallow and better represented by tabs, pages, or nested lists.

## Popup

Use when:
- UI appears temporarily near an anchor: menus, quick choices, tooltips, small pickers.
- The popup should dismiss when focus/context changes.

Avoid when:
- The UI is a full workflow or modal state. Prefer `Window`.

## Window

Use when:
- The UI needs show/hide lifecycle, modal behavior, centering, close handling, or window-level animation.
- Runtime code benefits from `Window` hooks instead of ad-hoc component state.

Avoid when:
- The panel is a permanent part of a screen layout.

## Transition

Use when:
- The motion belongs to the UI composition and designers should tune timing/curves.
- Runtime code only needs to trigger, hook, or inject values.

Avoid when:
- The animation is mostly gameplay/engine object behavior outside FairyGUI's display tree.
