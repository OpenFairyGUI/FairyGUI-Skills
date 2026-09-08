# FairyGUI Skills

[中文说明](README_zh.md)

> **Final edition: v1.1.0 — maintenance ended on 2026-09-08.**
> This repository is archived and no longer actively maintained. The existing editor and runtime Skills remain available as reference material.

## Where to go next

| Goal | Maintained project |
|---|---|
| FairyGUI project I/O, SDK/CLI/MCP automation, transactions, publishing and recovery | [OpenFairyGUI](https://github.com/OpenFairyGUI/OpenFairyGUI) |
| Agent-assisted UI creation/editing, reusable components, asset analysis, Viewer previews and published-artifact validation | [FairyGUI-Maker](https://github.com/OpenFairyGUI/FairyGUI-Maker) and its [current Skill](https://github.com/OpenFairyGUI/FairyGUI-Maker/tree/main/.agents/skills/use-fairygui-maker) |

These projects do not fully replace this repository's Unity, Cocos Creator, LayaAir and other runtime coding guidance. That material is retained here without ongoing updates; verify relevant APIs and examples against the engine and SDK version you actually use. The Skills bundle no tools, and following a project link does not install or connect a service.

## Final update

- Consolidated business-driven UI guidance and resource/component reuse, Controller/Gear states, Relations, Transitions, lists and branch decisions.
- Added outcome-based acceptance examples for template cards, reward states and responsive layout with entrance motion.
- Clarified presentation ownership, runtime refresh behavior and the limits of preview/persistence evidence.
- Preserved existing runtime references and historical installation paths; added maintenance notices to both Skill entrypoints.

The [v1.1.0 release](https://github.com/OpenFairyGUI/FairyGUI-Skills/releases/tag/v1.1.0) is the final snapshot. No further fixes, engine-version updates or support are planned. Validation covers Skill metadata, packaged reference links and archive contents; it is not a new multi-engine runtime certification or an Agent behavior benchmark.

## Historical installation (frozen version)

For the retained guidance, check out tag `v1.1.0` or extract that release before installing. This is a multi-skill repository: install the skill directories, not the repository root.

- `skills/fairygui-editor`
- `skills/fairygui-runtime`

Each skill includes `agents/openai.yaml` for Codex skill UI metadata.

## Historical Claude Code plugin

The frozen repository includes a Claude Code plugin manifest. The commands below are retained for compatible clients; future client compatibility is not maintained.

- Plugin manifest: `.claude-plugin/plugin.json`
- Marketplace catalog: `.claude-plugin/marketplace.json`
- Plugin skills location: `skills/`
- Local development/test command: `claude --plugin-dir .`
- Skill names in Claude plugin mode are namespaced by plugin name:
  - `/fairygui-skills:fairygui-editor`
  - `/fairygui-skills:fairygui-runtime`

### Claude marketplace flow

This repository root is also a single-plugin Claude marketplace.

- Add marketplace: `/plugin marketplace add .`
- Install plugin: `/plugin install fairygui-skills@fairygui-marketplace`
- After install, plugin skills are available as:
  - `/fairygui-skills:fairygui-editor`
  - `/fairygui-skills:fairygui-runtime`

The marketplace entry intentionally omits a `version` field because the plugin already defines `version` in `.claude-plugin/plugin.json`.

## Skills

- `skills/fairygui-editor/`: FairyGUI editor project engineering standards, business-driven component/state decisions, package/component authoring, publishing, generated code settings, plugin automation, and troubleshooting.
- `skills/fairygui-runtime/`: FairyGUI runtime coding standards for Unity, Unity Puerts, Cocos Creator, LayaAir, ThreeJS-style integrations, package loading, bindings, events, lists, transitions, lifecycle, and performance.

## Structure

```text
.claude-plugin/
  marketplace.json
  plugin.json
skills/
  fairygui-editor/
    agents/
      openai.yaml
    SKILL.md
    references/
      advanced-editor-features.md
      business-driven-ui.md
      component-selection.md
      editor-workflows.md
      plugin-automation.md
      publish-build.md
      state-and-layout.md
      troubleshooting.md
  fairygui-runtime/
    agents/
      openai.yaml
    SKILL.md
    references/
      component-binding.md
      data-binding-and-refresh.md
      engine-patterns.md
      events-animation.md
      loading-packages.md
      lists-scrolling.md
      performance-lifecycle.md
      runtime-architecture.md
      runtime-setup.md
      ui-lifecycle-and-ownership.md
      unity-patterns.md
      cocoscreator-patterns.md
      layaair-patterns.md
```
