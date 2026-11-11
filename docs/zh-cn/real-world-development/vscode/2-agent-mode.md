---
title: "练习 2 - 使用 Copilot Agent Mode 添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件，并执行类似运行测试的操作。作为开发者，你的流程通常包括：定位所有必要文件、进行修改、运行测试、调试、发现漏改的文件、再做一次更新……事情会一直持续下去。

这正是 Copilot Agent Mode 发挥作用的地方。

Copilot Agent Mode 被设计为在你的 IDE 中更自主地工作。它的行为类似于开发者，首先探索现有项目结构，执行必要更新，运行类似测试这样的任务，并自动修复发现的错误。让我们看看如何使用 Agent Mode 为站点引入新功能。

> [!NOTE]
> 虽然名称相似，但 agent mode 和 cloud agent 面向的是两种不同的体验。Agent mode 在你的 IDE 中执行任务，允许快速反馈循环和交互。Cloud agent 则设计为团队中的配对程序员，以异步方式工作，像团队成员一样通过 issue 和 pull request 与你互动。

在本练习中，你将学习：

- Copilot Agent Mode 如何探索项目、识别相关文件并进行协调修改。
- GitHub Copilot Agent Mode 如何在 UI 和数据层中实现新功能。
- 在合并到代码库前如何审查 Copilot Agent Mode 生成的变更和测试。

## 场景

随着游戏列表不断增长，你希望允许用户按类别和发布者进行筛选。你已经在上一练习中添加了 publishers helper，现在你将使用 Copilot Agent Mode 完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

在进行任何更改之前，让我们先探索 Tailspin Toys 网站，了解它当前的功能。

该网站是一个以开发者主题为核心的众筹平台，用于展示桌游。它允许用户浏览游戏列表并查看每个游戏的详细信息。网站是一个单一的 Astro 应用，页面在构建时会渲染为静态 HTML。页面通过 Drizzle ORM 直接在 frontmatter 中查询本地 SQLite 数据库——没有单独的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/` 中，任何交互都通过一个小而聚焦的 Astro `<script>` 使用标准 DOM API 添加。

### 启动网站

为了让运行网站更简单，已经提供了一个 `npm run dev` 任务，用于启动单个 Astro 开发服务器。你可以在 GitHub Codespace 中按以下步骤运行它：

1. 返回你的 codespace。你应该已回到上一练习结束时的 `main` 分支。
2. 在 Codespace 内打开一个新终端窗口，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
3. 为这次筛选工作创建并切换到一个新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，你应该会看到一条指示 URL 的横幅，类似如下：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 通过按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中，选择从 Codespace 终端中的 localhost URL 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是到你的 codespace 的私有隧道，现在它正托管你的 Web 服务器！

### 浏览网站

一旦网站运行起来，你就可以开始探索其功能。网站的主要功能包括：

- **首页**：展示游戏列表，包括标题、图片和描述。
- **游戏详情页**：当你选择某个游戏时，会跳转到详情页，显示更多信息，包括标题、描述、发行商和类别。

## 用 Copilot 浏览待办清单

> [!TIP]
> **打开 Copilot Chat**
>
> 在开始下面的练习前，返回你的 codespace，打开 Copilot Chat 面板，并选择 **New Chat** 启动一个全新会话。模式和模型选择因练习而异——每一步都会在相关位置说明。
>
> 网站的初始实现已经可用，但我们希望通过增加新功能来增强它。先从查看待办清单开始。当你从模板创建项目时，已经自动为你创建了一批 issue —— 让 GitHub Copilot 帮你展示这些项目。

1. 在 Chat 视图中从 agents 下拉菜单选择 **Agent**。**Agent** 会自动规划并实现跨文件的变更，运行终端命令，并调用工具。

   ![Chat 视图中的 agent picker 截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并不表示偏好某个特定模型。构建这个工作坊时，我们使用的是 Claude Sonnet 4.5，因此在说明中包含它。我们希望你收到的代码建议相对一致，从而获得良好体验。然而，由于 LLM 具有概率性，你可能会注意到建议内容与工作坊说明中列出的内容有所不同。这完全正常，也属于预期。

> [!NOTE]
> 由于 LLM 的概率性，Copilot 可能会使用不同的 MCP 命令，但仍应该能够完成任务。

3. 通过向 Copilot 发送以下提示来查看 backlog issues：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行命令，列出所有 issue。
5. 查看生成的 issue 列表。

注意 Copilot 甚至已经根据它认为对用户最有用的事项为你排序了优先级。

## 审查说明文件

在启动 agent 来生成代码之前，先审查一下你将用来为其提供工作上下文的说明文件是个好主意。你将利用 [user interface (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 文件，其中包含关于如何为网站增加功能的上下文。

1. 在你的 codespace 中，导航到 `.github/instructions/ui.instructions.md`。
2. 记下整体指导原则，包括：
   - 架构概览。
   - 组件设计、可测试性和无障碍性原则。
   - 指向各种文件类型说明文件的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件允许你同时引用其他说明文件和项目中的文件。路径相对于说明文件所在位置计算。这允许复用、将复杂说明拆分成更小、更易管理的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选，应用需要至少进行三项独立更新：

- 在数据层（`src/lib/`）中添加或完善剩余的筛选逻辑。
- 为筛选行为添加或更新测试。
- 更新游戏列表页，加入筛选 UI。

此外，测试需要在合并到代码库前运行并通过。Copilot Agent Mode 可以帮你完成这些任务！现在让我们添加功能。

1. 你可以继续在当前 Copilot 对话中操作，也可以通过选择 **New Chat** 开始一个新对话。
2. 选择 **Add Context**、**Instructions**，并将 **ui** 作为说明文件。

   ![选择 UI instructions 文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确保 Chat 视图中仍然从 agents 下拉菜单选择了 **Agent**。

   ![Chat 视图中的 agent picker 截图](../../../_images/shared-chat-mode-selector.png)

4. 确保模型仍然选择 **Claude Sonnet 4.5**。
5. 使用以下提示词，要求 Copilot 根据待办清单中相关 issue 的要求实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何开始探索项目，定位所需功能相关文件。你应该能看到它找到数据层 helper 和 UI，以及测试文件。然后它开始修改文件并运行测试。

   ![Copilot 浏览项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> 你会注意到 Copilot 会执行几项任务，例如探索项目、修改文件和运行测试。这个过程可能需要几分钟，具体取决于任务复杂度和代码库规模。在这一过程中，你可能会注意到代码编辑器中出现 **Keep** 和 **Undo** 按钮。当 Copilot 完成后，你会为所有更改获得一个 **Keep** 或 **Undo**，因此在工作进行中不需要手动选择它们。

7. 根据 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中的对话框截图，提示用户确认是否运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 整个过程中你可能会遇到暂停，甚至看到一些测试失败。这没关系！Copilot 会在代码生成和测试之间反复迭代，直到任务完成且没有检测到错误。

   ![Copilot Agent Mode 完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，看看是否有潜在问题。

> [!CAUTION]
> 请记住，审查 Copilot 或任何 AI 工具生成的代码始终是非常重要的。

10. 回到正在运行的网站浏览器中。测试新功能！
11. 确认一切正常并审查代码后，选择 Copilot Chat 窗口中的 **Keep**。

## 总结与后续步骤

恭喜！在本练习中，我们学习了如何使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加新功能。我们学到了：

- GitHub Copilot Agent Mode 如何在 UI 和数据层中实现新功能。
- Copilot Agent Mode 如何探索项目、识别相关文件并进行协调修改。
- 在合并到代码库前如何审查 Copilot Agent Mode 生成的变更和测试。

现在让我们[用 Playwright MCP server 测试你的功能][next-lesson]，并为它打开一个 pull request。

### 额外探索练习——实现分页

随着游戏数量增加，会需要启用分页。使用你在本练习中学到的技能，提示 Copilot 更新站点以实现分页。代码需要考虑的几个点包括：

- 遵循现有最佳实践，包括使用现有说明文件。
- 思考你希望分页如何实现：允许用户选择页面大小，还是写死固定值。
- 在创建提示时确保给 Copilot 提供足够的指导，以按你的期望创建实现。
- 你可能需要与 GitHub Copilot 反复迭代，要求修改并提供上下文。这是与 Copilot 合作时的正常流程！

## 资源

- [Copilot ask、edit 和 agent 模式：功能与适用时机][choose-mode]
- [VS Code 中的 agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
