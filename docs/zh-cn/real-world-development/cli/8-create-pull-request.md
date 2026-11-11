---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、说明更新、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你的筛选实现、说明更新、技能更新、质量保证（QA）配置和测试都保存在同一分支上。现在是时候一起审查它们，并打开拉取请求。你自己已经合并过星级评分 PR；这次你将让 **Agent Merge** 负责后续流程。

> [!NOTE]
> 通常情况下，我们会把功能、说明更新、技能更新和 QA 智能体拆成几个单独的 PR。为了简化本工作坊，你已经把筛选和质量工作流程保留在同一段对话和分支中，并将所有这些更改放进这个 PR。

在本课中，你将：

- 了解 Agent Merge 是什么，以及它如何自动化合并生命周期。
- 检查完整的功能改动和验证证据。
- 创建筛选 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

整个筛选工作流程中，你已经用 Copilot 规划、实现和验证了一项功能。Tailspin Toys 现在希望在保持开发者仍然掌控合并授权的前提下，自动化剩余的 PR 工作。

## 介绍 Agent Merge

**Agent Merge** 会自动完成落地拉取请求所需的剩余工作。当你启用它时，Copilot 会处理阻塞 PR 的问题——修复失败的持续集成（CI）检查、响应审查意见、在需要时重新变基——然后在仓库允许自动合并时启用 GitHub 自动合并。

到目前为止，你一直都是自己选择 **Merge pull request**。而 Agent Merge 可以承担这一责任。在启用它之前，请先审查工作内容，并确认它已经准备好。

## 使用 Agent Merge 管理 PR

有了所有代码创建完成，让我们一起审查它，创建 PR，然后让 Agent Merge 处理后续流程。

1. 返回你的 Codespace。
2. 通过输入 `/agent` 打开智能体对话框。
3. 在可选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建一个新 PR。
5. 通过 `/pr agentmerge` 激活 agent merge。
6. Copilot 会持续监控 PR 上的持续集成进程。一旦所有检查成功，它就会执行合并。
7. 确认你看到 Copilot 发来的消息，例如类似 “PR #14 was squash-merged successfully.”

> [!IMPORTANT]
> Agent Merge 不会绕过所需审批、分支保护、合并队列、仓库设置或缺失权限。如果它被阻止，请阅读错误原因，并在仓库允许的情况下手动完成审查后的合并。

## 总结与后续步骤

你已经自动化了开发流程中的多个部分，包括生成代码、测试和验证代码，以及现在的拉取请求流程。你：

- 了解了 Agent Merge 是什么，以及它如何自动化合并生命周期。
- 检查了完整的功能改动和验证证据。
- 创建了筛选 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

下一步，你将[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，用于处理上下文、模型、共享和可选的云端委派。

## 资源

- [Manage pull requests with Copilot CLI][manage-prs]
- [Copilot CLI command reference][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
