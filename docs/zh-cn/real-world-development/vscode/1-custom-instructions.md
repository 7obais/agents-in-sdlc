---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Agent mode →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解某些背景信息，就应确保它可以获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[说明文件][instruction-files]指导代码生成。

在本练习中，你将：

- 探索项目专属上下文、编码准则和文档标准如何通过存储库自定义说明和路径范围说明文件传递给 Copilot。
- 使用当前说明生成筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不会遵循你设置的所有标准。Copilot 的行为并非确定。此练习的重点是观察更新说明后行为的变化趋势，而不是要求输出逐字一致。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践准则和要求，其中包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式，并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应有一段注释，说明文件用途。

通过说明文件，可以确保 Copilot 获得正确的信息，并按照这些实践完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和需求。这是一种强大的引导方式，可以让 Copilot 提供更相关的建议和代码片段。你可以指定首选编码约定、库，甚至希望代码注释采用的方式。说明文件既可以适用于整个存储库，也可以针对特定文件类型。

说明文件有两种：

- `.github/copilot-instructions.md`：单个说明文件，会应用于此存储库中的**每个**请求。文件应包含与 Copilot 大多数聊天或 CLI 请求相关的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型创建的说明文件。可用于特定语言（例如 TypeScript 或 Astro），也可用于创建 UI 组件或一组单元测试等任务。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 生成代码，不适用于代码补全或下一处编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本工作坊的范围。不过，示例项目展示了常见做法。概括来说：

- 将 `copilot-instructions.md` 中的内容限定为项目级指导，例如项目简介、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为文件类型（单元测试、Astro 组件、数据层）或特定任务提供具体说明。
- 使用自然语言。让指导清晰明确，并提供代码应如何编写或不应如何编写的示例。

说明文件没有唯一的编写方式，AI 的用法也不止一种。你可以通过试验找到最适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一组完善的说明文件。浏览此项目时，你会看到针对多种任务的说明，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。每个界面都有不同的入口，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前界面的课程会在相关位置介绍具体方法。
>
> 想找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索此项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件：一个核心 `copilot-instructions.md` 文件，以及一组面向不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中对项目的简要说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。在 **Code standards** 下，查看嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意这里包含 Astro、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段：它设置一个相对于存储库根目录的 glob，用于确定说明的适用范围。此处任何 TypeScript 测试文件（例如符合 `**/*.test.ts` 的文件）都会匹配。
5. 查看项目中专用于创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可以将较大的说明拆分为更小、可复用的文件，并提供示例供 Copilot 参考。（这些路径相对于说明文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目的编码标准，但尚未要求代码包含文档注释。接下来，你会添加 TSDoc 文档注释和文件注释标题的规则。

## 为更改创建分支

遵循最佳实践，为此次更改创建一个分支。

1. 从上一练习返回 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型的选择因练习而异；各步骤会在需要时说明。

要观察自定义说明的效果，先在现有说明文件的基础上发送提示。稍后更新说明后，再发送相同提示并比较结果。

1. 关闭之前练习留下的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意，生成的辅助函数是类型化函数，接收 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意，生成的代码**缺少** TSDoc 文档注释和文件级注释标题。

> [!CAUTION]
> Copilot 的行为具有概率性，即使没有明确要求，也可能会添加文档注释。即便如此也没关系；此处重点是观察更新说明后行为的一致性变化。

## 添加存储库级标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在将编码标准写入存储库说明，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 章节，通常位于第 27 行附近。注意其中记录了项目编码标准，但尚未包含代码文档要求，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件可以放在存储库级别（`.github/copilot-instructions.md`），提供全局指导；也可以使用 `*.instructions.md` 文件，为特定语言、文件类型或任务提供说明。存储库级文件适合放置项目级标准，例如刚添加的文档注释规则。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清空对话。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意现在生成的文件开头会有类似以下注释：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新标准，并将其应用于实际代码；下一练习将基于此代码继续开发。

## 提交并推送说明和分支

说明文件与其他项目资源一样，使用相同的源代码管理方式。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改已包含在工作区：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础工作更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他辅助更改（例如测试），也要暂存这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在可以创建拉取请求，并关联到积压中的文档标准议题。也可以手动完成，但这里将让 Copilot 使用已连接到项目的 GitHub 工具查找议题、创建用于关闭议题的 PR，然后合并它。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已为工作区配置 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按提示完成授权。稍后的练习会介绍这种连接的工作原理（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选择 **Agent**。
3. 让 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，说明其中包含说明更新和新的发行商辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 提示你允许它代表你执行 GitHub 操作时，请先查看命令，并酌情选择 **Allow**。
6. PR 创建后，使用以下提示让 Copilot 合并 PR，并将分支切回 main：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 提示你允许它执行 GitHub 操作和 shell 命令时，请先检查命令，再酌情授权。

你已借助 GitHub Copilot 创建并合并拉取请求！

## 总结和后续步骤

你探索了 Copilot 如何读取此项目说明文件中的上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 根据**现有**说明生成的内容。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次发送相同提示，观察生成代码如何采用新标准。
- 将说明更新和辅助函数基础工作提交到 `main`，以便下一练习继续使用。

接下来，在 [agent mode][next-lesson] 中使用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 条建议][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
