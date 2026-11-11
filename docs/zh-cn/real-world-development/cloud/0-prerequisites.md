---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在开始 Copilot cloud agent 练习之前，你需要先把一切准备好。你将创建 Tailspin Toys 仓库的自己的副本，并启动一个可以用来编辑说明文件、查看 cloud agent 产出工作结果的 [codespace][codespaces]。

## 设置实验仓库

要为你将创建的代码创建一个副本，你需要从 [模板][template-repository] 创建一个实例。新实例将包含实验所需的所有文件，你将在整个练习过程中使用它。

1. 在新的浏览器窗口中，导航到 [Tailspin Toys 模板][tailspin-template]。
2. 通过在实验仓库页面中选择 **Use this template** 按钮创建你自己的仓库副本。然后选择 **Create a new repository**。

   ![选择 Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你是在由 GitHub 或 Microsoft 组织的活动中完成这项工作坊，请按照导师提供的说明进行操作。否则，你可以在你有 GitHub Copilot 访问权限的组织中创建新仓库。

   ![输入仓库模板设置](../../../_images/ex0-repository-settings.png)

4. 记录你创建的仓库路径（**organization-or-user-name/repository-name**），后面的练习中你会反复参考它。

## 创建 codespace

接下来，你将使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是一个基于云的开发环境，允许你直接在浏览器中编写、运行和调试代码。它提供了一个功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 导航到你新创建的仓库。
2. 选择绿色的 **Code** 按钮。

   ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

   ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 可能需要几分钟时间，虽然它比手动安装所有服务快得多。与此同时，你可以利用这段时间探索 GitHub Copilot 的其他功能——下一步我们就会把注意力转到这些功能上。

> [!CAUTION]
> 在后续练习中，你还会返回到这个 codespace。暂时先把它留在浏览器标签页中，不要关闭。

> [!NOTE]
> 本工作坊设计为在 codespace 或本地 [dev container][dev-containers] 中运行。两者都能确保环境已安装顺利运行所需的全部前置条件。如果你更想在本地运行，打开克隆的仓库后，在 VS Code 中选择 **Reopen in Container**——VS Code 会构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

恭喜，你已经创建了实验仓库的副本！同时也开始了 codespace 的创建流程，你将利用它来与 Copilot cloud agent 协作完成后续操作。

## 下一步

让我们为 cloud agent 添加它需要遵循的自定义说明。继续阅读 [练习 1 - 自定义说明][next-lesson]。

## 资源

- [GitHub Codespaces 概览][codespaces]
- [从模板创建仓库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
