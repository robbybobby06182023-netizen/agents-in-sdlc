---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 让 GitHub Copilot 能在云端异步工作。你可以在 GitHub 上分配工作，cloud agent 会在后台接手——探索仓库、修改代码，并打开拉取请求——而你则可以继续做其他事情。

在这些练习中，你将添加 cloud agent 应遵循的自定义说明，然后分配一个 GitHub issue 让它实现工作。你还将审查并使用自定义智能体来塑造它的方案，从 agents dashboard 监控和引导会话，并在最后审查它的 pull request 并对结果进行迭代。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|------|
| [0. 先决条件][ex0] | 设置 | 创建你的仓库和 codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加 cloud agent 将遵循的自定义说明 |
| [2. Cloud Agent][ex2] | 异步代理 | 将 issue 分配给 Copilot cloud agent |
| [3. 自定义智能体][ex3] | 专用智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控和引导 agent 会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代 Copilot 的工作，并选择下一步 |

## 先决条件

在参加本工作坊前，请确认你已满足：

- [ ] 拥有 GitHub 账号，并启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 你的账号或组织已启用 Copilot cloud agent

> [!TIP]
> 没有付费计划？已验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费获取 GitHub Copilot。**Copilot Student** 计划包含本工作坊所用到的 agent、MCP、code review 和 Copilot CLI 功能，因此你可以用它完成每个 harness。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent 可用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**——除 Copilot Free 之外的所有计划。若你使用的是 Copilot Business 或 Enterprise，管理员必须为你的组织启用它。

> [!NOTE]
> 本 harness 不覆盖 MCP。要探索如何在 Copilot 中使用 MCP 服务器，请参见 [CLI harness](../cli/) 或 [VS Code harness](../vscode/)。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
