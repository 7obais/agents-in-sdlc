---
title: "练习 3 - 自定义智能体"
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

你将使用自定义智能体探索以下内容：

- 自定义智能体的定义方式。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前暗色主题中的文字与背景对比度不足，部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。你将使用自定义智能体生成所需功能。

## 检查无障碍自定义智能体

已经为你创建了无障碍自定义智能体。先检查其内容，了解它将如何指导 Copilot。

返回 codespace，然后查看无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意其中的 YAML frontmatter，包含 `name` 和 `description` 字段。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续章节，了解其中列出的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误。

## 创建并分配议题

Mission control 是管理环境中所有智能体的中心位置。你可以在此将任务分配给 Copilot cloud agent、监控任务，甚至在会话进行中提供额外指导。现在将高对比度模式任务分配给 Copilot。

1. 打开存储库。
2. 选择 Issues 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![Copilot 云端智能体分配对话框，其中突出显示了自定义智能体和无障碍智能体](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务！

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是针对特定任务和领域定制的专用 AI 助手。使用自定义智能体，可以将团队的专业经验和标准编写成可复用的智能体，引导 Copilot 更有效地完成特定类型的工作。

你探索了以下内容：

- 自定义智能体的定义方式。
- 如何将任务分配给自定义智能体。

在 Copilot 实现高对比度模式的同时，接下来可在 mission control 中[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为在组织中使用自定义智能体做准备][org-custom-agents]
- [为在企业中使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
