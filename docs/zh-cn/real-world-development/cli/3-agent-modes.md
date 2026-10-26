---
title: "练习 3 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 商定方案，使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式下检查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们已经为项目添加了一项小功能。但较大的更改需要更严谨的流程。幸运的是，GitHub Copilot CLI 能够配合组织现有的工作流程，帮助我们以正确的方式构建正确的功能。本课是系列课程的第一课，你将遵循典型的智能体驱动开发流程：从议题出发创建新功能，确保代码有效、功能行为符合预期，最终成功合并到项目中。

> [!NOTE]
> 在后续功能工作中，你会继续使用同一对话和分支。通常，不同文件类型的工作会放在不同的分支或 PR 中；这里为了聚焦核心概念，简化了流程。

在本课中，你将：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式下明确需求。
- 使用 Autopilot 模式实现新功能。
- 检查代码。
- 在转发的浏览器中手动验证功能。

后续完成此功能时，你还会更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行商筛选游戏。积压议题描述了这项功能，但组合多个类别等细节需要在编写代码前先商定。你将使用 Plan 模式确定这些细节，然后授权 Autopilot 在限定范围内实现。

## 背景

将 AI 编码智能体引入开发流程，并不会改变基本原则。事实上，这些原则可能更重要了！大多数开发者遵循的流程大致如下：

1. 从已提交、描述待办事项的议题开始。
2. 制定实现方案。
3. 编写并检查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多与上述流程类似。

遵循这一标准流程，可以确保 AI 生成的代码符合既定要求，并经过与手写代码相同的检查。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式之间切换：

- **Interactive**：你和智能体协同工作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，包括编写代码、运行测试，并持续迭代，直到任务完成。

先从 Plan 模式开始，检查计划，然后切换到 Autopilot 模式来实现计划。

## 从议题开始

开始筛选功能的工作前，返回 Codespace 并确保存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，然后返回 **Session** 选项卡。

现在可以看到提示以 `#7`（或类似编号）开头。`#` 可用于将 GitHub 上的议题或拉取请求 (PR) 添加到上下文中。

## 规划筛选功能

规划让你可以在交给 Copilot 实施功能或执行任务前，先明确实现方案。对于复杂任务，花一些时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示框下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：先探索项目，再确定最佳实现方案。

3. Copilot 可能会询问筛选功能应如何工作。根据你的偏好回答即可，没有唯一正确答案！
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并检查计划。
6. 如果计划的任何部分与你的决定不符，请让 Copilot 修改。

## 批准 Autopilot

计划已编写并检查，现在可以开始实现了！让 Copilot 使用 Autopilot 执行任务。

Autopilot 会让 Copilot 持续迭代，直到它认为问题已解决。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前安装版本中含义相同的选项。
2. 确认提示框下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按照计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先检查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再认为检查已完成。

## 检查并验证实现

代码生成后，应像其他代码一样，在合并前进行检查。现在一起检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 对照议题和规划时作出的决定检查结果。
4. 检查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 修复失败项。

## 体验新功能

代码看起来不错，但实际能运行吗？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中商定的组合筛选。
4. 确认重置和无结果时的行为符合议题要求和你的决定。
5. 返回 Codespace，并让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并检查了一项功能。在本课中，你：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式下明确需求。
- 使用 Autopilot 模式实现新功能。
- 检查代码。
- 在转发的浏览器中手动验证功能。

接下来，了解代码生成方式，并确保其符合文档中记录的实践：[使用自定义说明][next-lesson]。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
