---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在整个实验中，你一直在和 GitHub Copilot 一起完成几项任务，目标是改善用户体验并增加功能。你要求 Copilot 为代码补充文档，为设计团队构建相关游戏功能，并实现包括高对比度和浅色模式切换在内的无障碍功能。让我们一起查看这些代码变更，并在必要时向 Copilot 提供反馈，以改进其工作。

### 场景

正如之前反复强调的那样，软件设计和 DevOps 的基础原理并不会因引入生成式 AI 而改变。我们始终希望审查 AI 生成的代码，并遵循正常的 DevOps 流程。基于这一点，让我们在为团队的其余工作启用 review 之前，先审查 GitHub Copilot 为创建文档、相关游戏功能和无障碍功能所给出的建议。

## 安全性和 GitHub Copilot cloud agent

因为 Copilot cloud agent 会异步且在无监督状态下执行任务，所以已实施了一些安全约束，以确保一切保持安全。这些约束包括：

- Copilot 仅对你的仓库具有只读权限，对它将用于编码的分支才有写权限。
- Cloud agent 在 GitHub Actions 内运行，在其中创建一个独立的临时环境来处理工作。
- 任何 GitHub Actions workflow 在运行前都需要人类批准。
- [对外部资源的访问默认受限](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

让我们先查看 GitHub Copilot cloud agent 生成的第一个 pull request（PR）——为代码添加文档。你将使用 GitHub.com 标准的 PR 界面来完成这个任务。

> [!NOTE]
> 当你查看 PR 时，可能会看到关于 GitHub Copilot 被防火墙拦截的警告。这**是正常现象**，因为 Copilot 默认对外部资源的访问是有限的，包括对外部 MCP 服务器的调用。如果你愿意，可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests** 打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 或更具体标题的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 中会带有 **[WIP]** 标记。如果是这样，请等待 Copilot 完成工作。这可能需要几分钟，所以你也可以休息一下，或者反思迄今为止学到的一切。

4. 一旦 PR 准备就绪，选择 **Files changed** 选项卡并查看变更。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 浏览新更新的代码，其中包括新创建的 TSDoc 文档注释和其他文档。具体变更会因情况而异。

   扫描变更时，请关注 TSDoc 文档注释、TypeScript 约定和注释头。这些来自你在 Exercise 2 开始时审查过的自定义说明文件。

6. 评审完更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该能看到某些 workflow 正在等待批准。
8. 如果 workflow 正在等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)
9. 你应该能看到 workflow 被加入 PR 的 checks 部分。若一切正常，你应能看到单个 Astro 应用的项目检查已通过。这可能需要几分钟。

## 向 GitHub Copilot 请求修改

在 PR 上与 Copilot 协作并不只是单向的。你也可以像对其他团队成员那样，在 PR 评论中或代码内联评论中 @copilot。Copilot 会看到这些评论，并启动新的 session 来处理它们。由于结果具有非确定性，我们不能给出一段规定性的文本来要求你写什么。你可以考虑的请求想法包括：

- 在每个代码文件顶部添加注释头，简要说明它们的作用。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建一个 README，描述 Astro 应用结构。

1. 通过添加一条评论请求对生成文档进行修改，像对其他用户一样使用 **@copilot**。可以选择上面任一想法，或者提出你希望在代码库中看到的其他文档相关建议。
2. 选择 **View Session** 观察 Copilot 执行其工作。注意 Copilot 会启动一个新 session 来完成这些更新。
3. 你可以选择 **Back to pull request** 返回 PR。

   ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. 一旦 Copilot 完成修改，你应该会在 PR 中看到新的提交。
5. 选择 **Files changed** 选项卡审查这些变更。

随时继续迭代，直到你满意为止。满意后，你可以将 PR 从草稿转换为 ready，并将其合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

让我们返回到 Copilot 为解决“在游戏详情页中显示相关游戏”问题生成的 PR。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或更具体标题的 PR。
4. 选择 **Files changed** 选项卡查看它生成的代码。
5. 审查完更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该能看到某些 workflow 正在等待批准。
7. 如果 workflow 正在等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)
8. 你应该能看到 workflow 被加入 PR 的检查部分。若一切正常，你应能看到单个 Astro 应用的项目检查已通过。这可能需要几分钟。
9. **可选：** 你甚至可以在你的 Codespace 中切到这个分支，手动测试相关游戏功能。导航到你的 Codespace，打开终端，并运行以下命令（将 `<branch-name>` 替换成 Copilot 创建的分支名，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已经构建好了相关游戏功能！和之前一样，你可以与 Copilot cloud agent 迭代式协作，要求它进行更新。例如，你可能想要求 Copilot 调整显示的相关游戏数量，或者确保添加注释头和 TSDoc 文档注释（记得——这是在你更新自定义说明**之前**分配的！）。和之前一样，你可以在 **Conversation** 选项卡中新增评论来发出这些请求，Copilot 会看到并启动新的 session。

## 审查无障碍功能

最后，让我们审查使用自定义无障碍智能体实现的无障碍功能。这个 PR 应该同时包含你在 Exercise 3 中分配的高对比度模式，以及在 Exercise 4 的 mission control 中要求添加的浅色模式。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或更具体标题的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 中会带有 **[WIP]** 标记。如果是这样，请等待 Copilot 完成工作。这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查它生成的代码。
5. 审查实现时，特别关注：
   - 用于切换模式的 UI 组件。
   - 使用 local storage 持久化用户偏好。
   - 高对比度和浅色模式的 CSS 或样式改动。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 处理模式切换的 JavaScript/TypeScript 代码。
6. 审查完更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该能看到某些 workflow 正在等待批准。
8. 如果 workflow 正在等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)
9. 你应该能看到 workflow 被加入 PR 的检查部分。若一切正常，你应能看到单个 Astro 应用的项目检查已通过。这可能需要几分钟。
10. **可选：** 你可以切到这个分支在你的 Codespace 中手动测试无障碍功能。导航到你的 Codespace，打开终端，并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   然后启动应用，并在浏览器中测试高对比度和浅色模式切换，确保它们按预期工作，并在页面重载后保持生效。

注意自定义无障碍智能体如何帮助指导 Copilot 按照无障碍最佳实践实现这些功能。如果你看到任何无障碍问题或改进点，可以像之前那样，在评论中标记 **@copilot** 来请求更新。

## 可选练习——继续委派

当你能为 cloud agent 提供真正的待办事项，并把注意力转移到其他事情上时，cloud agent 的效果最佳。为了养成这个习惯，可以在仓库中再提交几个 issue，并将它们分配给 Copilot。建议想法包括：

- 在游戏详情页创建一份支持者兴趣登记表单。
- 在游戏列表页实现分页。
- 为数据访问 helper 添加输入校验和错误处理。

## 总结

你已经完成了 Cloud agent 工作坊。通过这些课程，你：

- **审查了这个仓库随附的自定义说明文件**，以便在后续 cloud agent 输出中观察其影响。
- **将 issue 分配给 Copilot cloud agent**，并观察它设置环境、规划并异步执行。
- **创建并使用了针对无障碍的自定义智能体**，添加高对比度和浅色模式切换。
- **使用 Copilot agents page 作为 mission control** 来监控和引导无障碍 session。
- **审查并迭代 cloud agent 的 pull requests**，通过标记 `@copilot` 请求修改，并批准 workflow。

## 审查与下一步

你已完成 Cloud agent 工作坊。如果你想继续探索，其他工作坊与这里所练习的内容互补：

- 🖥️ **[VS Code 工作坊](../../vscode/)** — 直接从 IDE 中探索 Copilot Chat agent mode 和 MCP 集成。
- 💻 **[CLI 工作坊](../../cli/)** — 在终端中交付两项已审查的改动，包含 Plan 和 Autopilot 模式、agent skills、自定义 QA 智能体、Playwright MCP、Agent Merge，以及一个可选的 `/delegate` 桥接回 cloud agent。

在你自己的仓库中，尝试这些后续思路：

- 将重构或测试覆盖率 issue 分配给 cloud agent。
- 为另一个领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot agents][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
