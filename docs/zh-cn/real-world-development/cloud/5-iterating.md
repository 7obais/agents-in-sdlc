---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 检查工作成果

在整个实验中，你与 GitHub Copilot 一起完成了多项改善用户体验和增加功能的任务。你让 Copilot 为代码添加文档、为设计团队实现可迭代开发的相关游戏功能，并实现了高对比度和浅色模式切换等无障碍功能。现在检查代码更改；必要时向 Copilot 提供反馈，改进它的工作。之后再允许团队其他成员开始审查。

### 场景

正如前面多次强调的那样，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要检查生成的代码，并遵循常规 DevOps 流程。因此，在邀请团队其他成员参与之前，先检查 GitHub Copilot 为文档、相关游戏功能和无障碍功能提供的建议。

## 安全与 GitHub Copilot cloud agent

Copilot cloud agent 会在无人监督的情况下异步执行任务，因此设置了一些安全限制，确保操作安全：

- Copilot 仅能读取存储库，并且**只能**写入其工作分支。
- Cloud agent 在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 运行任何 GitHub Actions 工作流之前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 检查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求 (PR)，也就是为代码添加文档的 PR。此练习使用 GitHub.com 上的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括调用外部 MCP 服务器。如有需要，可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。此时请等待 Copilot 完成工作。可能需要几分钟，可以趁此休息一下，或回顾已学内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并检查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 浏览更新后的代码，其中包含新添加的 TSDoc 文档注释和其他文档。具体更改可能有所不同。

    查看时，留意 TSDoc 文档注释、TypeScript 约定和文件注释标题。这些内容来自你在练习 2 开始时查看的自定义说明文件。

6. 检查完更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该能在拉取请求的检查区域看到工作流已排队。如果一切正常，单个 Astro 应用的项目检查应该会通过。这可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像提及团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新会话来处理请求。由于输出具有非确定性，无法给出固定的提示文本。可以要求 Copilot：

- 在每个代码文件顶部添加注释标题，简要说明文件用途。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样提及 **@copilot**。可以使用上面的建议，也可以提出其他文档改进请求。
2. 选择 **View Session**，查看 Copilot 如何执行任务。注意它会启动新会话来进行更新。
3. 选择 **Back to pull request** 可返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成修改后，PR 中应出现新的提交。
5. 选择 **Files changed** 选项卡检查更改。

可以持续迭代，直到满意为止。准备就绪后，可以将 PR 从草稿状态转换为可审查状态，并合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 检查相关游戏功能

现在返回 Copilot 创建的 PR，检查用于解决“在游戏详情页显示相关游戏”议题的更改。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，检查生成的代码。
5. 检查完更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 你应该能在拉取请求的检查区域看到工作流已排队。如果一切正常，单个 Astro 应用的项目检查应该会通过。这可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。前往 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，你可以继续迭代并请求 Copilot 更新。例如，可以调整显示的相关游戏数量，或要求补充注释标题和 TSDoc 文档注释（注意：分配此议题时，自定义说明尚未更新）。和之前一样，可以在 **Conversation** 选项卡中添加评论，提出修改请求，Copilot 会据此启动新会话。

## 检查无障碍功能

最后，检查通过无障碍自定义智能体实现的功能。此 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 在 mission control 中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，检查生成的代码。
5. 检查实现，尤其注意：
   - 用于切换模式的 UI 组件。
   - 如何使用本地存储保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 检查完更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该能在拉取请求的检查区域看到工作流已排队。如果一切正常，单个 Astro 应用的项目检查应该会通过。这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。前往 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，确认功能正常，并且刷新页面后设置仍然保留。

注意，无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进机会，可以像处理前几个 PR 一样，在评论中提及 **@copilot** 请求更新。

## 可选练习 — 继续委托任务

Cloud agent 非常适合处理真实积压事项，让你可以专注于其他工作。可以向自己的存储库提交更多议题并分配给 Copilot，培养这种工作方式。可以尝试：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 学习路径。在这些课程中，你：

- **检查了此存储库附带的自定义说明文件**，以便之后观察它们对 cloud agent 输出的影响。
- **将议题分配给 Copilot cloud agent**，并观察它异步设置环境、规划和执行任务。
- **创建并使用了自定义智能体**，实现高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为控制中心**，监控并引导正在进行的无障碍会话。
- **检查并迭代了 cloud agent 的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

Cloud agent 学习路径已完成。若想继续探索，其他学习路径会延伸你在此处实践的内容：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat agent mode 和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 在终端中通过 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云端智能体连接，交付两个经过审查的更改。

在自己的存储库中，可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
