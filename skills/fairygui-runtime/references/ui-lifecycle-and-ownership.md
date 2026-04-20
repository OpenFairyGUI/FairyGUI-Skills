# UI Lifecycle and Ownership

Use this file when deciding who creates, hides, disposes, unloads, or refreshes FairyGUI views and packages.

Derived from:
- FairyGUI runtime documentation for package loading/unloading, Window lifecycle, UIPanel lifecycle, and package asset memory.
- Official demos that remove root children with dispose, destroy owning host components, and keep package loading separate from view cleanup.

## Ownership Rules

- The object that creates a view owns its disposal unless ownership is explicitly transferred.
- A package manager owns package lifetime; feature views do not unload packages directly.
- A view may depend on a package, but package removal must wait until all dependent views are disposed.
- A list owns pooled items; callers must not dispose items returned to the list pool.
- A `Window` owns its content pane lifecycle according to the project's window policy.
- A Unity `UIPanel` is tied to the GameObject lifecycle unless the UI is explicitly detached.

## Hide, Remove, Dispose, Destroy

Use hide/remove when:
- The view is likely to be reused soon.
- The package should remain resident.
- State should be preserved.

Use dispose/destroy when:
- The view will not be reused.
- The owner is closing permanently.
- The scene/component that owns the view is being destroyed.

Use package unload when:
- All views created from the package are disposed.
- The package is large or rarely used.
- The host asset system is ready to release underlying assets.

Avoid:
- Calling `RemovePackage` while UI from that package is visible.
- Disposing pooled list items manually.
- Keeping hidden views forever without a memory policy.

## View Lifecycle Pattern

Recommended stages:
- `Create`: load package and instantiate view.
- `Bind`: cache child references, controllers, transitions, and register events once.
- `Show`: add to parent/root/window and play show transition if needed.
- `Refresh`: apply model data to text, controllers, loaders, lists, and transitions.
- `Hide`: stop transient interactions and remove from parent if reusable.
- `Dispose`: remove events, stop tweens/transitions if needed, dispose owned view.

## Async Open/Close Races

Guard against:
- Package load finishes after the user closes the screen.
- Remote icon/avatar finishes after the list item was reused.
- Extension registration happens after objects were already created.
- A transition callback fires after the owner was disposed.

Rules:
- Use an open token, generation number, or cancellation flag for async UI creation.
- Check owner/view validity in every async callback.
- Register binders and extensions before starting object creation.
- Clear or ignore callbacks when closing/disposal starts.

Example pattern:

```ts
const token = ++this._openToken;
fgui.UIPackage.loadPackage("UI/Inventory", () => {
    if (token !== this._openToken || this._disposed) return;
    this.createAndShowInventory();
});
```

## Events and Callback Cleanup

- Register view events once during bind.
- Remove long-lived event subscriptions during dispose when the event source outlives the view.
- Short-lived child events usually disappear with view disposal, but global/root/stage events must be removed.
- Stop or detach tweens that target disposed views.
- Treat transition callbacks as asynchronous callbacks and guard owner validity.

## Unity Notes

- `UIPanel` creates/destroys its UI with the GameObject lifecycle.
- If `UIPanel` depends on packages outside `Resources`, load them before the panel creates UI.
- For dynamic views, dispose manually when the owning MonoBehaviour or UI manager closes them.
- Decide whether FairyGUI or the host asset system owns AssetBundle unloading.

## Cocos Creator Notes

- Create FairyGUI views in component lifecycle methods or package callbacks.
- Dispose owned FairyGUI views in `onDestroy`.
- Remove `GRoot` children with dispose during screen switches when the whole screen is replaced.

## LayaAir Notes

- If a screen class owns a view, provide a `destroy`/dispose method and call it before replacing the screen.
- Remove root children with dispose when clearing a full UI mode.
- Reusable handlers should not capture stale view instances after disposal.

## Lifecycle Checklist

- Who owns this view?
- Who owns this package?
- Can this view outlive its package?
- Are global events removed?
- Are tweens/transitions stopped or guarded?
- Can async callbacks arrive after close?
- Are list items returned to pools rather than disposed?
