---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

[自定义智能体][custom-agents-concept] 是 GitHub Copilot 中的专用 AI 助手，可用于开发工作流中的特定任务或领域。通过在仓库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，你可以为 Copilot 提供聚焦的说明、最佳实践、编码模式和领域知识，从而更有效地指导它处理特定类型的工作。团队可以将专业知识固化为可复用的智能体——例如，执行 [WCAG][wcag] 合规性检查的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter（至少要有 `name` 和 `description`），后面则是一个 Markdown 提示，用于定义智能体的行为、专业能力和说明。

### 自定义智能体与 agent skills 的比较

自定义智能体和 [agent skills][agent-skills-concept] 之间存在一定的逻辑重叠。两者主要都由 Markdown 文件定义，并告诉 AI 应如何执行操作。最简单的区分方式是：**自定义智能体** 是工作者，**技能** 是工具。

自定义智能体拥有自己的上下文窗口，设计上可以协调技能，甚至协调其他智能体来完成工作。在本练习中，无障碍自定义智能体会审查并更新站点，使其符合无障碍准则；在执行过程中，它可能会调用 pull request 工作流技能，或调用用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一“正确”的方式。和 AI 的其他方面一样，请进行测试并反复迭代，找到适合你的环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

你将通过自定义智能体探索以下内容：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于确保其众筹平台对所有用户都可访问，无论其视觉能力或偏好如何。近期用户反馈指出，当前深色主题因文本与背景颜色对比不足，部分用户难以阅读。为解决这个无障碍问题，设计团队已要求实现高对比度模式，让用户可以打开和关闭。

由于无障碍至关重要，你希望尽快实施这一功能。你将使用自定义智能体来生成这项功能。

## 审查无障碍自定义智能体

一个自定义智能体已经为你创建好了，用于无障碍功能。让我们查看其内容，了解它将如何指导 Copilot。

返回你的 codespace，然后审查无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意 YAML frontmatter 中的 `name` 和 `description` 字段。

> [!CAUTION]
> 自定义智能体的 frontmatter 中必须包含 `name` 和 `description`。

3. 从那里开始浏览后续部分，重点查看：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍设计的最佳实践。
   - HTML、CSS 和 JavaScript 的代码示例。
   - 常见陷阱和错误列表。

## 创建并分配 issue

Mission control 是管理环境中所有智能体的中心位置。你可以将任务分配给 Copilot cloud agent、监控任务，甚至在会话进行中为它提供额外指导。现在先将创建高对比度模式的任务分配给 Copilot。

1. 导航到你的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 创建新 issue。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧，选择 **Assign to Copilot** 打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

   ![Copilot cloud agent 分配界面，其中高亮显示自定义智能体和 Accessibility agent](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理这个任务！

## 总结与后续步骤

本课探讨了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是为特定任务和领域定制的 AI 助手。借助自定义智能体，你可以将团队的专业知识和标准固化为可复用的智能体，引导 Copilot 更有效地执行特定类型的工作。

你探讨了这些概念：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

随着 Copilot 正在实现高对比度模式，我们现在可以把注意力转到[从 mission control 监控和引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [在组织中准备使用自定义智能体][org-custom-agents]
- [在企业中准备使用自定义智能体][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
