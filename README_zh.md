# FairyGUI Skills

[English README](README.md)

> **最终版本：v1.1.0 — 于 2026-09-08 停止维护。**
> 本仓库已归档，不再主动维护。已有编辑器与运行时 Skills 保留作为参考资料。

## 后续使用入口

| 目标 | 持续维护的项目 |
|---|---|
| FairyGUI 工程读写、SDK／CLI／MCP 自动化、事务、发布与恢复 | [OpenFairyGUI](https://github.com/OpenFairyGUI/OpenFairyGUI) |
| Agent 辅助 UI 制作与编辑、组件复用、资源分析、Viewer 预览和发布产物验证 | [FairyGUI-Maker](https://github.com/OpenFairyGUI/FairyGUI-Maker) 及其[当前 Skill](https://github.com/OpenFairyGUI/FairyGUI-Maker/tree/main/.agents/skills/use-fairygui-maker) |

这两个项目没有完整替代本仓库中的 Unity、Cocos Creator、LayaAir 等运行时编码指导。相关资料在此保留，不再持续更新；使用时请按实际引擎和 SDK 版本核验 API 与示例。Skills 本身不包含工具，访问项目链接也不会安装或连接服务。

## 最后一版更新

- 整理业务驱动的 UI 指导，以及资源／组件复用、Controller／Gear 状态、Relations、Transition、列表和分支的选择规则。
- 补充模板卡片、奖励状态、响应式布局与入场动效三个按结果验收的案例。
- 明确视觉属性归属、运行时刷新规则，以及预览和持久化证据的边界。
- 保留现有运行时参考和历史安装方式，并在两个 Skill 入口加入停止维护说明。

[v1.1.0 Release](https://github.com/OpenFairyGUI/FairyGUI-Skills/releases/tag/v1.1.0) 是最终快照，后续不再提供修复、引擎版本更新或支持。验证覆盖 Skill 元数据、分发后的参考链接与归档内容，不代表完成了新一轮多引擎运行时认证或 Agent 行为评测。

## 历史安装方式（冻结版本）

如需使用保留的指导，请先检出 `v1.1.0` 标签或解压该版本，再安装 Skill。这是一个多 skill 仓库，安装时应指向具体的 skill 目录，而不是仓库根目录。

- `skills/fairygui-editor`
- `skills/fairygui-runtime`

两个 skill 都包含 `agents/openai.yaml`，用于 Codex 的技能 UI 元数据。

## 历史 Claude Code plugin

冻结版本包含 Claude Code plugin 清单。下列命令为兼容的客户端保留，不再维护未来客户端的兼容性。

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

- `skills/fairygui-editor/`：FairyGUI 编辑器工程规范，覆盖面向业务场景的组件/状态设计、包和组件设计、发布设置、生成代码、插件自动化和排错。
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
