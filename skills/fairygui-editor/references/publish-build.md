# Publishing and Build

Use this file for FairyGUI publish settings, generated code, atlas planning, CLI publishing, and build pipeline rules.

Derived from:
- FairyGUI editor publishing documentation, including package settings, generated code, atlas settings, branch output, and command-line publishing.
- Official Unity, Cocos Creator, and LayaAir runtime packaging guidance and release-output conventions.

## Publish target rules

- Decide the target SDK before setting output: Unity, Cocos Creator, LayaAir, ThreeJS, or another runtime have different loader and extension expectations.
- Prefer binary format for modern projects. It is smaller, faster to parse, and produces less runtime garbage than XML definitions.
- Use forward slashes in publish paths when the same UI project is shared across Windows and macOS.
- Keep publish paths relative to the project root when possible. Use variables such as `{publish_file_name}`, `{branch_name}`, and project custom properties for CI-friendly output.
- Treat branch publish paths as first-class outputs. Do not mix branch output with trunk output unless the runtime loader is explicitly branch-aware.

## Package name vs published file name

- Published file name is used for loading a package file.
- Package name is used for creating objects from the package.
- Example rule: load with published file name/path, create with `UIPackage.CreateObject("PackageName", "ComponentName")` or SDK equivalent.
- When troubleshooting "package loaded but component missing", check both names separately.

## Atlas planning

- Use a max atlas size that matches the target platform; 2048 is a common baseline.
- Enable automatic page split only when the runtime and assets support it. Animations and bitmap fonts should not be split across pages.
- Split large backgrounds, rich-color images, and UI controls into different atlases when compression/filtering needs differ.
- For Unity, do not rely on editor PNG8 compression to save build memory. Set Unity texture import formats instead.
- For Unity ETC1/PVRTC alpha workflows, use editor alpha channel separation and set both output textures to compressed formats in Unity.
- Avoid mip maps for UI atlases unless the project has a deliberate world-space UI reason.

## Texture import expectations by runtime

- Unity: atlas textures should be `Default`/2D, not `Sprite`; disable mip maps for regular UI; choose filter/compression per atlas.
- Cocos Creator: published UI resources should live under `assets/resources` or a subdirectory for dynamic loading; atlas images can remain raw image assets.
- LayaAir: keep `fui` extension unless the Laya build pipeline converts it; if mini-game rawinflate is a problem, disable compressed descriptions in publish settings.

## Generated code

- Enable generated code only when the project accepts tighter designer-programmer coupling.
- Generate code into a stable source path and namespace/module that matches the runtime project.
- Use a member prefix such as `m_` to avoid child names colliding with base class properties like `icon` or `title`.
- Prefer index-based generated member lookup for performance when component child order is stable; prefer name-based lookup when designers reorder children often.
- Always call the generated binder `BindAll` before creating any UI that uses generated classes.
- Create generated UI classes with `CreateInstance`, not direct constructors.

## Command-line publishing

- Use command-line publishing for CI or repeatable local builds:

```bat
FairyGUI-Editor -batchmode -p project_desc_file [-b package_names] [-t branch_name] [-o output_path] [-logFile log_file_path]
```

- Run the command from the FairyGUI-Editor directory or ensure the executable is on `PATH`.
- Pass `-b` with comma-separated package names for partial publish; omit it to publish all packages.
- Use `-t` for branch name and `-o` to override output path.
- Use `-logFile` in automation. Treat missing logs as a startup/path problem first.

## Scripted publishing and export

- From FairyGUI 6.0+, use `-script` and `-scriptArg` to run plugin functions during command-line execution.
- Use `PublishHandler` for scripted package publishing.
- Use `ExportStringsHandler` when exporting localization strings.
- When a custom script takes over publishing, pass `-b -` to avoid default package publishing.

## Release validation checklist

- Confirm package file names, package names, and runtime load paths match.
- Confirm generated code is refreshed when component structure changes.
- Confirm atlases, split pages, alpha separation, and compression settings match the runtime project.
- Confirm preview-only list data or test-only loader assets are excluded from publish output.
- Confirm dependent packages are either packed together or loaded before use.
- Confirm publish output is reproducible from editor UI and command line.
