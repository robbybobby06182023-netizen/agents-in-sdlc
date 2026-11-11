---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

[自定义智能体][custom-agents-concept] 在 GitHub Copilot 中允许你创建专门定制的 AI 助手，用于开发工作流中的特定任务或领域。通过在仓库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，你可以为 Copilot 提供聚焦的说明、最佳实践、编码模式和领域特定知识，从而更有效地指导它执行特定类型的工作。团队可以将专业知识固化为可复用的智能体——例如，一位执行 [WCAG][wcag] 合规性检查的无障碍智能体、一位遵循安全编码实践的安全智能体，或者一位维护一致测试模式的测试智能体。

自定义智能体由项目的 `.github/agents` 文件夹中的 Markdown 文件定义，或者全局定义在 `~/.copilot/agents` 中。每个文件都有 YAML frontmatter，至少包含 `name` 和 `description`，随后是一个 Markdown 提示词，用于定义智能体的行为、专业能力和说明。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能（agent skills）][agent-skills-concept]之间存在一些逻辑上的重叠。两者主要都由 Markdown 文件定义，并告诉 AI 如何执行操作。最清晰的区分方式是：**自定义智能体** 是执行工作的角色，而**技能**是工具。

自定义智能体拥有自己的上下文窗口，可以协调技能（甚至其他智能体）来完成工作。在这个练习中，无障碍自定义智能体会根据无障碍准则审查并更新站点；在此过程中，它可以调用 pull request 工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一“正确”的方式。和 AI 的其他方面一样，请测试并迭代，找到适合你的环境和场景的方案。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于确保其众筹平台对所有用户都可访问，无论其视觉能力或偏好如何。近期用户反馈指出，当前深色主题因为文本和背景颜色对比不足，部分用户难以阅读。为解决这个无障碍问题，设计团队已要求实现高对比度模式，让用户可以打开和关闭。

由于无障碍至关重要，你希望尽快实现它。你将使用自定义智能体来生成这项功能。

在这个练习中，你将：

- 审查现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体来实现高对比度模式。

## 审查无障碍自定义智能体

一个自定义智能体已经为你创建好了，用于处理无障碍问题。让我们查看它的内容，了解它将如何指导 Copilot。

返回你的 codespace，然后在终端中切到一个从 `main` 分叉出来的全新分支，用于无障碍工作（你会保留 Exercise 3 中的筛选 PR 分支独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 注意 YAML frontmatter 中的 `name` 和 `description` 字段。

> [!CAUTION]
> 自定义智能体的 frontmatter 中必须包含 `name` 和 `description`。

3. 从那里开始浏览后续部分，重点查看：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍设计的最佳实践。
   - HTML、CSS 和 JavaScript 的代码示例。
   - 常见陷阱和错误列表。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中展示 `.github/agents` 中定义的每个自定义智能体。选择自定义智能体后，聊天会话便会限定在该智能体的说明和工具范围内。

> [!TIP]
> **打开 Copilot Chat**
>
> 在开始下面的练习前，返回你的 codespace，打开 Copilot Chat 面板，并选择 **New Chat** 启动一个全新会话。模式和模型选择因练习而异——每一步都会在相关位置说明。

1. 如果还没有选择，就在 Chat 视图中从智能体下拉菜单选择 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的智能体下拉菜单（其中显示当前活动智能体——默认是 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 开始工作——它会探索代码库、提出修改建议，并将编辑应用到你的项目文件中。每次编辑都会在聊天中按文件路径和 diff 的形式出现，你可以直接审查。
6. 当编辑落地时，**Files changed** 指示器会更新，让你看到该智能体在当前会话中已修改的工作集。

> [!NOTE]
> 这个过程可能需要几分钟。Copilot 会对你的仓库做真实修改——它会编辑现有文件，如 Astro 组件、CSS，以及相关测试。

你将在下一练习中审查并引导这项正在进行的工作。

## 总结与后续步骤

本课探讨了 GitHub Copilot 中的[自定义智能体][custom-agents]，即为特定任务和领域定制的专用 AI 助手。通过自定义智能体，你可以将团队的专业知识和标准固化为可复用智能体，引导 Copilot 更有效地完成特定类型的工作。

你探讨了这些概念：

- 如何定义自定义智能体。
- 在 Copilot Chat agent mode 中使用自定义智能体。

下一步，你将[监控并引导智能体的工作][next-lesson]——在实时过程中审查变更，并在同一会话中为其增加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 中的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试你的功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
