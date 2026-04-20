# Lists and Scrolling

Use this file for `GList`, virtual lists, object pools, scroll panes, tree views, pull-to-refresh, and list item effects.

Derived from:
- FairyGUI list and scroll-pane documentation.
- Official Unity, Cocos Creator, and LayaAir virtual-list and extension-list examples.

## Dynamic list rules

- Use `AddItemFromPool`/`RemoveChildrenToPool` pairs for reusable dynamic rows.
- Do not create rows with `CreateObject` and later remove them to a pool; that grows the pool incorrectly.
- Do not add rows from a pool and remove them without returning them to the pool unless they are intentionally retained elsewhere.
- Call `EnsureBoundsCorrect` before reading row positions immediately after a list update.

## Renderer list pattern

- Define an item renderer and assign `numItems` instead of manually creating every item when the list is data-driven.
- Renderers should be idempotent because pooled or virtualized items are reused.
- Use the row index and data source to fully refresh visible state: title, icon, selected/read flags, controllers, and transient effects.

## Virtual list requirements

- Enable scrolling in the editor.
- Set the default item resource in the editor or via `defaultItem`.
- Register extension class before the list creates items.
- Call `SetVirtual`/`setVirtual` before assigning `numItems`.
- Assign `itemRenderer` before setting or refreshing `numItems`.

Unity:

```csharp
_list = _mainView.GetChild("mailList").asList;
_list.SetVirtual();
_list.itemRenderer = RenderListItem;
_list.numItems = 1000;
```

Cocos Creator:

```ts
this._list = this._view.getChild("mailList", fgui.GList);
this._list.setVirtual();
this._list.itemRenderer = this.renderListItem.bind(this);
this._list.numItems = 1000;
```

LayaAir:

```ts
this._list = this._view.getChild("mailList").asList;
this._list.setVirtual();
this._list.itemRenderer = Laya.Handler.create(this, this.renderListItem, null, false);
this._list.numItems = 1000;
```

## Selection and scrolling

- Use `AddSelection(index, scrollToView)`/`addSelection(index, scrollToView)` when selecting items programmatically.
- Use `scrollPane.ScrollTop`/`scrollTop` and `ScrollBottom`/`scrollBottom` for jump controls.
- Use editor-bound selection/page controllers when UI state should follow selected item or page.
- Avoid setting item positions manually except for special offsets allowed by the selected list layout.

## Item effects

- For list entry effects, ensure bounds are correct, iterate visible children, and play per-row transition with staggered delay.
- Reset row visibility/state inside renderer or extension methods so reused rows do not inherit previous visual state.
