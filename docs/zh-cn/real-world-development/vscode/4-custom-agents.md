---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]可以根据开发工作流程中的特定任务或领域，创建专门的 AI 助手。在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，就能为 Copilot 提供重点明确的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业经验编写成可复用的智能体，例如负责确保符合 [WCAG][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局存放在 `~/.copilot/agents`。每个文件都有 YAML frontmatter，至少包含 `name` 和 `description`，后面跟着定义智能体行为、专长和说明的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]有一定重叠。两者主要都通过 Markdown 文件定义，用来指导 AI 执行操作。最简单的区分方式是：**自定义智能体**是执行工作的角色，**技能**是工具。

自定义智能体拥有独立的上下文窗口，并且可以在工作过程中协调技能（甚至其他智能体）。在本实验中，无障碍自定义智能体会根据无障碍指南检查并更新网站；在此过程中，它可以调用拉取请求工作流程技能或运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一正确的编写方式。与其他 AI 用法一样，请在自己的环境和场景中测试并不断调整。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前暗色主题中的文字与背景对比度不足，部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。你将使用自定义智能体生成所需功能。

在本练习中，你将：

- 检查现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 检查无障碍自定义智能体

已经为你创建了无障碍自定义智能体。先检查其内容，了解它将如何指导 Copilot。

返回 codespace，然后打开终端，从 `main` 创建新的无障碍工作分支（练习 3 的筛选 PR 会保留在独立分支中）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 注意其中的 YAML frontmatter，包含 `name` 和 `description` 字段。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续章节，了解其中列出的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，聊天会使用该智能体的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型的选择因练习而异；各步骤会在需要时说明。

1. 如果 Chat 视图的智能体下拉列表中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表，其中显示当前智能体；默认情况下为 **default**。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改建议并编辑项目文件。每项编辑都会在聊天中内联显示文件路径和差异，供你检查。
6. 智能体修改文件时，**Files changed** 指示器会更新，显示本次会话中已修改的文件。

> [!NOTE]
> 这可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习将检查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是针对特定任务和领域定制的专用 AI 助手。使用自定义智能体，可以将团队的专业经验和标准编写成可复用的智能体，引导 Copilot 更有效地完成特定类型的工作。

你探索了以下内容：

- 自定义智能体的定义方式。
- 如何在 Copilot Chat agent mode 中使用自定义智能体。

接下来，[监控并引导智能体工作][next-lesson]，在更改进行时检查结果，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
