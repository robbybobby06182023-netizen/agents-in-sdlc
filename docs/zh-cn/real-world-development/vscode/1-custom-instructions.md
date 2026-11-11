---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Agent mode →][next-lesson]

上下文在使用生成式 AI 时至关重要。如果某项任务必须按特定方式完成——或者 Copilot 需要知道某些背景信息——你就希望这些上下文可访问。向 Copilot 共享具体上下文的方法有很多，其中关键的是 [instruction files][instruction-files]，它们用于提供关于代码生成的指导。

在这个练习中，你将：

- 了解项目级上下文、编码规范和文档标准如何通过仓库自定义说明和按路径限定范围的说明文件传递给 Copilot；
- 在当前说明生效的前提下，生成筛选的第一块数据切片（publishers helper）；
- 向 `.github/copilot-instructions.md` 添加一个新的全仓库标准；
- 重新运行相同的提示，并观察生成的代码如何采用新标准；
- 提交说明更新和筛选切片到 `main`，以便在下一练习中 Copilot 能使用更新后的指导。

> [!CAUTION]
> 生成的代码可能与你设定的一些标准不完全一致。Copilot 是非确定性的。本练习的重点是观察在更新说明后，行为变化呈现出的*趋势*，而非逐字符匹配输出结果。

## 说明文件

### 场景

和任何优秀的开发团队一样，Tailspin Toys 都遵循一套开发实践准则和要求。这些包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 文档应以 TSDoc 文档注释的形式添加到代码中。
- 每个文件头部都应添加一段注释，说明该文件的作用。

通过使用说明文件，你可以确保 Copilot 拥有正确的信息，以便完成任务，并符合这些实践要求。

### 自定义说明

自定义说明允许你向 Copilot 提供上下文和偏好，让它更好地理解你的编码风格和需求。这是一个非常强大的功能，可以帮助你引导 Copilot 生成更相关的建议和代码片段。你可以指定首选的编码约定、库，甚至是你希望代码中包含的注释类型。你可以为整个仓库创建说明，也可以为特定类型文件创建说明，以提供任务级上下文。

说明文件有两种类型：

- `.github/copilot-instructions.md`：这是一个对整个仓库**每次**请求都发送给 Copilot 的单一说明文件。这个文件应包含项目级信息——适用于大多数发给 Copilot 的聊天或 CLI 请求的上下文。这可能包括所用技术栈、正在构建内容的概览、最佳实践以及其他全局指导。
- `.github/instructions/*.instructions.md` 文件可以为特定任务或文件类型创建说明。你可以用它们为特定语言（如 TypeScript 或 Astro）提供指导，也可以用于创建 UI 组件或一组新的单元测试等任务。

> [!NOTE]
> 在 IDE 中工作时，说明文件仅用于 Copilot Chat 的代码生成——不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会同时使用仓库级说明和 `*.instructions.md` 文件（带有 `applyTo` front matter）。
>
> 最后，Copilot [支持使用其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳实践

关于创建说明文件的完整讨论超出本工作坊的范围。不过，示例项目中提供的示例展示了一种具有代表性的方式。总体上看：

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如当前正在构建什么、项目结构以及全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供具体说明。
- 使用自然语言。保持指导清晰。给出代码应当和不应当是什么样的示例。

没有唯一正确的方式来创建说明文件，就像没有唯一正确的方式来使用 AI 一样。你会通过不断试验找到最适合你项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的说明文件。你在这个项目中探索时，可能会注意到有些说明文件用于多种任务类型，包括 [UI 更新][ui-instructions] 和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助你生成说明文件。每个界面暴露方式略有不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）——你所在界面的课程会在相关位置指出。
>
> 想要模板或起点？探索 [awesome-copilot][awesome-copilot]，这是一个收集说明文件、自定义智能体和其他资源的仓库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索这个项目中的说明文件

花一点时间阅读这个仓库附带的说明文件——这里有一个核心的 `copilot-instructions.md`，以及一组针对各种任务的 `*.instructions.md` 文件。你可以在编辑器中或 GitHub 网页 UI 中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览这个文件，注意它对项目的简要描述，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下面，注意嵌套的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并看看。注意其中包含针对 Astro 文件、Drizzle 数据层、测试等的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段——它设置了一个 glob（相对于仓库根目录），用于决定这些说明适用于哪些文件。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都将匹配。
5. 注意这些针对本项目创建单元测试的特定说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中链接到了其他说明文件（如 `unit-tests.instructions.md`）以及项目中的现有文件。这使你可以将更大的说明集拆成较小、可复用的文件，并指出 Copilot 在生成代码时应遵循的示例。（这些路径相对于说明文件本身，而不是仓库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但它还没有要求在代码中写文档说明。在下一步中，你将添加 TSDoc 文档注释和文件注释头的规则。

## 为我们的更改创建分支

这里按照一些最佳实践来做：为更改创建一个分支。

1. 返回你在上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开一个新终端。
3. 创建并切换到一个新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 在更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 在开始下面的练习前，返回你的 codespace，打开 Copilot Chat 面板，并选择 **New Chat** 启动一个全新会话。模式和模型选择因练习而异——每一步都会在相关位置说明。要观察自定义说明的影响，请先在当前说明文件生效的情况下发送一个提示。之后，你将更新它并重新发送相同的提示，看看差异。

1. 关闭之前练习中打开的编辑器标签页，让 Copilot 只接收你想要的上下文。
2. 打开 `src/lib/publishers.ts`，这样 Copilot 知道辅助函数应该放在哪里。
3. 在 Chat 视图中从 agents 下拉菜单选择 **Agent**，这样 Copilot 就能应用文件修改。

   ![Chat 视图中的 agent picker 截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件及其测试。
6. 注意提议的 helper 是一个带类型的函数，它的第一个参数接收 `db` 客户端，并返回一个带类型的 publishers 数组——这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该文件适用于 `src/lib/*.ts`）。
7. 注意提议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> 由于 Copilot 是概率性的，可能即使没被明确要求，也会添加文档注释。如果出现这种情况也没关系——说明更新后，*行为一致性*的提升才是这里要观察的重点。

## 添加新的全库标准

如前所述，`.github/copilot-instructions.md` 旨在为 Copilot 提供项目级信息。让我们确保仓库编码标准被记录下来，以改善后续代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 定位 **Code formatting requirements** 部分，应该在大约第 27 行附近。注意它如何记录项目的编码标准——但它目前还没有规定代码中的文档要求，因此生成的 helper 才没有文档注释。
3. 在现有标准的正下方添加以下 markdown 行，指示 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 正如上一课所示，说明文件既可以在仓库级别创建（`.github/copilot-instructions.md`）用于全局指导，也可以作为 `*.instructions.md` 文件用于特定语言、文件类型或任务。项目级文件更适合放置像你刚刚添加的文档注释规则这样的全局标准。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，并选择 **New Chat** 清空缓冲区。
2. 点击回到 `src/lib/publishers.ts`，让 Copilot 专注于正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意提议的文件现在会以类似下面的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意提议的函数现在会包含一段 TSDoc 注释，类似：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循了一个新的项目标准，并将其应用到了真实代码中；下一练习会在此基础上继续构建。

## 提交说明并推送分支

说明文件和仓库中的任何资产一样，都使用相同的源代码管理方式来维护。因此，让我们提交并推送分支到我们的仓库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开一个新终端窗口。
2. 在终端中运行以下命令，确认说明更新和 helper 更改已经出现：

   ```bash
   git status
   ```

3. 在终端中，暂存说明更新和 Copilot 为 helper 基础部分所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示了额外支持更新（例如测试），也将这些文件一起添加。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中，将分支推送到仓库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，我们应该创建一个 pull request，并将其关联到待办清单中与更新说明文件相关的 issue。我们可以手动完成，但 Copilot 可以借助已经连接到项目的 GitHub 工具来为我们处理。让我们让 Copilot 找到这个 issue，从当前分支创建 PR，并说明该 PR 关闭该 issue。

> [!NOTE]
> 这是第一个让 Copilot 代表你操作 GitHub 的练习。项目模板已经将 GitHub 的工具接入到你的工作区，因此无需额外设置——第一次 Copilot 使用这些工具时，VS Code 可能会提示你登录 GitHub。按提示允许它即可。稍后在后面的练习中，你将了解这种连接是如何工作的（Model Context Protocol）。

1. 在你的 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保从 agent picker 中选择了 **Agent**。
3. 让 Copilot 查找与更新说明文件相关的 issue，并从当前分支创建一个 PR，说明该 PR 关闭该 issue，并同时包含我们已为后续筛选工作新增的 publishers helper foundation：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. 当系统要求你 **Allow** Copilot 代表你执行 GitHub 操作时，审查该命令并按需选择 **Allow**。
6. PR 创建后，要求 Copilot 合并 PR，并通过以下提示将本地分支切回 `main` 并拉取最新代码：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当系统再次要求你 **Allow** Copilot 执行 GitHub 操作和 shell 命令时，审查命令并按需选择 **Allow**。

你现在已经在 GitHub Copilot 的帮助下创建并合并了一个 pull request！

## 总结与后续步骤

你已经了解了 Copilot 如何从该项目中的说明文件获取上下文，然后在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，并观察在*现有*说明下 Copilot 会生成什么；
- 向 `.github/copilot-instructions.md` 添加新的全仓库标准；
- 重新运行相同提示，并观察提议的代码如何采用新标准；
- 提交说明更新和 helper foundation 到 `main`，以便下一练习可以在此基础上继续构建。

下一步，你将在 [agent mode][next-lesson] 中让这些说明真正发挥作用，因为 Copilot 会为整个代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
