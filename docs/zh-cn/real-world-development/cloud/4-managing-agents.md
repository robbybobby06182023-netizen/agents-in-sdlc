---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前面几节练习中，你要求 Copilot cloud agent 承担三项独立任务，目标是改善用户体验并增加功能。虽然 cloud agent 被设计为异步且自主运行，但监控这些任务仍然非常重要。

你可以使用多种工具来管理分配给 cloud agent 的任务，包括 GitHub.com 上的 [agents page][agents-page]。在这个 mission control 中，你可以看到所有具有开放 pull request（PR）的 agent 任务。你可以查看其执行的操作，甚至在会话进行中引导它，帮助其调整方向。

在本课中，你将：

- 浏览 agents page 以监控 cloud agent 任务。
- 引导正在进行中的 session，要求增加额外功能。

## 场景

在为网站添加高对比度模式之后，团队意识到这是个好时机，也顺带加入“浅色模式”。因为站点样式更新和切换功能的工作已经在进行中，所以把这部分功能也一起加入似乎很自然。你希望引导 agent 的工作，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

让我们看看当前分配给 Copilot cloud agent 的所有任务状态。

1. 导航到 [Copilot agents page](https://github.com/copilot/agents)。
2. 注意主面板和左侧面板中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
   - 更新代码库文档。
   - 生成修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务。查看 Copilot 执行过的任务，其中可能包括：
   - 从仓库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 为完成分配任务执行各种步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 工作状态和采用的方案而有所不同。

4. 另外注意右侧出现的 pull request（PR）面板。它允许你查看 PR 以及已变更的文件，以便进一步监控。

## 引导 cloud agent

现在你已经看到当前活跃任务，让我们在处理高对比度模式时要求 Copilot 同时包含浅色模式切换功能。

1. 选择与“添加高对比度模式”相关的 session。实际标题会因 Copilot 使用的名称和当前工作状态而变化。

   ![mission control 中的无障碍功能会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示已经完成设置并开始工作。通常当你开始看到下图类似消息时，就说明这一阶段已经开始了。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot agents 页面上的 cloud agent 任务截图，其中高亮显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示，并将其纳入后续流程中。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！它会在你发送消息时当前正在执行的特定步骤完成后，将新请求纳入其流程。

和之前一样，这可能还需要几分钟，所以这是一个暂停并反思你所学到和探索过的一切的好时机。

## 总结与后续步骤

本课探讨了 Copilot agents page，这是监控和引导 GitHub Copilot cloud agent 任务的中心控制台。通过这个 mission control，你可以跟踪所有活跃和已完成的任务，查看正在执行的工作，甚至在任务进行中重定向它以调整范围或提供额外指导。

你探讨了这些概念：

- 浏览 mission control 和 agents page 以监控 cloud agent 任务。
- 重定向正在进行中的 session，要求增加额外功能。

随着 Copilot 完成无障碍功能的工作，我们现在可以把注意力转到[迭代 Copilot 创建的 pull requests][next-lesson]。

## 资源

- [Copilot agents page][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
