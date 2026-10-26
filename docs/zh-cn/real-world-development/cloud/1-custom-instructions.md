---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解某些背景信息，就应确保它可以获取这些上下文。[说明文件][instruction-files]可以提供此类指导，让 Copilot 不仅了解你希望它做什么，也了解如何完成任务。

在本练习中，你将：

- 探索项目专属上下文、编码准则和文档标准如何通过存储库自定义说明和路径范围说明文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，这里不会通过前后对比提示来展示效果。Copilot cloud agent 会在 GitHub 议题上异步工作，因此很难实时并排演示影响。后续在本环境中检查云端智能体创建的拉取请求时，可以看到说明文件的效果。

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

## 添加存储库级标准

接下来只需进行一项编辑：添加一条项目级规则，要求以 TSDoc 文档注释和文件级注释标题的形式在代码中编写文档。之后，cloud agent 会在本环境的后续练习中处理议题时应用这条规则。

编辑前，先创建一个分支（cloud agent 会读取议题目标分支上的说明，但你仍应将修改作为独立提交处理）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并说明更新

Cloud agent 会读取议题目标分支上的说明。下一练习中将议题分配给 Copilot 时，它会从 `main` 创建分支，因此必须先将说明更新合并到 `main`，cloud agent 才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。这样，cloud agent 在下一练习中处理分配给它的议题时就会读取这些说明。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main` 分支，可以跳过分支步骤并直接提交到 `main`。此处创建分支，是为了让工作坊更贴近真实项目的处理方式。

## 总结和后续步骤

你探索了 Copilot 如何读取此项目说明文件中的上下文，并向 `.github/copilot-instructions.md` 添加了存储库级标准。后续在本环境中查看 cloud agent 创建的拉取请求时，可以看到这条标准的实际效果。

接下来，[将第一个议题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 条建议][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
