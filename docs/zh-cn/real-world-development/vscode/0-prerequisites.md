---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在开始 VS Code 练习之前，你需要先把一切准备好。你将创建 Tailspin Toys 仓库的自己的副本，启动一个可工作的 [codespace][codespaces]，并确认 GitHub Copilot Chat 已在编辑器中正常运行。

## 设置实验仓库

要为你将创建的代码创建一份副本，你需要从 [模板][template-repository] 创建一个实例。新实例将包含实验所需的所有文件，并在整个练习过程中持续使用它。

1. 在一个新的浏览器窗口中，导航到 [Tailspin Toys 模板][tailspin-template]。
2. 通过在实验仓库页面中选择 **Use this template** 按钮创建自己的仓库副本，然后选择 **Create a new repository**。

   ![选择 Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你是在 GitHub 或 Microsoft 主持的活动中完成本工作坊，请遵循导师提供的说明。否则，你可以在拥有 GitHub Copilot 访问权限的组织中创建新仓库。

   ![输入仓库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下你创建的仓库路径（**organization-or-user-name/repository-name**），因为之后的实验中你会反复参考它。

> [!NOTE]
> **你的待办清单已就绪**
>
> 当你从模板创建仓库时，GitHub issues 的待办清单会自动为你创建。你将在整个工作坊中基于这些 issues 工作——不需要自己创建任何 issue。

## 创建 codespace

接下来，你将使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是一个基于云的开发环境，允许你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 导航到你新创建的仓库。
2. 选择绿色的 **Code** 按钮。

   ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建一个新的 Codespace。

   ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 可能需要几分钟时间，尽管它比手动安装所有服务快得多。与此同时，你可以利用这段时间探索 GitHub Copilot 的其他功能——下一步我们会把注意力转到这些功能上。

> [!CAUTION]
> 在后续练习中，你还会回到这个 codespace。暂时先把它留在浏览器标签页中，不要关闭。

> [!NOTE]
> 本工作坊设计为在 codespace 或本地 [dev container][dev-containers] 中运行。两者都确保环境安装了顺利运行所需的全部前置条件。如果你更想在本地运行，打开克隆的仓库后，在 VS Code 中选择 **Reopen in Container**——VS Code 会构建与 codespace 使用的开发容器相同的环境。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和 agent mode

要访问 GitHub Copilot Chat agent mode，你需要在 IDE 中安装 GitHub Copilot Chat 扩展；如果你正在使用 GitHub Codespace，这通常已经安装好了。

> [!TIP]
> 如果你还没有安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。或者打开 VS Code 的 Extensions 视图，搜索 **GitHub Copilot Chat**，然后选择 **Install**。

安装扩展后，你可能需要用 GitHub 账号进行认证以启用它。

1. 返回你的 codespace。
2. 如果你在编辑器右侧还没看到 Copilot Chat，选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似 “Hello world” 的消息，并按回车键。这将激活 Copilot Chat。
4. 如果你尚未认证，系统会提示你登录 GitHub 账号。按照说明完成认证。

   ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 完成认证后，你应该可以看到 Copilot Chat 窗口。

## 总结

恭喜，你已经创建了实验仓库的副本！同时也开始了 codespace 的创建流程，你将在后续开始编写代码时使用它。

## 下一步

让我们开始让 Copilot 发挥作用。继续阅读 [练习 1 - 自定义说明][next-lesson]，在那里你将教 Copilot 你的项目约定。

## 资源

- [GitHub Codespaces 概览][codespaces]
- [从模板创建仓库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
