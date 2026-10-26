---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot cloud agent 处理了三项任务，目标都是改善用户体验和增加功能。Cloud agent 虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以通过多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，可以查看所有拥有未关闭拉取请求 (PR) 的智能体任务。你可以了解智能体执行的操作，也可以在会话进行中提供指导。

在本课中，你将：

- 探索智能体页面，监控 cloud agent 任务。
- 在会话进行中提供指导，请求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到现在也适合添加浅色模式。既然已经在更新网站样式并添加切换功能，那么同时加入这项功能也很合理。你需要引导智能体，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应该能看到已分配给 Copilot 的任务，包括：
    - 更新代码库文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择其中一项正在运行的任务，检查 Copilot 已执行的操作。这些操作可能包括：
    - 检出存储库代码。
    - 为 Copilot 创建环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 的工作进度和处理方式而异。

4. 同时查看右侧的拉取请求 (PR) 窗格。这里可以查看 PR 和已更改的文件，以便进一步监控。

## 引导 cloud agent

现在你已经了解当前正在进行的任务，接下来请求 Copilot 在实现高对比度模式的同时添加浅色模式切换。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。出现类似以下消息时，说明已进入此阶段。
3. 在 **Steer active session while Copilot is working** 对话框中添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot 智能体页面中的云端智能体任务，突出显示“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！完成当前步骤后，它会将新请求纳入流程。

和之前一样，这需要几分钟。此时可以暂停一下，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心位置。通过这个控制中心，你可以跟踪所有正在进行和已完成的任务、检查智能体正在执行的工作，也可以在会话进行中调整范围或提供更多指导。

你探索了以下内容：

- 使用 mission control 和智能体页面监控 cloud agent 任务。
- 在会话进行中提供指导，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来可以[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
