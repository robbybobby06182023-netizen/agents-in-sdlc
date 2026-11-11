---
title: "练习 3 - 使用 Playwright MCP 服务器测试你的功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：Agent mode][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚用 agent mode 构建了筛选功能。在打开 pull request 之前，你应该先确认它确实能在浏览器中正常工作。你不必自己逐步点击应用，而是连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器来测试这个功能——然后发布你的分支并打开 PR。

在这个练习中，你将：

- 了解什么是 Model Context Protocol (MCP) 以及 MCP 服务器如何扩展 Copilot 的新工具。
- 在你的工作区中添加 Playwright MCP 服务器。
- 让 Copilot 使用它在浏览器中手动测试你的筛选功能。
- 发布分支并为筛选工作打开 pull request。

## 什么是 Model Context Protocol (MCP)？

当 agent mode 能够超越编辑器、访问外部能力时，它的威力会大幅增强。Model Context Protocol (MCP) 正是 Copilot 实现这一点的方式——它是一种让智能体与外部工具和服务通信的标准方式。

![示意图展示 agent mode 的内部工作方式，以及它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。使用 MCP，AI 智能体可以实时与外部工具和服务通信，从而获取最新信息（通过资源）并代表你执行操作（通过工具）。

这些工具和资源通过 MCP 服务器访问，它充当 AI 智能体与外部工具和服务之间的桥接层。MCP 服务器负责管理 AI 智能体与外部工具（例如现有 API 或本地工具，如 NPM 包）之间的通信。每个 MCP 服务器代表一组 AI 智能体可以访问的工具和资源。

以下是两个常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：该服务器提供一组用于管理 GitHub 仓库的 API 访问能力。它允许 AI 智能体执行创建新仓库、更新现有仓库以及管理 issue 和 pull request 等操作。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：该服务器使用 Playwright 提供浏览器自动化能力。它允许 AI 智能体执行导航网页、填写表单和点击按钮等操作。

还有许多其他 MCP 服务器可用于访问不同的工具和资源。GitHub 托管了一个 [MCP 注册表](https://github.com/mcp)，用于提高可发现性并促进生态系统贡献。

> [!CAUTION]
> 关于安全性，要像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，仔细审查其源代码、验证发布者，并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 审查 MCP 配置

`.vscode/mcp.json` 文件配置了这个 VS Code 工作区中可用的 MCP 服务器。

1. 在你的 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到一个已经配置好的 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

这个 `github` 条目由项目模板提供，所以 Copilot 才能在上一练习中读取 backlog issue。它使用的是[远程 GitHub MCP 服务器][remote-github-mcp-server]，因此不需要本地安装——VS Code 通过 HTTP 连接到它，并在 Copilot 首次使用其工具时要求你进行 GitHub 认证。

## 添加 Playwright MCP 服务器

现在你将添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server] 会为 Copilot 提供一个可控制的浏览器，正好用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加一个 `playwright` 条目，使文件看起来像这样：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会在你的机器上启动它，执行 `npx @playwright/mcp@latest`。`--headless` 参数告诉 Playwright 在不显示窗口的情况下运行浏览器，这在 codespace 中尤其必要，因为那里没有桌面环境来显示浏览器。

> [!NOTE]
> Tailspin Toys 项目本身也在使用 Playwright 进行端到端测试，因此 Playwright 需要的浏览器通常已经安装好。如果 Copilot 后来报告缺少浏览器，让它运行 `npx playwright install chromium`，然后再试一次。

## 启动并信任服务器

VS Code 会在需要时**按需启动** MCP 服务器——Copilot 首次需要某个服务器的工具时，VS Code 会启动它，并询问你是否信任该服务器。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它来启动服务器，并确认配置正确。
2. 如果 VS Code 询问你是否信任该服务器，请查看配置并选择信任，以便服务器启动。

手动启动是可选的——如果跳过此步骤，VS Code 会在下一步 Copilot 首次需要其工具时启动服务器，并提示你确认是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server] 为 Copilot 提供一个可操控的真实浏览器。你不必自己逐步检查应用，智能体可以打开页面、导航、应用筛选条件并读取结果，然后向你报告观察结果并总结。无需离开对话，这通常是确认功能是否符合预期的最快方式。

在实现层面，Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]，而不是截图。这意味着智能体会像辅助技术一样，根据带标签的结构化元素（按钮、链接、列表项）进行推理，因此快速的功能检查也能兼作基础的无障碍检查。

服务器连接好且应用正在运行时，让 Copilot 测试你刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步完成测试，并报告发现。将总结与 issue 中的验收标准对照——如果某处表现不符预期，继续追问，或让它先修复代码再打开 pull request。

> [!NOTE]
> 应用需要在 `http://localhost:4321` 上运行，才能执行此测试。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器——如果它报告缺少浏览器，让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建 pull request

既然已经确认功能可用，你就可以准备打开 pull request（PR）供团队审查了。第一步是发布 `filtering-vscode` 分支。

1. 在 codespace 中导航到 **Source Control** 面板，查看 Copilot 做出的更改。
2. 通过选择 **+** 图标来暂存这些修改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示所做更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到你的仓库。

## 创建 pull request

创建 pull request 有很多种方式，包括通过 github.com 和 GitHub 命令行界面（CLI）。但既然你已经在使用 GitHub Copilot，不妨让它为你创建 PR！它可以找到相关 issue，并创建一个关联到它的 PR。

1. 导航到 Copilot Chat 面板，并选择 **New Chat** 启动一个新会话。
2. 确保从智能体下拉菜单中选择了 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 让 Copilot 为你创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要，选择 **Continue** 允许 Copilot 执行收集信息和执行操作所需的任务。第一次使用 GitHub 工具时，你可能会被提示进行 GitHub 认证——按照提示允许它即可。
5. 注意 Copilot 会如何搜索 issue，找到正确的一个，并创建 PR。
6. 选择 Copilot 生成的链接来查看你的 pull request，但请**先不要合并**它。

## 总结与后续步骤

恭喜！在这个练习中，你：

- 了解了什么是 Model Context Protocol (MCP) 以及 MCP 服务器如何扩展 Copilot 的新工具。
- 在工作区中添加了 Playwright MCP 服务器。
- 在发布代码之前，使用它在浏览器中手动测试了你的筛选功能。
- 发布了分支，并为筛选工作打开了 pull request。

你使用 MCP 服务器来*测试*功能——但 MCP 只是扩展 Copilot 的一种方式。下一步，让我们[创建一个自定义智能体][next-lesson]，以简化无障碍审查等专项任务。

## 资源

- [What the heck is MCP and why is everyone talking about it?][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP 服务器][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
