---
name: fairygui-editor
description: FairyGUI editor project engineering standards and workflows. Use when working with FairyGUI editor projects, packages, components, relations, controllers, lists, transitions, publishing settings, generated code settings, command-line publishing, editor plugins, custom inspectors, import/export, adaptation, or editor-side troubleshooting.
---

# FairyGUI Editor Skill

## Workflow

1. Confirm the target runtime before giving editor guidance: Unity, Cocos Creator, LayaAir, ThreeJS, or another SDK.
2. Classify the task:
   - Project/package/component authoring: read references/editor-workflows.md.
   - Choosing which editor component or object type to use: read references/component-selection.md.
   - Controllers, gears, relations, adaptation, layout, or state organization: read references/state-and-layout.md.
   - Transitions, masks, hit testing, custom data/properties, i18n, import/export, or other advanced editor features: read references/advanced-editor-features.md.
   - Publishing, generated code, atlas, branch, CLI, or CI: read references/publish-build.md.
   - Editor plugin, custom inspector, automation script, Lua, or TypeScript plugin API: read references/plugin-automation.md.
   - Broken preview, missing resources, event/hit-test surprises, publish errors, or runtime mismatch: read references/troubleshooting.md.
3. Prefer editor-side conventions that are stable across runtimes: package names, exported file names, component extension names, child names, controllers, relations, transitions, and publish settings.
4. Treat reference files as self-contained guidance. Do not require access to a local FairyGUI source checkout.
5. Return actionable steps: editor panels/settings first, then expected runtime impact.

## Output expectations

- Provide explicit menu paths, panel names, and key settings when possible.
- Distinguish package name from published file name whenever loading or creation is discussed.
- Warn when an editor choice affects runtime code: generated bindings, atlas split, binary format, compressed description, branch publish path, or item URL.
- Keep SKILL.md lean; load only the reference file that matches the task.
