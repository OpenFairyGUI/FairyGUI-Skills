# FairyGUI Skills

[English README](README.md)

这个仓库包含面向 FairyGUI 编辑器工程和运行时开发的 AI Agent skill 定义。

## 安装说明

这是一个多 skill 仓库。安装时应指向具体的 skill 目录，而不是仓库根目录。

- `skills/fairygui-editor`
- `skills/fairygui-runtime`

两个 skill 都包含 `agents/openai.yaml`，用于 Codex 的技能 UI 元数据。

## Claude Code Plugin

仓库根目录现在同时也是一个可用的 Claude Code plugin。

- Plugin 清单：`.claude-plugin/plugin.json`
- Marketplace 目录：`.claude-plugin/marketplace.json`
- Plugin skills 位置：`skills/`
- 本地开发/测试命令：`claude --plugin-dir .`
- 在 Claude plugin 模式下，skill 名称会带插件命名空间：
  - `/fairygui-skills:fairygui-editor`
  - `/fairygui-skills:fairygui-runtime`

### Claude marketplace 安装方式

仓库根目录现在同时也是一个“单插件 marketplace”。

- 添加 marketplace：`/plugin marketplace add .`
- 安装 plugin：`/plugin install fairygui-skills@fairygui-marketplace`
- 安装后可用的 skill 名称：
  - `/fairygui-skills:fairygui-editor`
  - `/fairygui-skills:fairygui-runtime`

`marketplace.json` 没有重复声明 `version`，因为插件版本已经在 `.claude-plugin/plugin.json` 中定义。

## Skills

- `skills/fairygui-editor/`：FairyGUI 编辑器工程规范，覆盖包和组件设计、发布设置、生成代码、插件自动化和排错。
- `skills/fairygui-runtime/`：FairyGUI 运行时编码规范，覆盖 Unity、Unity Puerts、Cocos Creator、LayaAir、ThreeJS 风格集成、包加载、绑定、事件、列表、动效、生命周期和性能。

## 目录结构

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
