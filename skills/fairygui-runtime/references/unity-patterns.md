# Unity Runtime Patterns

Use this file for compact Unity C# examples distilled from official FairyGUI Unity demos.

Derived from:
- Official FairyGUI Unity examples for `Basics`, `BundleUsage`, `Extension`, `Transition`, and `VirtualList`.
- FairyGUI Unity documentation for `UIPanel`, package loading, AssetBundle loading, events, batching, and memory.

## UIPanel scene pattern

- Load packages, global UI config, generated binders, and extensions in `Awake`.
- Bind `UIPanel.ui`, child references, events, and lists in `Start`.
- Use `UIPanel` when UI should share a GameObject lifecycle.

```csharp
void Awake()
{
    UIPackage.AddPackage("UI/Basics");
    UIConfig.defaultFont = "Microsoft YaHei";
    UIConfig.verticalScrollBar = "ui://Basics/ScrollBar_VT";
    UIConfig.horizontalScrollBar = "ui://Basics/ScrollBar_HZ";
    UIConfig.popupMenu = "ui://Basics/PopupMenu";
}

void Start()
{
    GComponent view = GetComponent<UIPanel>().ui;
    view.GetChild("btn_Back").onClick.Add(OnBack);
}
```

## Dynamic package/object pattern

- Use dynamic creation for code-owned screens, temporary effects, and packages not represented by scene `UIPanel`.
- Make full-screen views relate to `GRoot` so they follow resolution changes.

```csharp
UIPackage.AddPackage("UI/BundleUsage");
GComponent view = UIPackage.CreateObject("BundleUsage", "Main").asCom;
view.fairyBatching = true;
view.MakeFullScreen();
view.AddRelation(GRoot.inst, RelationType.Size);
GRoot.inst.AddChild(view);
view.GetTransition("t0").Play();
```

## AssetBundle package pattern

- Load bundles with Unity first.
- Use `UIPackage.AddPackage(bundle)` for one-bundle packages.
- Use the descriptor/resource bundle overload for split descriptor and atlas bundles.
- Guard duplicate package registration yourself for bundle overloads.

## Extension class pattern

- Register before creating the package item.
- Extend the correct base class, commonly `GButton` for list rows that behave like selectable buttons.
- Bind children/controllers/transitions in `ConstructFromXML`.

```csharp
void Awake()
{
    UIPackage.AddPackage("UI/Extension");
    UIObjectFactory.SetPackageItemExtension("ui://Extension/mailItem", typeof(MailItem));
}

public class MailItem : GButton
{
    GTextField _timeText;
    Controller _readController;

    public override void ConstructFromXML(FairyGUI.Utils.XML cxml)
    {
        base.ConstructFromXML(cxml);
        _timeText = GetChild("timeText").asTextField;
        _readController = GetController("IsRead");
    }
}
```

## Virtual list pattern

- Register row extension first.
- Call `SetVirtual` before assigning `numItems`.
- Fully refresh row state in the renderer because rows are reused.

```csharp
_list = _mainView.GetChild("mailList").asList;
_list.SetVirtual();
_list.itemRenderer = RenderListItem;
_list.numItems = 1000;

void RenderListItem(int index, GObject obj)
{
    MailItem item = (MailItem)obj;
    item.setRead(index % 2 == 0);
    item.title = index + " Mail title here";
}
```

## Transition pattern

- Use transition callbacks to restore state or remove temporary components.
- Use frame labels and hooks for dynamic data injection.

```csharp
Transition t = target.GetTransition("t0");
t.Play(() => GRoot.inst.RemoveChild(target));
```

## Cleanup and performance

- Remove reusable views from parent; dispose views that will not be reused.
- Keep shared packages resident and unload rarely used packages only after their views are disposed.
- Enable `fairyBatching` on top-level complex panels or windows, never on `GRoot`.
