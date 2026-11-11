---
title: "第 2 课 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 对游戏卡片进行一项小改动，检查浏览器中的效果，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

现在你已经安装并试用了 Copilot CLI，是时候对项目做第一次修改了。保守起见，这次改动会很小：游戏数据里已经有星级评分，但主页上的游戏卡片还没有显示它们。你将要求智能体展示评分、审查改动，并将其作为第一个拉取请求合并。

在本课中，你将：

- 在功能分支上开始一次聚焦的 Copilot 对话。
- 要求智能体对项目做一项小改动。
- 使用 `/diff` 审查变更。
- 运行应用并在转发浏览器中确认修改。
- 打开并合并你的第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，而且它们已经出现在游戏详情页中。但主页上的游戏卡片目前只显示标题、类别、发行商和说明。作为热身，你将让智能体在每张卡片上显示已有评分——这是一项小而独立的修改，非常适合作为第一个会话任务。

## 对话结构

**对话** 是你与 Copilot CLI 一起处理任务的地方。与 Copilot app 不同，一次正常的 CLI 对话会直接使用终端当前已检出的仓库和 Git 分支，而不是创建专用工作树。保存的对话允许你稍后回到同一讨论中，而文件和分支仍然是磁盘上的普通 Git 状态。

在会话中，你会看到三件事：你的提示词和智能体的回复、智能体探索和修改文件时的工具活动，以及可以用 `/diff` 检查的改动内容。

## 启动会话并请求修改

让我们开始一个新会话来实现该功能。

1. 返回你的 Codespace。
2. 如果终端之前没有打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开。
3. 如果还没打开，使用以下命令启动 Copilot：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 启动新会话，并按 <kbd>Enter</kbd>。
5. 使用以下提示词请求修改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，定位展示游戏详情的文件，并创建必要代码。现在，你已经使用 Copilot CLI 增加了一个新功能。

## 审查差异

所有 AI 生成的改动在合并前都必须经过审查，即使它们很小。现在就在 Copilot CLI 中查看这些变更。

1. 输入 `/diff` 并检查每个已修改文件。
2. 确认当评分存在时，游戏卡片会显示数值评分；当 `starRating` 为 `null` 时，显示 `No rating yet`。
3. 确认测试覆盖了这两种状态。
4. 审查 Copilot 运行的检查结果，并要求它修复任何失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 因为 Copilot 和所有生成式 AI 工具一样，具有概率性而非确定性，实际代码可能与示例不同。请关注行为，而不是期待完全一致的实现。

## 检查修改

当然，我们不能只看代码就假定它可用。现在让 Copilot 启动网站，以便在 Codespaces 转发的浏览器中检查更新后的用户界面。

1. 向 Copilot 提出以下请求，让它启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 提示端口 `4321` 已可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示其评分（满分 5 分）。
4. 返回 Copilot，并要求它停止它启动的开发服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 打开并合并你的第一个拉取请求

你现在已经创建了功能！现在是时候创建拉取请求（PR）来将新代码合并到项目里。

1. 让默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 在创建 PR 的同时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），选择 Copilot CLI 显示的 URL 打开 PR。
4. 审查变更文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回你的 Codespace，并用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结与后续步骤

恭喜！你已经使用 GitHub Copilot CLI 成功交付了第一个变更。具体来说，你：

- 在功能分支上启动了聚焦的 Copilot 对话。
- 指示智能体对游戏卡片做了一项小改动。
- 使用 `/diff` 审查了变更。
- 运行应用并在转发浏览器中确认了星级评分。
- 打开并合并了你的第一个拉取请求。

下一步，你将[从筛选议题开始，并使用 Plan 和 Autopilot 模式][next-lesson]来构建更大的功能。

## 资源

- [GitHub Copilot CLI 简介][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
