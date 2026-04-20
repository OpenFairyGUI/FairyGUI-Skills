# Cocos Creator Runtime Patterns

Use this file for compact Cocos Creator TypeScript examples distilled from official FairyGUI Cocos Creator demos.

Derived from:
- Official FairyGUI Cocos Creator examples for `DemoEntry`, `MainMenu`, `MailItem`, and `VirtualListDemo`.
- FairyGUI Cocos Creator documentation for package loading, `GRoot`, events, coordinates, and package lifetime.

## Scene entry pattern

- Create `GRoot` once in the scene entry component.
- Use Cocos component lifecycle for screen creation and cleanup.
- Keep UI project files outside `assets`; publish output goes under `assets/resources`.

```ts
onLoad() {
    fgui.GRoot.create();
    this.node.on("start_demo", this.onDemoStart, this);
    this.addComponent(MainMenu);
}
```

## Package load and screen creation

- Use paths relative to `resources`.
- Create by package name and component name after the package load callback.
- Use `makeFullScreen` for root screens.

```ts
fgui.UIPackage.loadPackage("UI/MainMenu", () => {
    this._view = fgui.UIPackage.createObject("MainMenu", "Main").asCom;
    this._view.makeFullScreen();
    fgui.GRoot.inst.addChild(this._view);
});
```

## Global overlay/close pattern

- Add cross-screen overlays directly to `GRoot`.
- Use relations to keep overlay controls pinned during resize.
- Use a high sorting order for global controls.

```ts
this._closeButton = fgui.UIPackage.createObject("MainMenu", "CloseButton");
this._closeButton.setPosition(fgui.GRoot.inst.width - this._closeButton.width - 10,
    fgui.GRoot.inst.height - this._closeButton.height - 10);
this._closeButton.addRelation(fgui.GRoot.inst, fgui.RelationType.Right_Right);
this._closeButton.addRelation(fgui.GRoot.inst, fgui.RelationType.Bottom_Bottom);
this._closeButton.sortingOrder = 100000;
this._closeButton.onClick(this.onDemoClosed, this);
fgui.GRoot.inst.addChild(this._closeButton);
```

## Extension class pattern

- Register extensions before creating rows.
- Extend the matching FairyGUI base class.
- Bind child references in `onConstruct`.

```ts
fgui.UIObjectFactory.setExtension("ui://VirtualList/mailItem", MailItem);

export default class MailItem extends fgui.GButton {
    private _timeText: fgui.GTextField = null!;

    protected onConstruct(): void {
        this._timeText = this.getChild("timeText", fgui.GTextField);
    }
}
```

## Virtual list pattern

- Load package, register extension, create view, then set up the list.
- Call `setVirtual` before assigning `numItems`.
- Bind the renderer to the component instance.

```ts
this._list = this._view.getChild("mailList", fgui.GList);
this._list.setVirtual();
this._list.itemRenderer = this.renderListItem.bind(this);
this._list.numItems = 1000;

private renderListItem(index: number, item: MailItem): void {
    item.setRead(index % 2 == 0);
    item.title = index + " Mail title here";
}
```

## Cleanup pattern

- Remove `GRoot` children with dispose when switching demo screens.
- Destroy the Cocos component that owns the current screen.
- Dispose the view in `onDestroy` when the component owns a persistent view reference.

```ts
onDestroy() {
    this._view.dispose();
}
```

## Event rules

- Prefer FairyGUI `onClick` and `fgui.Event` constants for UI input.
- Use FairyGUI coordinates and conversion APIs when mapping UI positions, not raw Cocos node coordinates.
