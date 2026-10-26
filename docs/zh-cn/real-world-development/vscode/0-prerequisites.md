---
title: "练习 0：准备工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要做好准备。你将创建 Tailspin Toys 存储库的副本，启动一个 [codespace][codespaces] 供你使用，并确认编辑器中的 GitHub Copilot Chat 已正常运行。

## 设置实验存储库

要为练习中的代码创建存储库副本，请从[模板][template-repository]创建一个实例。新实例会包含实验所需的所有文件，你将在整个练习中使用它。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面选择 **Use this template** 按钮创建自己的存储库副本，然后选择 **Create a new repository**。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你参加的是由 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下创建的存储库路径（**organization-or-user-name/repository-name**），后续实验中会用到。

> [!NOTE]
> **积压议题已准备就绪**
>
> 从模板创建存储库时，系统会自动为你创建一组 GitHub 积压议题。整个工作坊中都会使用这些议题，无需自行创建。

## 创建 codespace

接下来，你将使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是一种基于云的开发环境，让你可以直接在浏览器中编写、运行和调试代码。它是功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 打开新创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，不过仍比手动安装所有服务快得多！等待期间，可以先探索 GitHub Copilot 的其他功能，接下来就会介绍它们。

> [!CAUTION]
> 后续练习中还会回到 codespace。现在先让它保持打开状态，并留在浏览器的一个选项卡中。

> [!NOTE]
> 本工作坊可以在 codespace 或本地 [dev container][dev-containers] 中运行。这两种环境都预装了所需内容，确保练习顺利进行。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 会构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和 agent mode

要在 IDE 中使用 GitHub Copilot Chat agent mode，需要安装 GitHub Copilot Chat 扩展。如果你使用 GitHub Codespace，通常已经预装了此扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace 安装][copilot-chat-extension]。也可以在 Visual Studio Code 中打开 Extensions 视图，搜索 **GitHub Copilot Chat** 并选择 **Install**。

安装扩展后，可能还需要使用 GitHub 帐户进行身份验证，才能启用它。

1. 返回 codespace。
2. 如果编辑器右侧未显示 Copilot Chat，请选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似 “Hello world” 的消息并按 Enter。这会激活 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示你登录 GitHub 帐户。请按提示完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 身份验证完成后，Copilot Chat 窗口应会显示出来。

## 总结

恭喜，你已创建实验存储库副本！你还开始创建 codespace，后续编写代码时将使用它。

## 下一步

现在开始使用 Copilot。继续学习[练习 1 - 自定义说明][next-lesson]，了解如何让 Copilot 遵循项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
