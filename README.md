# FairyGUI Skills

[中文说明](README_zh.md)

This repository contains AI Agent skill definitions for FairyGUI editor and runtime development.

## Installation

This is a multi-skill repository. Install the skill directories, not the repository root.

- `skills/fairygui-editor`
- `skills/fairygui-runtime`

Each skill includes `agents/openai.yaml` for Codex skill UI metadata.

## Claude Code Plugin

This repository root is also a valid Claude Code plugin.

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

- `skills/fairygui-editor/`: FairyGUI editor project engineering standards, package/component authoring, publishing, generated code settings, plugin automation, and troubleshooting.
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
