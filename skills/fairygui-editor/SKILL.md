---
name: fairygui-editor
description: Archived FairyGUI editor reference guidance. Use for component reuse, business-driven UI design, Controllers/Gears, Relations, lists, Transitions, publishing, editor plugins, adaptation or troubleshooting. Verify advice against the installed editor and runtime version; this Skill bundles no tools.
---

# FairyGUI Editor Skill

## Maintenance status

Final edition `v1.1.0` (2026-09-08). This repository is archived and no longer maintained. The references remain usable, but are not a current-version compatibility guarantee.

Use [OpenFairyGUI](https://github.com/OpenFairyGUI/OpenFairyGUI) for project SDK/CLI/MCP automation, or [FairyGUI-Maker](https://github.com/OpenFairyGUI/FairyGUI-Maker) for Agent authoring and preview workflows. Their scopes do not replace all editor/runtime guidance here. Continue the requested task with available capabilities; these links do not authorize installation, connection or project mutation. Check relevant guidance against the user's actual editor/SDK and preserve discussion/review as read-only.

## Workflow

1. Identify the target editor/runtime version from project configuration and installed dependencies; ask only if a material choice cannot be inferred. Unity, Cocos Creator, LayaAir, ThreeJS and other SDKs differ.
2. Classify the task:
   - Project/package/component authoring: read references/editor-workflows.md.
   - Business-driven UI decisions, reuse and outcome-based acceptance: read [references/business-driven-ui.md](references/business-driven-ui.md).
   - Choosing which editor component or object type to use: read references/component-selection.md.
   - Controllers, gears, relations, adaptation, layout, or state organization: read references/state-and-layout.md.
   - Transitions, masks, hit testing, custom data/properties, i18n, import/export, or other advanced editor features: read references/advanced-editor-features.md.
   - Publishing, generated code, atlas, branch, CLI, or CI: read references/publish-build.md.
   - Editor plugin, custom inspector, automation script, Lua, or TypeScript plugin API: read references/plugin-automation.md.
   - Broken preview, missing resources, event/hit-test surprises, publish errors, or runtime mismatch: read references/troubleshooting.md.
3. Prefer editor-side conventions that are stable across runtimes: package names, exported file names, component extension names, child names, controllers, relations, transitions, and publish settings.
4. Start from user intent instead of object assembly: identify the action, state, audience, and frequency before choosing components or controllers.
5. Treat reference files as self-contained guidance. Do not require access to a local FairyGUI source checkout.
6. Return actionable steps: editor panels/settings first, then expected runtime impact.

## Output expectations

- Provide explicit menu paths, panel names, and key settings when possible.
- Distinguish package name from published file name whenever loading or creation is discussed.
- Warn when an editor choice affects runtime code: generated bindings, atlas split, binary format, compressed description, branch publish path, or item URL.
- Keep SKILL.md lean; load only the reference file that matches the task.
