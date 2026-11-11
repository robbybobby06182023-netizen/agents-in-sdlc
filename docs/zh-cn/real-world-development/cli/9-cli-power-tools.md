---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话以及可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和任何优秀的 CLI 工具一样，GitHub Copilot CLI 也包含了许多斜杠命令来与它交互。这些命令暴露了高级功能、幕后信息以及额外配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在让我们探索几个其他有用的命令。

在本课中，你将：

- 使用 `/context` 和 `/compact` 来了解 Copilot 如何管理对话上下文。
- 使用 `/model` 来查看你可用的模型。
- 学习 `/share` 如何导出或共享会话。
- 探索并行工作、工作树以及云端代理委派的可选命令。

## 场景

你已经完成了核心 CLI 工作流。现在让我们看几个额外能力——管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot cloud agent][about-cloud-agent]。

## 探索 Copilot CLI 上下文

当处理更大、更复杂的任务时，你可能会遇到模型的最大上下文窗口限制。Copilot CLI 会在必要时自动压缩对话，你也可以使用斜杠命令手动检查或压缩上下文。

1. 返回你的 Codespace，并在仓库根目录启动 Copilot CLI（如果尚未打开）。
2. 输入：

   ```plaintext
   /context
   ```

3. 记录模型、当前 token 使用量，以及上下文如何在系统说明、工具、消息和空闲空间之间划分。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就不大，差异可能不会很明显。

> [!NOTE]
> 当上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。只有在你想选择压缩时机时才使用 `/compact`。当你切换到无关任务并希望开启新的对话时，请使用 `/clear` 或 `/new`。

## 选择你的模型

不同模型有不同的长处，不同开发者也有不同偏好。Copilot CLI 允许你列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和使用信息。
3. 保持当前模型、选择另一个模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作并分享经验有助于每个人改进对 AI 工具的使用方式。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件，创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，并查看你当前安装版本中的 `/share` 选项。
2. 如果你想共享这个会话，选择最适合你需求的目标，例如 `/share file` 以导出本地 Markdown 文件。
3. 在将导出内容发给任何人或发布前先检查它。会话导出可能包含提示词、响应和项目细节。

发布链接或 gist 是可选的。不要发布你团队不打算共享的仓库或对话内容。

## 可选：扩展规模或委派工作

核心工作坊已经完成。Copilot CLI 还提供了针对更大任务的命令：

- `/fleet` 可以将独立子任务分配给多个子代理并并行运行。
- `/worktree` 可以为单独任务创建独立的 Git 工作树。
- `/delegate` 可以将任务发送给 Copilot cloud agent，它会异步处理，并可能打开拉取请求。

这些命令是可选的，因为它们可能会创建额外的工作树或远程任务。在尝试它们之前，先启动一个新的、作用域明确的任务，并通过你正常的工作流检查结果。如果你想深入研究异步代理工作，请继续学习 [cloud agent workshop][cloud-workshop]。

## 总结与后续步骤

在 Copilot CLI 中使用斜杠命令可以让你配置它、共享会话，并了解幕后发生的事情。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 学习了 `/share` 如何导出或共享会话。
- 探索了并行工作、工作树和云端代理委派的可选命令。

还有更多斜杠命令可用，也有更多内容等你在 Copilot CLI 中继续探索！让我们通过 [回顾我们学到的内容][next-lesson] 和一些继续学习的下一步来结束这段旅程。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
