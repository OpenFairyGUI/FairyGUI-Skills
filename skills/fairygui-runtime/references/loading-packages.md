# Package Loading

Use this file for runtime package loading, unloading, asset bundles, resource paths, and dependency handling.

Derived from:
- FairyGUI package-loading documentation for Unity, Cocos Creator, and LayaAir.
- Official AssetBundle, asynchronous package loading, and package-owned loading examples.

## Universal rules

- Load by published file path/name; create objects by editor package name and component name.
- Load dependent packages before creating components that reference them.
- Register generated binders and package item extensions before creating objects from the package.
- Avoid frequent package load/unload for commonly used UI. Keep shared/basic packages resident.
- Dispose components before removing their package; package removal makes existing components unable to display assets correctly.

## Unity loading

- Resources path:

```csharp
UIPackage.AddPackage("UI/Basics");
GComponent view = UIPackage.CreateObject("Basics", "Main").asCom;
GRoot.inst.AddChild(view);
```

- Single AssetBundle: load the bundle yourself, then call `UIPackage.AddPackage(bundle)`.
- Two AssetBundles: put the `.bytes` definition in one bundle and atlas/audio assets in the resource bundle, then call `UIPackage.AddPackage(descBundle, resBundle)`.
- Custom sync loader: return assets from the callback and specify the proper destroy method.
- Custom async loader: pass description bytes and call `item.owner.SetItemAsset(item, res, destroyMethod)` when each asset finishes loading.
- `AddPackage(path)` deduplicates by path key; AssetBundle overloads do not deduplicate, so the project must guard against duplicates.

## Unity package memory

- `UIPackage.LoadAllAssets` preloads package assets.
- `UIPackage.RemovePackage` unloads package assets and, for FairyGUI-owned bundles, can unload bundles.
- `UIPackage.UnloadAssets` releases loaded assets without removing package definitions; `ReloadAssets` restores them.
- Set `UIPackage.unloadBundleByFGUI = false` when the host asset system owns bundle lifetime.

## Cocos Creator loading

- FairyGUI can load the package:

```ts
fgui.UIPackage.loadPackage("UI/Bag", () => {
  const view = fgui.UIPackage.createObject("Bag", "Main").asCom;
  fgui.GRoot.inst.addChild(view);
});
```

- Or the host can load resources first, then call `fgui.UIPackage.addPackage("UI/Bag")`.
- Use paths relative to `resources`.
- Use `fgui.UIPackage.removePackage("Bag")` only after disposing UI created from it.

## LayaAir loading

- Manual flow: load package resources through Laya, then call `fgui.UIPackage.addPackage`.
- FairyGUI-owned flow:

```ts
fgui.UIPackage.loadPackage("resources/ui/VirtualList", Laya.Handler.create(this, this.onUILoaded));
```

- If description compression is disabled, `rawinflate.min.js` is not needed.
- If loading fails, inspect the Laya loading pipeline first; FairyGUI expects the resources to be available before `addPackage`.
