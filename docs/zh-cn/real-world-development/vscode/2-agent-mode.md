---
title: "练习 2 - 使用 Copilot Agent Mode 添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者通常需要查找所有相关文件、修改代码、运行测试、调试、找出遗漏的文件、再次更新……这样的过程可能无穷无尽。

Copilot Agent Mode 正是为此而设计的。

Copilot Agent Mode 可以在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行所需更新、运行测试等任务，并自动修复发现的问题。现在看看如何使用 Agent Mode 为网站添加新功能。

> [!NOTE]
> 虽然名称相似，但 agent mode 和 cloud agent 面向不同的使用场景。Agent mode 在 IDE 中执行任务，可以快速反馈并与你交互。Cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解：

- Copilot Agent Mode 如何探索项目、查找相关文件并协调多处更改。
- GitHub Copilot Agent Mode 如何跨 UI 和数据层实现新功能。
- 合并代码前，如何检查 Copilot Agent Mode 生成的更改和测试。

## 场景

随着游戏列表不断扩大，你希望用户可以按类别和发行商筛选。上一练习中已经添加了发行商辅助函数，现在使用 Copilot Agent Mode 完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改前，先浏览 Tailspin Toys 网站，了解当前功能。

网站是一个带有开发者主题的桌游众筹平台。用户可以查看游戏列表和详情。它是一个单独的 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有独立后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过使用标准 DOM API 的小型、局部 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 命令，用于启动单个 Astro 开发服务器。可以按以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，当前应在 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下 URL 的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示，询问是否在浏览器中打开 `http://localhost:4321`，请通过选择 **x** 关闭对话框。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，选择 Codespace 终端中的 localhost URL 链接会自动跳转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 codespace 的专用隧道，网站服务器现在由它托管！

### 浏览网站

网站运行后，可以浏览其功能。主要功能包括：

- **主页**：显示桌游列表及其标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示游戏标题、说明、发行商和类别等信息。

## 使用 Copilot 探索积压事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型的选择因练习而异；各步骤会在需要时说明。

网站的初始实现可以正常工作，但我们希望增加新功能。先查看积压事项。使用模板创建项目时，系统已自动创建一组积压议题；现在让 GitHub Copilot 展示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。该智能体会自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个特定模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此说明中也采用了该模型，希望代码建议较为一致，确保体验良好。不过，由于大型语言模型具有概率性，你收到的建议可能与工作坊中的描述不同。这是正常且符合预期的。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 发送以下提示，让 Copilot 查看积压议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有议题的命令。
5. 检查生成的议题列表。

注意，Copilot 还根据对用户的价值对议题进行了排序。

## 检查说明文件

让智能体生成代码前，先检查用于为 Copilot 提供上下文的说明文件。本练习将使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 说明文件，其中记录了为网站添加功能时应遵循的指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 架构概览。
   - 组件设计、可测试性和无障碍原则。
   - 指向 Astro、Tailwind CSS 和 Drizzle 等文件类型专用说明的链接。

> [!TIP]
> 说明文件既可以引用其他说明文件，也可以引用项目文件。路径相对于说明文件所在位置，因此可以重复使用说明、将大型说明拆分成更易管理的小文件，并在生成代码时提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用进行三项独立更新：

- 在数据层 (`src/lib/`) 中添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，加入筛选 UI。

此外，合并前还需要运行并通过测试。Copilot Agent Mode 可以替你完成这些工作！现在添加此功能。

1. 可以在当前 Copilot 对话中继续，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 说明文件。

   ![选择 UI 说明文件](../../../_images/ex3-select-instructions-file.png)

3. 确保 Chat 视图的智能体下拉列表中仍然选择 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认当前仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据积压中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。你会看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个步骤，例如探索项目、修改文件和运行测试。任务复杂度和代码库大小会影响耗时，可能需要几分钟。处理过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 按钮，因此无需在工作进行中选择。

7. Copilot 提示后，选择 **Continue** 运行测试。

   ![Copilot Chat 对话框询问用户是否愿意运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没有问题！Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且检测不到错误。

   ![Copilot Agent Mode 已完成的 Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请务必检查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能！
11. 确认一切正常并检查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！在本练习中，你了解了如何使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加新功能。你学习了：

- GitHub Copilot Agent Mode 如何跨 UI 和数据层实现新功能。
- Copilot Agent Mode 如何探索项目、查找相关文件并协调多处更改。
- 合并代码前，如何检查 Copilot Agent Mode 生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为它创建拉取请求。

### 附加探索练习 — 实现分页

随着游戏列表不断扩大，需要支持分页。使用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 考虑分页的实现方式：允许用户选择每页大小，还是固定大小。
- 提示中应提供必要指导，让 Copilot 按照你的期望实现功能。
- 可能需要与 GitHub Copilot 多轮迭代，提出修改要求并提供上下文。这是使用 Copilot 的常规方式！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能与适用场景][choose-mode]
- [VS Code 中的 agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
