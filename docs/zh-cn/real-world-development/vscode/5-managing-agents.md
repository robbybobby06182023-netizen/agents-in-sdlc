---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

当你把 GitHub Copilot 放到 agent mode 时，它会自主工作——探索代码库、提出修改、编辑文件并运行命令。因为这些工作会在你的机器上实时发生，所以 VS Code 中的 Copilot Chat 能让你看到每个工具调用和每次文件编辑的实时状态。你可以直接在聊天中查看每个 diff，接受或拒绝单个变更，并通过后续提示来引导会话，调整或扩展工作，而无需离开编辑器。

在这个练习中，你将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中引导智能体同时增加浅色模式切换。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。与此同时，设计团队又提出了一个附加需求——他们也希望增加一个浅色模式切换，以便用户在默认主题过暗时切换。你不会从头重新开启会话，而是利用现有 session 来扩展这项工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体采取的每一步——它读取的文件、运行的命令以及提出的编辑内容。每个文件编辑都会以 diff 的形式直接出现在聊天中，并带有 **Keep** / **Undo** 控件。

1. 返回练习 4 的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天窗口，观察每个工具调用的进展。你会看到文件读取、文件编辑以及它要求运行的终端命令。
3. 智能体提出文件编辑时，diff 会直接出现在聊天中：
   - 选择 **Keep** 接受该修改。
   - 选择 **Undo** 撤销该修改，并让智能体再试一次。
4. 你也可以点击聊天顶部的 **Files changed** 指示器，查看该 session 期间已修改的完整工作集。

> [!NOTE]
> 如果某个工具调用需要权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认为可以接受的命令。

5. 等待智能体完成高对比度模式的实现。你应该会看到一条最终消息，概括它所做的更改。

## 引导智能体添加浅色模式

现在高对比度模式已经实现，你将扩展同一会话来添加浅色模式切换。由于继续使用同一个聊天 session，智能体会保留之前的上下文——包括它修改过的组件、采用的样式方案，以及设置 local storage 时使用的方案。

1. 在 Copilot Chat 输入框中，发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会接着之前的工作继续处理，探索已有修改，并为浅色模式提出新增内容。
3. 像处理高对比度一样，实时审查每个新编辑。对每个 diff 使用 **Keep** 或 **Undo**。
4. 如果智能体的方向不符合预期——例如，它试图以你不喜欢的方式重构切换 UI——可以选择聊天输入框旁的 **Stop** 按钮停止它，然后发送一条澄清性的后续消息。

> [!TIP]
> 引导进行中的 session 比从头开始更快。使用后续提示来要求调整（例如“简化 CSS——改用 CSS 变量，而不要重复选择器”），来修正方向（例如“不要增加第三个切换按钮，把浅色模式整合到现有设置面板中”），或者扩展范围（例如“也为两种切换都添加键盘快捷键”）。

## 审查工作集

提交前，快速检查智能体修改过的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查改动文件的完整列表。你应该看到 Astro 组件、样式以及相关测试等更新。
3. 打开几个已修改的文件，逐段查看 diff。确认自定义智能体遵循的无障碍模式已经反映出来——ARIA 属性、键盘导航、语义 HTML，以及通过 local storage 持久化的设置。
4. 当你满意时，在 Source Control 面板中暂存并提交改动。你将在[下一课][next-lesson]中发布它们。

## 总结与后续步骤

你已使用 Copilot Chat agent mode 监控并引导进行中的会话——逐一审查文件编辑，接受或拒绝改动，并通过后续提示扩展工作，同时保留上下文。

在本课中，你探讨了：

- 实时监控智能体的工作。
- 接受和拒绝单个文件编辑。
- 在会话进行中通过后续提示引导它扩展工作。

在[下一课][next-lesson]中，你将把无障碍工作发布为 pull request，并回顾整个实验中本地自定义智能体所产生的变更。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用 agent mode][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
