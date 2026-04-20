# Component Binding and Extensions

Use this file for generated code, manual child lookup, UIObjectFactory extensions, windows, and data binding patterns.

Derived from:
- FairyGUI generated-code publishing guidance.
- Official Unity and Cocos Creator extension-class examples for package item binding.
- Unity Puerts runtime callback bridging patterns.

## Binding strategy

- Use generated code for stable, designer-programmer agreed components.
- Use manual lookup for experiments, small views, or components whose child structure changes frequently.
- Use `UIObjectFactory` extensions for item classes, custom buttons, list rows, windows, and components with behavior.
- Register extensions before any package object that uses the item URL is created.

## Generated code rules

- Call the generated binder `BindAll` once during boot and before creating matching UI.
- Create generated components through `CreateInstance`.
- Regenerate code after component child names, order, or types change.
- Prefer member prefixing to avoid collisions with inherited fields/properties.
- If generated binding uses index lookup, treat child order as a code contract. If designers reorder often, use name lookup even if it is slower.

## Manual lookup rules

- Cache child references after construction/load callback:

```csharp
_mainView = GetComponent<UIPanel>().ui;
_list = _mainView.GetChild("mailList").asList;
_controller = _mainView.GetController("c1");
```

- Use typed getters in TypeScript runtimes when available:

```ts
this._list = this._view.getChild("mailList", fgui.GList);
```

- Keep child names semantic and stable. Avoid scattering `"n6"` style lookups through production code unless generated from fixed examples.

## Extension classes

- Unity C#:

```csharp
UIPackage.AddPackage("UI/Extension");
UIObjectFactory.SetPackageItemExtension("ui://Extension/mailItem", typeof(MailItem));
```

- Cocos Creator TypeScript:

```ts
fgui.UIObjectFactory.setExtension("ui://VirtualList/mailItem", MailItem);
```

- LayaAir TypeScript:

```ts
fgui.UIObjectFactory.setExtension("ui://VirtualList/mailItem", MailItem);
```

- In extension classes, use construction hooks to bind children:
  - Unity C#: override `ConstructFromXML`.
  - Cocos/Laya TypeScript: implement `onConstruct`.
  - Unity Puerts: assign `__onConstruct` and delegate to a TypeScript method.

## Extension class shape

- Inherit from the matching FairyGUI base type. A button-like item should extend `GButton`; a plain component should extend `GComponent`.
- Cache controllers, text fields, and transitions in the construction hook.
- Expose small intent methods such as `setRead`, `setFetched`, `setTime`, or `playEffect` instead of making callers know child names.
- Keep transient data outside UI objects unless it is genuinely view state.

## Window patterns

- For FairyGUI `Window`, set `contentPane` in initialization and use show/hide hooks for event binding, refresh, and cleanup.
- In Unity Puerts, assign `__onInit`, `__onShown`, and `__onHide` delegates to bridge FairyGUI virtual callbacks.
- Use editor components for the content pane; use the runtime window class for lifecycle and orchestration.
