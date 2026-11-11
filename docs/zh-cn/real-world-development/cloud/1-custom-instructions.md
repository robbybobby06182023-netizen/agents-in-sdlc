---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果某项任务必须以特定方式完成——或者 Copilot 需要了解一些背景信息——你就需要确保这些上下文可用。[说明文件][instruction-files] 可以提供这种指导，让 Copilot 不仅了解*你希望它做什么*，还了解*你希望它如何完成*。

在本课中，你将：

- 了解项目级上下文、编码规范和文档标准如何通过仓库自定义说明和按路径限定范围的说明文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 运行环境不同，你不会在这里进行*前后对比*提示——Copilot cloud agent 会异步处理 GitHub issue，因此很难实时演示其影响。你将在本工作坊后续查看 cloud agent 创建的拉取请求时，看到说明文件的实际效果。

## 说明文件

### 场景

和任何优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 文档应以 TSDoc 文档注释的形式添加到代码中。
- 每个文件顶部都应添加一段注释，说明该文件的作用。

使用说明文件可以确保 Copilot 获得正确的信息，并按照这些实践要求完成任务。

### 自定义说明

自定义说明允许你向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和需求。这是一项非常强大的功能，可以帮助你引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望代码中包含的注释类型。你可以为整个仓库创建说明，也可以为特定文件类型创建说明，提供任务级上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：这是一个针对整个仓库**每次**请求都会发送给 Copilot 的单一说明文件。它应包含项目级信息，也就是适用于大多数发送给 Copilot 的聊天或 CLI 请求的上下文。这可以包括所用技术栈、正在构建的内容概览、最佳实践以及其他全局指导。
- `.github/instructions/*.instructions.md` 文件可以为特定任务或文件类型添加说明。你可以用它们为特定语言（例如 TypeScript 或 Astro）提供指导，也可以用于创建 UI 组件或新增一组单元测试等任务。

> [!NOTE]
> 在 IDE 中工作时，说明文件仅用于 Copilot Chat 的代码生成——不会用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用仓库级说明文件以及带有 `applyTo` front matter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持使用其他标准的说明文件][custom-instructions-support]，包括 `AGENTS.md` 和 `CLAUDE.md` 文件。

### 管理说明文件的最佳实践

关于如何创建说明文件的完整讨论超出了本工作坊范围。不过，示例项目中的内容展示了一种具有代表性的做法。从总体上看：

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如项目概况、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供具体说明。
- 使用自然语言。保持指导清晰。提供代码应该如何写以及不应该如何写的示例。

创建说明文件没有唯一正确的方法，就像使用 AI 也没有唯一正确的方法一样。通过不断实验，你会逐渐找到最适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该具备一套完善的说明文件。查看本项目中的文件时，你可能会注意到它们覆盖了许多任务类型，包括 [UI 更新][ui-instructions] 和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面对这一功能的暴露方式不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）——你所在环境的课程会在相关位置指出。
>
> 想找模板或起点？可以看看 [awesome-copilot][awesome-copilot]，这是一个汇集说明文件、自定义智能体和其他资源的仓库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的说明文件

花一点时间阅读此仓库附带的说明文件——其中包含一个核心 `copilot-instructions.md`，以及一组面向不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网页 UI 中查看它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览该文件，注意其中对项目的简要说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，还要注意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览。你会看到其中包含针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段——它设置一个相对于仓库根目录的 glob，用于决定这些说明适用于哪些文件。在这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 注意其中针对为本项目创建单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中链接到了其他说明文件（如 `unit-tests.instructions.md`）以及项目中的现有文件。这样可以把较大的说明集拆分为更小、可复用的文件，并为 Copilot 指出生成代码时可参考的示例。（这些路径是相对于说明文件本身，而不是仓库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但目前还没有要求在代码中编写文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的全库标准

接下来，你只需要做一项编辑：添加一条项目范围的规则，要求代码使用 TSDoc 文档注释和文件级注释头。后续练习中 cloud agent 处理你分配的 issue 时会读取这项规则。

在编辑前，先为这项工作创建一个分支（cloud agent 会从 issue 的目标分支读取说明，但你仍然可以先在自己的分支上提交这项更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 旨在向 Copilot 提供项目级信息。让我们确保仓库编码标准已被记录下来，以改善后续的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 定位 **Code formatting requirements** 部分，应该在第 27 行附近。注意它记录了项目的编码标准——但目前还没有规定代码中的文档说明，这也是生成的 helper 缺少文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，指导 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 正如你在上一课中看到的，说明文件既可以放在仓库级文件（`.github/copilot-instructions.md`）中提供全局指导，也可以放在 `*.instructions.md` 文件中，指导特定语言、文件类型或任务。像刚刚添加的文档注释规则这样的全局标准，适合放在仓库级文件中。

## 提交、推送并合并说明更新

cloud agent 会从 issue 的目标分支读取说明文件。因此，在下一课中将 issue 分配给 Copilot 时，它会从 `main` 分支创建新分支。你必须先将说明更改合并到 `main`，cloud agent 才能读取这些更改。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上从 `custom-instructions-cloud` 创建拉取请求，并将其合并到 `main`。这样 cloud agent 在后续练习中处理你分配的 issue 时，就能读取这些说明。

> [!TIP]
> 如果你更希望在本工作坊中直接在 `main` 上工作，也可以跳过分支步骤，直接提交到 `main`。这里加入分支步骤，是为了让工作坊更贴近现实项目中的常见做法。

## 总结与后续步骤

你已经了解 Copilot 如何从项目说明文件中获取上下文，并向 `.github/copilot-instructions.md` 添加了一项新的全库标准。你将在后续练习中看到这项标准如何影响 cloud agent 生成的拉取请求。

接下来，你将[把第一个 issue 分配给 Copilot cloud agent][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
