# Performance and Lifecycle

Use this file for UI lifetime, disposal, package memory, batching, draw calls, and runtime performance tradeoffs.

Derived from:
- FairyGUI Unity draw-call optimization and package memory documentation.
- Official runtime examples that show view disposal, root children cleanup, package unloading, and AssetBundle ownership boundaries.

## Component lifecycle

- Remove a view from its parent to hide it when it may be reused soon.
- Dispose a view when it will not be reused.
- Dispose or destroy owning host-engine components in their lifecycle hooks.
- Do not remove a package while visible components created from it still need assets.
- Prefer resident shared packages for common controls, fonts, scroll bars, popup menus, and button sounds.

## Package memory

- Use package unloading for rarely used feature UI, not for every screen switch.
- Use `UnloadAssets`/`ReloadAssets` in Unity when package definitions should stay but heavy assets can be released.
- For AssetBundle pipelines, decide whether FairyGUI or the host asset manager owns bundle unloading.

## Unity batching

- Use `fairyBatching` on top-level components or windows when draw calls matter.
- Do not enable `fairyBatching` on `GRoot`.
- Window already aligns well with `fairyBatching` usage; top-level full-screen panels are also good candidates.
- If runtime movement/resize of children changes overlap, call `InvalidateBatchingState` on the changed object.
- If a transition causes ordering problems during playback, prefer changing the transition design. Use `invalidateBatchingEveryFrame` only when necessary.

## Masks and draw calls

- Rectangular overflow masks are fastest.
- Custom masks and reverse masks often force material differences and can prevent draw-call batching.
- Stencil-based masks can have runtime-specific limitations. Check the target engine before relying on image masks or reverse masks.

## Lists and pooling

- Use virtual lists for large data sets.
- Use item pools for dynamic list rows.
- Reset all row visual state in renderers because items are reused.
- Avoid frequent create/destroy loops for list items and temporary effects.

## Input and hit-test cost

- Disable touch on Unity `UIPanel` for non-interactive HUD/UI.
- Use click-through for visual-only overlays.
- Avoid pixel hit tests unless necessary for irregular shapes.
