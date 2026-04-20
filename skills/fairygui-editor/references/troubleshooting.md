# Troubleshooting

Use this file for editor-side diagnosis. Start from the symptom, identify whether the root cause is editor setup, publish output, runtime loading, or component design.

Derived from:
- FairyGUI FAQ and editor documentation for publishing, components, lists, transitions, hit testing, masks, and generated code.
- Common mismatch patterns between editor package settings and runtime loading/creation code.

## First-pass triage

- Ask which runtime consumes the publish output and where the output files are written.
- Compare editor package name, published file name, and runtime loading path.
- Check whether the issue happens in editor preview, after publish, or only after runtime loading.
- Check whether generated code was refreshed and whether the runtime called binder registration before creating UI.
- For command-line issues, check executable path, working directory, project descriptor path, and log file.

## Package loads but UI cannot be created

- Verify the loader uses the published file name/path, not necessarily the package name.
- Verify creation uses the package name and component name.
- Verify dependencies are loaded first when components reference another package.
- Verify branch output is not being mixed with trunk package definitions.
- Verify binary/compressed description settings match the runtime SDK version and loader pipeline.

## Missing atlas, white textures, or broken images

- Unity: atlas texture should be 2D `Default`, not `Sprite`; disable mip maps for regular UI.
- Unity: if using separated alpha, ensure both color and alpha textures are present and imported with the intended compression.
- Cocos Creator: keep published resources under `resources` for `loadPackage`; do not expect scene references to load UI packages.
- LayaAir: ensure assets are loaded before `addPackage` if using manual loading; FairyGUI does not own the Laya loader pipeline.
- Check resource exclusion settings for loader test images or editor-only placeholders.

## Generated code issues

- Call `BindAll` before any package object creation that expects generated classes.
- Use generated class `CreateInstance`; do not call constructors directly.
- Regenerate code after renaming, deleting, reordering, or changing child objects if generated binding uses indices.
- Use member prefixes to avoid collisions with built-in properties like `icon` and `title`.

## List problems

- If items do not display, check default item resource, overflow scroll mode, `itemRenderer`, and `numItems`.
- If virtual list fails, ensure virtual mode is enabled before assigning `numItems`.
- If dynamic lists leak or grow memory, check that pool creation and pool removal APIs are paired.
- If item coordinates are read too early, call `EnsureBoundsCorrect` before reading layout results.

## Click, mask, and hit-test surprises

- Full-screen transparent components block input unless click-through is enabled.
- Images and plain text are not touchable by default; a click-through component containing only them may become fully transparent to input.
- Custom masks can change hit-testing and batching behavior.
- Pixel hit-test images must stay in the same package and cannot be replaced by loader content.

## Transition surprises

- A transition leaves the component at its last frame unless a final frame restores the previous state.
- Replayed transitions can reuse current values when the first key value is unchecked.
- Hooks, value overrides, duration changes, and target changes depend on frame labels.
- Retarget a transition only while the transition is stopped.
