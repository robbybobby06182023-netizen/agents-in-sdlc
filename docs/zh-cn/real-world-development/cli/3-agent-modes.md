---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认方案，使用 Autopilot 根据议题实现过滤，并使用 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们从给项目增加一个小功能开始。但更大的改动需要更稳健的流程。幸运的是，GitHub Copilot CLI 设计来适配组织现有的工作流，确保我们构建的是正确的内容、以正确的方式完成开发。这是几个课程中的第一节：你将按典型的智能体驱动开发流程工作，首先从议题生成新功能，确保代码有效、功能按预期工作，并最终成功合并到项目中。

> [!NOTE]
> 你会继续使用同一段对话和分支来完成整个功能开发流程。通常情况下，不同文件类型会有不同分支或 PR，但我们会走一条捷径，以便把重点放在核心概念上。

在本课开始时，你将：

- 从 GitHub 议题启动一个新的 Copilot 对话。
- 在 Plan 模式中定义要求。
- 在 Autopilot 模式中实现新功能。
- 审查代码。
- 在转发浏览器中手动验证功能。

随着功能推进，你还将更新仓库说明，自定义现有的 `quality-checks` 技能，接入 MCP 验证，创建 QA 智能体，并打开功能 PR。

## 场景

Tailspin Toys 的目录在增长，访问者需要按类别与发行商筛选游戏。待办事项描述了该功能，但组合类别等细节需要在编码前达成一致。你将先使用 Plan 模式决定这些细节，再授权 Copilot 进行受限实现，并随后在 Autopilot 中完成开发。

## 背景

将 AI 编码智能体引入开发流程不会改变基础原则。事实上，它们反而会变得更加重要！大多数开发者遵循这样的流程：

1. 从已提交的议题开始，明确要完成什么。
2. 制定实现计划。
3. 构建并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码审查完成且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体细节会因团队和组织不同而不同，但大多数流程都会在这个主题上做变体。

通过遵循这种标准流程，你可以确保 AI 生成的代码符合既定要求，并经过与手写代码相同的审查流程。

## 对话模式

**对话模式** 控制智能体拥有多少自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间循环切换：

- **Interactive**：你和智能体一起协作。智能体会建议修改，并在继续前等待你的输入。
- **Plan**：智能体首先制定计划，并被禁止编辑项目文件。
- **Autopilot**：智能体可自主工作——编写代码、运行测试，并不断迭代直到任务完成。

先进入 Plan 模式，审查计划，然后用 Autopilot 来实现它。

## 从议题开始

在开始筛选工作之前，返回你的 Codespace，确认仓库和终端已准备好。

1. 返回你的 Codespace。如果它已停止，先重新启动它。
2. 确认星级评分 PR 已合并。
3. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开。
4. 更新 `main`，再为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按 <kbd>Tab</kbd> 两次打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键高亮标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 把议题加入提示词，并返回 **Session** 选项卡。

注意提示词现在以 `#7`（或类似编号）开头。`#` 允许你将 GitHub 上的议题或拉取请求带入上下文。

## 规划筛选功能

规划让你有机会在交给 Copilot 实施前，先定义要采用的方法。对任何复杂工作而言，花一点时间规划通常都是明智的。让我们切换到 Plan 模式，并让 Copilot 创建方案。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示词下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后面，输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 开始构建计划！它会先探索项目，然后确定最佳方案。

3. 在此过程中，Copilot 可能会询问筛选功能应该如何工作。根据你的偏好回答这些问题。这里没有绝对错误答案！
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动以审查计划。
6. 要求 Copilot 修订任何与你的决定不一致的部分。

## 批准 Autopilot

计划已写好并审查完成，现在开始实现功能吧！让 Copilot 继续工作，使用 autopilot。

Autopilot 会允许 Copilot 持续迭代问题，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你安装版本中相似的选项。
2. 确认提示词下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何根据既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后实现可能会立即开始，所以要先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请在将检查视为完成前先解决环境问题。

## 审查和验证实现

一旦代码生成出来，就需要在合并前进行审查，就像任何其他代码一样。让我们一起审查代码并运行站点，确保一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff` 并检查筛选实现和测试。
3. 将结果与议题及计划中做出的决定进行比对。
4. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 审查项目检查的输出，并要求 Copilot 修复任何失败项。

## 探索新功能

代码看起来不错——但它运行起来怎么样？像之前一样启动应用，并通过 Codespaces 转发端口打开站点。

1. 向 Copilot 提出以下请求，让它启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及你在计划中达成一致的组合筛选。
4. 确认重置和空结果行为符合议题和你的决定。
5. 返回你的 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

你已经使用不同的对话模式来构建和审查功能。在本课中，你：

- 从 GitHub 议题启动了一个新的 Copilot 对话。
- 在 Plan 模式中定义了要求。
- 在 Autopilot 模式中实现了新功能。
- 审查了代码。
- 在转发浏览器中手动验证了功能。

下一步，让我们进一步研究代码是如何生成的，确保它遵循文档约定，并[使用自定义说明][next-lesson]来实现这一点。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
