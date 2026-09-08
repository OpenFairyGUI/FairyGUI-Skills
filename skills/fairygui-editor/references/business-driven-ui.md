# Business-Driven UI Design

Use this file when the question is not only "how do I build this UI" but "what should this UI be for the user, and how should FairyGUI model that intent."

Apply only the decisions needed by the request. Preserve supplied designs and existing project contracts; do not turn a small edit into a screen redesign. This is tool-independent guidance, not permission to edit a project or a promise of automation support.

Derived from:
- FairyGUI editor patterns where controllers, buttons, lists, relations, and transitions reflect product state rather than raw object assembly.
- Common game and application UI flows such as login, inventory, shop, guide, battle HUD, settings, rewards, and multi-step dialogs.

## Core Principle

Design the UI around user intent, business state, and scene flow first. Choose FairyGUI objects only after the team is clear about:
- Who is using the UI.
- What action they are trying to complete.
- What state or permission gates the action.
- What changes when the scene, mode, or selection changes.

Do not start from "place several images and texts." Start from "the player needs to claim a reward," "the operator needs to switch tabs," or "the user needs to compare items and confirm a choice."

## Decision Order

Use this order when authoring a component:

1. Define the user goal.
2. Define the screen or business state changes around that goal.
3. Choose the interaction pattern: button, tab, list item, popup, window, or passive display.
4. Decide which parts should be modeled by controllers/gears in the editor.
5. Leave live data, permissions, and async orchestration to runtime code.

## Reuse Existing Assets and Components

- Inspect existing resources and templates before creating another one. Match their structure, component type and supported instance properties, not only their names or appearance.
- Reference the existing template and image/font resources when instance title, icon or state can express the difference. Check resource URLs, IDs and cross-package dependencies rather than guessing them.
- Change a shared definition only after checking its consumers. If only one placement should differ, prefer a supported instance override or a scoped variant.
- Create wrapper components when independently reusable named variants are requested; ordinary placements can reference the template directly.
- Keep definition reuse separate from runtime pooling: shared templates still create independent instances. Atlas organization alone does not prove lower draw calls or memory use.

Use [component selection](component-selection.md) for object types and [state/layout guidance](state-and-layout.md) for Controllers, Relations and branches. Preserve existing references and unrelated content when editing.

## Model Actions, Not Decorations

Prefer `Button` when:
- The object triggers an action.
- The object must expose enabled, disabled, selected, pressed, or highlighted states.
- The action needs a stable runtime contract such as `title`, `icon`, and a named state controller.

Prefer plain `Component` or `Image` when:
- The object only displays information.
- The interaction belongs to a larger parent object and the child itself is not the action target.

Avoid turning every clickable area into a generic component with ad-hoc click code. If the user perceives the object as a button, model it as a `Button`.

## Model Business States with Controllers

Use controllers when the user sees mutually exclusive UI states such as:
- login / loading / success / error
- empty / content / locked
- normal / selected / disabled
- novice / advanced
- list page A / B / C
- scene mode switch such as bag tab, quest category, or battle HUD mode

Use runtime code to set controller pages based on business state. Do not manually toggle many child properties one by one if the state is finite and previewable in the editor.

Keep independent dimensions, such as selection and eligibility, separate when property ownership is clear. Define which Controller/Gear, layout rule or runtime field owns each changing property. A static label needs no Controller, and business eligibility or reward issuance stays in application logic.

## Typical Scenario Mapping

### Primary Action

If the UI contains a primary action such as `Buy`, `Claim`, `Start`, or `Confirm`:
- Make it a `Button`.
- Define disabled and busy states visually.
- Add a controller if the label/icon/state changes by business condition.
- Reserve runtime code for validation, pricing, cooldowns, and network flow.

### Scene or Mode Switching

If the screen changes between tabs, modes, or sub-scenes:
- Use a controller for the visual state contract.
- Use buttons, tabs, or list selection to drive the controller.
- Keep mode-specific layout differences in gears or page-specific objects.
- Avoid creating separate duplicated root components when one component with a controller can express the variation cleanly.

### Data Collections

If the user scans or compares many items:
- Use `List` or `Tree` based on data shape.
- Make each row/item a reusable component that reflects item state such as selected, owned, equipped, locked, or recommended.
- Let runtime data refresh the item; let editor controllers represent the row's visual states.

### Temporary Decisions

If the user makes a quick contextual choice:
- Use `Popup` for anchored short-lived interactions.
- Use `Window` when the flow is modal, multi-step, or lifecycle-heavy.

## Acceptance Examples

These are task outcomes to verify, not prevalidated fixtures or fixed operation JSON. Use the user's actual component names, IDs, content and supported APIs.

| Task | Structure to consider | Evidence to collect |
|---|---|---|
| Create daily, weekly and bonus cards from one template | Template references with different instance titles and existing icons | Saved references resolve correctly; template children and image bytes remain unchanged; rendered cards show the intended content. |
| Add locked, claimable and claimed reward states | One semantic state Controller with the needed text/look/display Gears | Cycle through all three states and back. Text, mark visibility, graying and touchability match the state table; actual input confirms interaction when required. A click does not prove reward issuance. |
| Widen a panel and add an entrance | Initial geometry plus Relations; a Transition for fade/slide motion | Test intended widths and longer text, check clipping and margins, replay without accumulating offsets, and recheck existing states. Measure exact timing only with a runtime facility that supports it. |

Use the available editor or authoring tools to inspect, edit and save when authorized. Reopen saved fields to prove persistence; use actual target-runtime observations and images for behavior. A screenshot alone cannot prove reference reuse, clickability or exact animation timing. Record unsupported checks as unverified and continue independent supported work. Do not invent operations, flatten requested editable behavior, or claim a tool ran when only instructions were provided.

## Audience and Frequency

Adjust complexity by audience:
- Frequent actions deserve stronger visual hierarchy, larger hit areas, fewer nested steps, and more direct controls.
- Rare or risky actions can use confirm dialogs, muted styling, or secondary placement.
- Expert-facing tools can expose denser information, but should still map repeated states through controllers instead of runtime child mutation.

## Editor-to-Runtime Contract

The editor should own:
- Visual states
- layout
- hierarchy
- component naming
- controller pages
- transitions

Runtime code should own:
- data
- permissions
- cooldowns
- async loading
- remote results
- feature flags
- navigation decisions

This means runtime code should usually set:
- controller page/index
- button enabled/disabled
- text/icon/value fields
- list item data

It should not usually rebuild visual structure for every business state.

## Review Checklist

- What user action or decision is this component helping with?
- Should this thing be a `Button` instead of a generic `Component`?
- Are finite scene or business states expressed as controllers?
- Are repeated states previewable in the editor?
- Is runtime code setting state through named controllers instead of patching many children?
- Is the visual hierarchy shaped around the user flow, not just around art slices?
