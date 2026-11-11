---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

很少有组织完全没有技术债务。可能是未解决的安全问题、需要更新的遗留代码，或者长期搁置在待办中的功能需求，因为没有时间去实现。GitHub Copilot 的 cloud agent 旨在执行诸如更新代码和添加功能等任务，且以自主方式完成。agent 完成工作后，会生成一个供人类开发者审查的草稿 PR。这样可以分担枯燥任务、加快开发流程，并让开发者将注意力集中在更大局的事项上。

你将通过 Copilot cloud agent 探索以下内容：

- 为生成代码自定义环境。
- 确保操作安全执行。
- 明确定义作用域的 issue 的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇创建首版站点的承包商留下了非常不理想的文档——你会注意到它几乎完全缺失。作为第一步，他们希望看到为应用中的所有导出函数补充 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望每个游戏的详情页显示相关游戏——同一类别中的其他标题——这样用户可以继续浏览。他们不需要一个精美的实现，现在只想要一个可用于 UX 验收测试的版本。这目前是一个阻塞项，但还有其他优先级更高的问题。

这两个示例都属于很容易被降优先级的任务，非常适合分配给 Copilot cloud agent。Copilot cloud agent 可以异步处理它们，让开发者专注于其他任务，然后再回来审查 Copilot 的工作，确认一切都符合预期。

## 介绍 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以在后台执行任务，和人类开发者的工作方式很相似。与人类开发者一样，这可以通过多种方式完成，包括 [将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。一旦分配，Copilot 会创建一个草稿拉取请求来跟踪其进度，设置环境，并开始处理任务。你可以在它执行过程中或完成后深入查看 Copilot 的会话。一旦它准备好让你审查方案，它就会在 PR 中标记你！

## 明确定义作用域说明的重要性

GitHub Copilot 中并没有魔法——你不能跳过思考。即使看似简单的操作，一旦剥开层层复杂性，也会发现其中有很多需要考虑的地方，所以[请谨慎为 Copilot cloud agent 设定任务范围][cloud-agent-best-practices]。把它当作你的 AI 配对程序员：分阶段工作，学习、试验，并随着推进持续适应。软件开发的基础原理并不会因为引入生成式 AI 而改变。

## 这个仓库中的自定义说明

前面的练习介绍了自定义说明以及它们如何指导 Copilot。如果你已经完成了“自定义说明”动手练习，你已经看到了 `.github/copilot-instructions.md` 的实际作用；如果还没看过，现在是快速回顾的好时机。在将工作分配给 Copilot cloud agent 之前，先只读看看这个仓库中已经包含的说明文件，以便在后面的练习中观察它们产生的效果。

在 GitHub 网页 UI 中打开以下文件，或者如果你已经有运行中的 codespace，就在其中打开：

- `.github/copilot-instructions.md` - 查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释的期望。
- `.github/instructions/unit-tests.instructions.md` - 注意其中的 `applyTo` frontmatter，它将这些说明限定在 `**/*.test.ts` 文件范围内。

当你在下一节中将 `Code lacks documentation` issue 分配给 cloud agent 时，观察生成的 pull request 中是否出现 TSDoc 文档注释、TypeScript 约定和注释头——这些都来自这些说明文件。之后在后面的练习中我们还会再次指出这一点。

## 为 Copilot cloud agent 设置开发环境

无论是谁参与代码创建，通常都需要特定的环境和一些设置脚本来确保一切处于良好状态。将任务分配给 Copilot 时也是如此，因为它在执行任务时的方式类似于 SWE。

Cloud agent 在工作的前期使用 [GitHub Actions][github-actions] 创建环境。你可以通过在 `.github/workflows/copilot-setup-steps.yml` 中创建一个[专用的设置工作流][setup-workflow]，让它在真正开始工作前先运行。这使它能够访问所需的开发工具和依赖项。这个实验已经预先配置好了，以帮助工作流推进并支持本次学习。它确保 Copilot 可以访问 Node.js、项目依赖、端到端测试所需的浏览器二进制文件，以及单个 Astro 应用的迁移和种子 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从仓库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设置为这些步骤所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果你需要在设置步骤中克隆仓库（例如安装依赖），则需要 contents: read 权限。如果设置步骤不克隆仓库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

它看起来和任何其他 GitHub workflow 文件一样，但有几个关键点：

- 它包含一个名为 `copilot-setup-steps` 的单一 job。这个 job 会在 Copilot 开始处理 pull request 之前，在 GitHub Actions 中执行。
- 注意 `workflow_dispatch` 触发器，它允许你从仓库的 **Actions** 选项卡手动运行该 workflow。这对于测试 workflow 能否成功运行非常有用，而不是等待 Copilot 来执行它。

## 添加文档

虽然每个人都理解文档的重要性，但大多数项目要么文档过时，要么完全缺失。这类技术债务往往不会被及时处理，导致生产力下降，并使代码维护和新成员入门变得更困难。幸运的是，Copilot 非常擅长生成文档，这正是一个非常适合分配给 Copilot cloud agent 的 issue。它会在后台工作，以生成所需文档。后面的练习中你会回来审查它的工作成果。

1. 在新浏览器标签页中，导航到你的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 创建新 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧，选择 **Assign to Copilot** 打开分配对话框。

   ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的 pull request（PR），其标题大致类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，等待片刻后刷新浏览器窗口。
12. 过几分钟后，你应该会看到 Copilot 已创建待办清单。

> [!NOTE]
> Copilot 的待办清单可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面提到的 workflow）、分析项目，并决定解决问题的最佳方案。

13. 查看清单及其将要完成的任务。
14. 滚动 PR 时间线，你应该能看到 Copilot 已开始处理该 issue 的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 你可能需要刷新窗口才能看到更新后的指示器。

16. 注意你可以滚动浏览实时会话，看到 Copilot 是如何解决问题的。这包括检查代码并理解状态、暂停思考并决定合适方案，以及创建代码。

这可能需要几分钟。Copilot cloud agent 的一个主要目标就是让它能够异步执行任务，从而释放我们去处理其他事情。我们将利用这一点：先将另一个任务分配给 Copilot cloud agent，然后转而投入编写代码，以为应用添加新功能。

## 为游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一个巨大优势是能够分工协作，你可以专注于一组任务，而它专注于另一组任务。虽然为设计团队添加“相关游戏”部分可能不一定会耗时很长，但它仍然是一段可能被用于其他任务的时间。让我们将它分配给 Copilot cloud agent！

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧，选择 **Assign to Copilot** 打开分配对话框。

   ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久之后，你应该会在 issue 第一条评论中看到一组 👀，表示 Copilot 正在处理这个任务！

![Copilot 使用 👀 表情表示它正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理你的第二个请求！Copilot cloud agent 的工作方式类似于 SWE，因此你不需要主动持续监控它，而是在它完成后再来审查。现在让我们把注意力转向创建和使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。使用 cloud agent 时，你可以将 issue 分配给 Copilot，让它异步处理。你可以用它来处理技术债务、创建新功能，或帮助把代码从一个框架迁移到另一个框架。

你探讨了这些概念：

- 为生成代码自定义环境。
- 确保操作安全执行。
- 明确定义作用域的 issue 的重要性。
- 将 issue 分配给 Copilot。

随着 cloud agent 在后台勤奋工作，我们现在可以把注意力转向创建和使用自定义智能体。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以使用它可用的自定义说明，这些我们已在前面的模块中探讨过。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
