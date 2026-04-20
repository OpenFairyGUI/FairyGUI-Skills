# LayaAir Runtime Patterns

Use this file for compact LayaAir TypeScript/JavaScript examples distilled from official FairyGUI LayaAir demos.

Derived from:
- Official FairyGUI LayaAir examples for `DemoEntry`, `VirtualListDemo`, and shared demo screen structure.
- FairyGUI LayaAir documentation for plugin setup, package loading, compressed descriptions, and root initialization.

## Boot pattern

- Configure `fairygui.js` as a Laya plugin.
- Add `fgui.GRoot.inst.displayObject` to `Laya.stage` before showing UI.
- Include `rawinflate.min.js` only when compressed package descriptions are published.

```ts
Laya.stage.addChild(fgui.GRoot.inst.displayObject);
fgui.UIConfig.defaultFont = "SimSun";
fgui.UIConfig.verticalScrollBar = "ui://Basic/ScrollBar_VT";
fgui.UIConfig.horizontalScrollBar = "ui://Basic/ScrollBar_HZ";
fgui.UIConfig.popupMenu = "ui://Basic/PopupMenu";
fgui.UIConfig.buttonSound = "ui://Basic/click";
```

## Package loading pattern

- Use package paths matching the published resource location.
- If Laya loads assets manually, call `fgui.UIPackage.addPackage` only after assets are loaded.
- For package-owned loading, use `fgui.UIPackage.loadPackage`.

```ts
fgui.UIPackage.loadPackage("resources/ui/VirtualList",
    Laya.Handler.create(this, this.onUILoaded));
```

## Screen switch pattern

- Put global controls on `GRoot`.
- Use relations and sorting order for resize-safe overlays.
- On close, remove `GRoot` children with dispose and recreate the menu/screen owner.

```ts
this._closeButton.setXY(fgui.GRoot.inst.width - this._closeButton.width - 10,
    fgui.GRoot.inst.height - this._closeButton.height - 10);
this._closeButton.addRelation(fgui.GRoot.inst, fgui.RelationType.Right_Right);
this._closeButton.addRelation(fgui.GRoot.inst, fgui.RelationType.Bottom_Bottom);
this._closeButton.sortingOrder = 100000;
this._closeButton.onClick(this, this.onDemoClosed);
```

## Extension and virtual list pattern

- Register row extensions before the list creates items.
- Use `setVirtual` before assigning `numItems`.
- Use `Laya.Handler.create(this, callback, null, false)` for list renderers so the handler is reusable.

```ts
fgui.UIObjectFactory.setExtension("ui://VirtualList/mailItem", MailItem);

this._list = this._view.getChild("mailList").asList;
this._list.setVirtual();
this._list.itemRenderer = Laya.Handler.create(this, this.renderListItem, null, false);
this._list.numItems = 1000;
```

## Renderer pattern

```ts
private renderListItem(index: number, obj: fgui.GObject): void {
    const item = obj as MailItem;
    item.setRead(index % 2 == 0);
    item.title = index + " Mail title here";
}
```

## Mini-game notes

- If `rawinflate` causes platform issues, disable compressed descriptions in FairyGUI publish settings.
- Keep `fui` output unless the Laya build pipeline is expected to transform it for the target platform.
