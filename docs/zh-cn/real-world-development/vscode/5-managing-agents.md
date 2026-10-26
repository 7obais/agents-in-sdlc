---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

在 agent mode 中，GitHub Copilot 会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作实时发生在你的计算机上，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以内联检查每个差异、接受或拒绝单项更改，也可以通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

在本练习中，你将：

- 实时监控无障碍智能体的工作。
- 检查并接受建议的文件编辑。
- 在会话进行中引导智能体，在高对比度模式旁添加浅色模式切换。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队提出了新需求：还希望为觉得默认主题太暗的用户提供浅色模式切换。无需从头开始新对话，可以引导当前会话扩展工作范围。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中内联显示差异和 **Keep** / **Undo** 控件。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容查看每次工具调用。你会看到文件读取、文件编辑，以及请求运行的终端命令。
3. 智能体提出文件编辑时，差异会内联显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的所有文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你确认安全的命令。

5. 等待智能体完成高对比度模式。完成后，它会显示一条总结所做更改的消息。

## 引导智能体添加浅色模式

高对比度模式已就绪，现在可以在同一对话中扩展工作，添加浅色模式切换。继续使用同一聊天会话时，智能体会保留之前工作的上下文，包括修改过的组件、采用的样式方案和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续之前的工作，探索已有更改并提出添加浅色模式的方案。
3. 和高对比度模式一样，逐项检查新编辑。对每处差异使用 **Keep** 或 **Undo**。
4. 如果智能体偏离预期（例如以不希望的方式重构切换 UI），可以选择聊天输入框旁的 **Stop** 按钮停止它，然后发送澄清提示。

> [!TIP]
> 引导正在运行的会话比从头开始更快。可以使用后续提示提出调整要求（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个切换项，将浅色模式集成到现有设置面板中”），或扩展范围（“同时为两个切换项添加键盘快捷键”）。

## 检查工作集

提交之前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 检查所有已更改文件。你应该能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改的文件并查看差异。确认自定义智能体要求的无障碍模式已经应用，例如 ARIA 属性、键盘导航、语义化 HTML，以及使用本地存储保存设置。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课将发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat agent mode 监控并引导正在进行的会话：逐项检查文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

在本课中，你探索了：

- 实时监控智能体工作。
- 接受或拒绝单项文件编辑。
- 使用后续提示引导正在进行的会话。

在[下一课][next-lesson]中，你将发布无障碍功能更改并创建拉取请求，同时检查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用 agent mode][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
