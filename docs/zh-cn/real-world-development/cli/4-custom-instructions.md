---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "探索仓库说明、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果某项任务需要以特定方式完成，你需要让 Copilot 能访问相应指导。[说明文件][instruction-files] 不仅描述你希望代码*做什么*，还说明代码应该*如何组织*。现在你已经构建了筛选功能，接下来将探索 Copilot 使用的说明、添加一项文档标准，并将其应用于代码。

在本课中，你将：

- 探索仓库说明和按路径限定范围的说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 了解说明文件对代码的影响。

## 场景

和任何优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值，并在适用时包含可注入的 `db` 参数。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码变更时，注释也应保持最新。
- 应保留现有格式设置和 lint 指南。

通过使用说明文件，可以确保 Copilot 获得正确的信息，并按照这些实践要求完成任务。

## 说明文件

自定义说明允许你向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和需求。这项功能可以帮助你引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码约定、库，以及希望包含的注释类型。你可以为整个仓库创建说明，也可以为特定文件类型创建说明，提供任务级上下文。

说明文件有两种：

- `.github/copilot-instructions.md` 是一个说明文件，会针对该仓库的**每个**请求发送给 Copilot。它应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md` 文件会针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境对其他说明格式的支持各不相同。在依赖特定格式之前，请参阅[自定义说明支持参考][custom-instructions-support]。

## 探索本项目中的自定义说明文件

入门项目已附带一组说明文件。让我们先看看现有内容，再进行修改，以了解它们的影响。

1. 返回你的 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，注意其中对项目的简要说明和编码指导。这些说明适用于你在此仓库中与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段——它设置一个 glob，用来确定这些说明适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他说明文件和现有项目文件。这让你能够把较大的说明集拆分成较小、可复用的文件，并为 Copilot 提供可参考的示例。

## 更新说明文件以符合团队指导

现有文件已经是不错的起点，但仍有一处不足。我们将修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 在最后一个项目符号后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在会话开始时加载仓库说明。保存说明更新后，退出并恢复筛选会话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选会话仍处于打开状态，请输入 `/exit` 关闭它。Copilot CLI 会自动保存会话。
2. 在 `game-filters-cli` 分支的仓库根目录中，恢复已保存的会话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选会话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看修改过的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结与后续步骤

你探索了 Copilot CLI 如何获取说明文件中的上下文，并应用了一项新的标准。在本课中，你：

- 探索了仓库说明和按路径限定范围的说明文件如何传递给智能体。
- 更新了说明文件，确保遵循编码标准。
- 了解了说明文件对代码的影响。

接下来，你将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
