---
title: "第 10 课 - 总结与下一步"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和进一步资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在一条持续的 Tailspin Toys 工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace，安装了 Copilot CLI，浏览了项目，并定位到了种子筛选问题。
- 添加了星级评分，随后在转发浏览器中查看结果，并手动合并了第一个拉取请求（PR）。
- 从筛选问题出发，在 Plan 模式中定义方案，在 Autopilot 模式中构建它，并在 Interactive 模式中复查它。
- 使用自定义说明指导代理，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol (MCP) 服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择了质量保证（QA）自定义智能体，用于评估需求、覆盖率、技能结果和浏览器证据。
- 复审完整的筛选变更，并为筛选 PR 授权了 Agent Merge。
- 探索了用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 你交付了什么

这个工作坊有两个 PR 里程碑，每个里程碑都来自更新后的 `main` 分支上的单独分支：

1. **星级评分：** 在游戏卡片上展示现有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流：** 实现筛选、更新说明并将其应用到功能中，自定义 `quality-checks` 报告，创建 QA 配置，并包含相关测试。

从规划筛选到打开其 PR，你使用的是同一段对话和分支。我们将这部分工作合并到一个 PR 中，以便简化这个工作坊。

## 不同类型的验证

你通过几种方式检查代码：自动化测试、你自己的浏览器检查，以及通过 MCP 让 Copilot 在浏览器中进行探索。`quality-checks` 技能运行单元测试、lint 和类型检查，并以新的格式报告结果。QA 将这些结果与需求评审和测试覆盖率一起汇总，然后再决定是否创建 PR。

新增的测试应该填补真正的空白；一个不需要新增测试的 QA 检查也可能是正确的。在授权合并前，请审查代码和证据，并在变更后刷新受影响的证据。

## 最佳实践

你提供给 Copilot 的上下文和工具会塑造它的工作成果。在本工作坊中，你更新了说明，自定义了技能，创建了 QA 配置，并配置了 MCP 服务器。跨会话复用这些定制，并在团队需求变化时调整它们。说明定义标准，技能描述可重复任务，自定义智能体定义专业角色，MCP 服务器连接外部工具。审查实际变更和工具结果，而不仅仅看代理摘要。

将 **模式和模型** 匹配到任务。使用 **Plan** 在构建前梳理方案，使用 **Interactive** 对聚焦变更保持掌控，并使用 **Autopilot** 处理作用域明确的任务。对常规编辑选择更快的模型，对复杂工作选择更强大的模型。

上下文仍然和基础设施一样重要。清楚地描述你想构建的内容、原因以及如何实现，会显著改变输出结果。

## 更多可探索内容

你已经覆盖了核心工作流。还有几个更值得一看的 CLI 功能：

- `/review`：让代码审查代理分析变更。
- `/rubber-duck`：和一个问题讨论并获取另一种视角。
- `/fleet`：协调独立子任务并行执行。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot cloud agent。

## 下一步

提高任何工具熟练度的最佳方式，就是持续使用它！把它用于生产代码、爱好代码，以及你多年想写但从未开始的那个小应用。与团队分享你的经验，也从他们那里学习。并且一如既往，不断探索文档。

如果你想在终端中扩展 Tailspin Toys，请继续可选的 [Foundry Backer Concierge 系列][foundry]。要比较其他环境，请探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app] 或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
