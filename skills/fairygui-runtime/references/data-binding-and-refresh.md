# Data Binding and Refresh

Use this file when deciding how runtime data flows into FairyGUI views, list items, controllers, loaders, and transitions.

## Contents

- Refresh Model
- Model Boundaries
- Refresh Order
- Controllers as View State
- Loader Refresh and Stale Results
- List Item Renderers
- Full vs Partial Refresh
- Event-to-Model Flow
- Refresh Checklist

Derived from:
- FairyGUI runtime examples that separate construction hooks, child caching, item renderers, controller updates, and transition hooks.
- Common UI architecture patterns for deterministic refresh and stale async protection.

## Refresh Model

Use this lifecycle:
- `Create`: instantiate the view after packages, binders, and extensions are ready.
- `Bind`: cache children/controllers/transitions and register events once.
- `Refresh(data)`: apply the current model to visual state.
- `Dispose`: remove long-lived listeners and release owned UI.

Rules:
- Do not register events inside every refresh.
- Do not create package objects inside every refresh unless the object is truly dynamic.
- Make refresh safe to call multiple times with the same data.

## Model Boundaries

Keep in models/services:
- Business state.
- Permissions.
- Inventory/account/player data.
- Remote URLs and loading state.
- Localization keys and formatted values.

Keep in FairyGUI:
- Visual hierarchy.
- Controllers/gears for visual state.
- Transitions.
- Layout and relations.
- Small view-local `data` fields used for identity or context.

Avoid:
- Storing full business models in `GObject.data`.
- Letting controllers become the source of business truth.
- Letting UI item renderers mutate global model state.

## Refresh Order

Prefer this order:
- Text and numeric values.
- Icons/loaders.
- Controller states.
- Progress/slider values.
- List counts/renderers.
- Transitions or effects that depend on final visual state.

Reason:
- Controllers/gears may change visibility or properties.
- Lists may reuse items.
- Transitions should start after the target state is coherent.

## Controllers as View State

Use controllers for:
- Visual mode.
- Read/unread.
- Equipped/locked/selected.
- Loading/empty/error/content states.
- Tab/page selection.

Avoid:
- Encoding open-ended business data as pages.
- Using controller page names as the only source of application state.

Pattern:

```csharp
readController.selectedIndex = model.IsRead ? 1 : 0;
titleText.text = model.Title;
```

## Loader Refresh and Stale Results

Use loaders for runtime-selected content.

Rules:
- Assign a placeholder before async content loads.
- Keep a version/token on the view or item when loading remote content.
- Ignore late results if the item was reused or refreshed with different data.
- Clear loader URLs when a row becomes empty or recycled.

Example:

```ts
const version = ++this._iconVersion;
this.iconLoader.url = placeholderUrl;
loadIcon(model.iconId).then(url => {
    if (version !== this._iconVersion) return;
    this.iconLoader.url = url;
});
```

## List Item Renderers

Renderer rules:
- Renderers must be idempotent.
- Renderers must fully set all visible state, not only changed fields.
- Renderers must reset transient state from previous data.
- Renderers must not register duplicate events.
- Renderers must not load packages.

Good row API:

```ts
item.setRead(data.isRead);
item.setFetched(data.isFetched);
item.setTime(data.timeText);
item.title = data.title;
```

Avoid:
- Setting only title and leaving previous controller states untouched.
- Starting row effects every time the list scrolls unless the effect is deliberate.
- Disposing row objects owned by the list pool.

## Full vs Partial Refresh

Use full refresh when:
- The model changed broadly.
- A screen opens.
- Localization/theme changes.
- Package or controller structure changed.

Use partial refresh when:
- One field changed and the view is stable.
- One list row changed and you can identify the visible item.
- A loader result arrives for a still-current token.

For virtual lists:
- Update the backing data first.
- Refresh `numItems` when item count changes.
- Re-render visible items or the specific visible row when only data changes.

## Event-to-Model Flow

Recommended flow:
- User event occurs in view.
- View calls presenter/controller/application callback with intent.
- Application updates model.
- View refreshes from the updated model.

Avoid:
- View directly mutating deep business state and then manually patching UI.
- Event handlers that both change model and partially update unrelated visual state.

## Refresh Checklist

- Are events registered once?
- Is refresh idempotent?
- Are all controller states set every time?
- Are loader stale results guarded?
- Are list rows fully reset?
- Does UI read from model instead of becoming the model?
- Is transition/effect playback intentional for this refresh?
