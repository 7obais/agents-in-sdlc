---
title: "练习 0：准备工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot cloud agent 练习前，需要做好准备。你将创建 Tailspin Toys 存储库的副本，并启动一个 [codespace][codespaces]，用于编辑说明文件和检查云端智能体生成的成果。

## 设置实验存储库

要为练习中的代码创建存储库副本，请从[模板][template-repository]创建一个实例。新实例会包含实验所需的所有文件，你将在整个练习中使用它。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面选择 **Use this template** 按钮创建自己的存储库副本，然后选择 **Create a new repository**。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你参加的是由 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下创建的存储库路径（**organization-or-user-name/repository-name**），后续实验中会用到。

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

## 总结

恭喜，你已创建实验存储库副本！你还开始创建 codespace，后续练习中将使用它与 Copilot cloud agent 协作。

## 下一步

现在为云端智能体添加自定义说明。继续学习[练习 1 - 自定义说明][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
