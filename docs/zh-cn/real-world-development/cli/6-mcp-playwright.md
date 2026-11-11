---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "检查或配置 Copilot CLI 中的 Playwright MCP，并用它在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如我们已经强调过的那样，编写代码不仅仅是写代码本身。我们还需要与数据、外部服务交互，甚至需要让 Copilot 能访问更多自动化能力。这正是 MCP 服务器发挥作用的地方。MCP 服务器让 Copilot 能超出 CLI 内置能力，提供更多工具和服务。

在本课中，你将：

- 了解什么是 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它。
- 如果 Playwright MCP 服务器尚未可用，就添加它。
- 让智能体使用浏览器工具来探索筛选功能。

## 场景

虽然单元测试和端到端测试很重要，但要验证 UI 的更新，最终还是需要真实地与 UI 交互。你希望允许 Copilot 像用户一样使用你正在开发的网站，以进一步自动化改动，增加对功能正常工作的信心。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体提供了一种与外部工具和服务通信的方式。通过使用 MCP，AI 智能体可以实时与外部工具和服务通信，从而访问最新信息并代表你执行操作。

这些工具和资源通过 MCP 服务器访问，该服务器充当 AI 智能体和外部工具/服务之间的桥接层。每个 MCP 服务器代表一组不同的可用工具和资源。

一些常见的现有 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供访问 GitHub 仓库、议题和拉取请求管理 API 的能力。
- **[Playwright MCP Server][playwright-mcp-server]**：提供基于 Playwright 的浏览器自动化能力。

还有许多其他 MCP 服务器可用。GitHub 托管有一个 [MCP registry][mcp-registry]，用于提升可发现性并促进生态贡献。

> [!CAUTION]
> 对待 MCP 服务器要像对待项目中的任何其他依赖一样谨慎。在使用之前，仔细检查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Playwright MCP 服务器尚未可用，就添加它，让 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Playwright MCP 已经配置在 Copilot CLI 中，可以跳过这些设置步骤，直接继续下一节。

1. 返回你的 Codespace。
2. 在 Copilot CLI 中输入以下命令打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 在名称处输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 将以下内容粘贴到 **Command** 对话框中：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 通过 Playwright 探索功能

此前你手动确认了功能按预期工作。现在让我们使用 Copilot 中可用的 Playwright MCP 服务器来做同样的事情！

1. 使用下面的提示词，告诉 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 你实际上不需要显式告诉 Copilot 使用 MCP 服务器，因为它通常会自行判断。但既然你知道应该使用什么工具，这样提示也不算坏。它有助于保证结果更稳定，并节省一些 token。

2. 观察 Copilot 如何列出在浏览器中执行的各个步骤，以确认功能是否正常。
3. 阅读报告，确认一切按预期运行。

Copilot 会启动服务器，使用 Playwright 与网站交互，停止服务器，并最终给出报告。

## 总结与后续步骤

恭喜，你已经使用 Playwright MCP 服务器从 Copilot CLI 的浏览器中探索了功能。回顾一下，你：

- 了解了什么是 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它。
- 添加了 Playwright MCP 服务器（如果尚未配置）。
- 让智能体通过浏览器工具来探索筛选功能。

下一步，你将[创建一个 QA 自定义智能体][next-lesson]，把技能和浏览器工具结合在一个专门角色中。

## 资源

- [What the heck is MCP and why is everyone talking about it?][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Add MCP servers to Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
