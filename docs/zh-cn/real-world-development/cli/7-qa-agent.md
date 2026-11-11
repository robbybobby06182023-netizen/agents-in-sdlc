---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建一个 QA 自定义智能体，结合议题要求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已经使用 `quality-checks` 技能运行自动化检查，并用 Playwright MCP 观察浏览器中的筛选体验。现在把这些能力集中到一个定义明确的 QA 角色中，利用自定义智能体来完成这项工作。

在本课中，你将：

- 了解自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选议题检查它的结果。

## 场景

Tailspin Toys 希望在打开功能 PR 之前，对要求、代码质量、自动化检查、测试覆盖率和浏览器行为保持一致的审查。自定义智能体能够协调这一 QA 流程，并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是一种由 Markdown 配置文件定义的专用 Copilot 版本。该配置文件描述了智能体的用途、说明和可用工具。对于这个工作坊，你将定义一个 QA 角色，放在 `.github/agents/qa.agent.md` 中，并在 Copilot CLI 中选择它。

你已经使用过的各类定制项承担着不同的职责。仓库说明描述团队标准。`quality-checks` 技能封装了可重复执行的检查。Playwright MCP 提供浏览器工具。QA 配置文件则告诉 Copilot 如何利用这些能力来评估要求并输出结论。它不会替代这些能力，也不需要另一段新对话。

## 创建 QA 配置文件

在打开功能 PR 之前，你将要求 Copilot 创建一个可复用的 QA 配置文件。该配置文件会定义 QA 执行的检查和必须遵守的边界。

1. 返回你的 Codespace，并确认筛选对话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

在使用新智能体之前，先检查它的配置文件，确认 Copilot 已捕捉到预期的 QA 工作流和边界。这可以避免生成一个不完整或过于宽泛的智能体，在你只想做验证时却去修改功能代码。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需字段；`name` 是可选的，但包含它会让智能体具有更清晰的显示名称。
3. 阅读配置文件说明，确认 QA 会从要求出发，遵循仓库说明，运行 `quality-checks` 技能，并使用 Playwright MCP。
4. 确认 QA 会给出支持性证据，报告各项要求为 pass、fail 或 blocked；在修改实现代码前必须先询问；并且不会提交变更或打开拉取请求。
5. 如果生成的配置文件遗漏了这些职责或边界，先让默认智能体修订它，再继续下一步。

## 根据议题运行 QA

Copilot CLI 会在会话启动时加载项目智能体。创建配置文件后，恢复同一段筛选对话，然后选择 QA，让它可以使用已经存在上下文中的议题和规划决策。

1. 启用智能体，使用以下提示：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 因为你刚刚创建了这个智能体，它可能不会立即出现在可用智能体列表中。但它确实存在，上面的命令就能激活它。

2. 使用下面的提示词，要求 QA 智能体评审该功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体开始工作！
4. 等它完成后，阅读它提供的报告！

## 总结与后续步骤

你为工作流加入了一个可复用的专家角色，并审查了它的工作结果。在本课中，你：

- 了解了自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查了一个可复用的质量保证配置文件。
- 选择了 QA 智能体，并根据筛选议题检查它的结论。

现在，你已经准备好实现、技能更新、QA 配置、测试和验证报告，下一步将一起审查它们并准备功能 PR。接下来，你将[在功能 PR 中整合这些内容，并使用 Agent Merge][next-lesson]。

## 资源

- [Create custom agents for Copilot CLI][create-agents]
- [Custom agent configuration][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
