---
title: "练习 4 - 使用自定义说明指导 Copilot"
description: "探索存储库说明，添加文档规范，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够获取相应指导。[说明文件][instruction-files]不仅描述你想要什么代码，也描述代码应如何组织。现在筛选功能已经完成，你将了解 Copilot 使用的说明、添加文档规范，并将其应用于代码。

在本课中，你将：

- 探索存储库说明和路径范围说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践准则和要求，其中包括：

- 注释应说明意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 文档说明其用途、参数和返回值；如果有可注入的 `db` 参数，也应记录。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持最新。
- 保留现有格式和 lint 指南。

通过说明文件，可以确保 Copilot 获得正确的信息，并按照这些实践完成任务。

## 说明文件

自定义说明可以向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和需求。这是一种强大的引导方式，可以让 Copilot 提供更相关的建议和代码片段。你可以指定首选编码约定、库，甚至希望代码注释采用的方式。说明文件既可以适用于整个存储库，也可以针对特定文件类型。

说明文件有两种：

- `.github/copilot-instructions.md`：单个说明文件，会应用于此存储库中的**每个**请求。文件应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 探索此项目中的自定义说明文件

项目中已包含一组说明文件。先了解现状，再进行更改，以便观察效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，注意其中简要介绍了项目及其编码指导。这些说明适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意这里包含 Astro、Drizzle 数据层、测试等方面的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段：它设置一个 glob，决定这些说明适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和项目现有文件的链接。这种方式可以将较大的说明拆分为更小、可复用的文件，并提供示例供 Copilot 参考。

## 更新说明文件以符合团队准则

现有文件已经打下了良好基础，但仍有不足。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 找到 **Code formatting guidance** 章节，通常位于第 35 行附近。
4. 在该章节的最后添加以下项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。更新说明后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新说明。

1. 如果筛选对话仍在运行，输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并逐一查看已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认内容准确说明了代码。

## 总结和后续步骤

你探索了 Copilot 如何读取说明文件中的上下文，并将新标准应用于功能代码。在本课中，你：

- 探索了存储库说明和路径范围说明文件如何传递给智能体。
- 更新了说明文件，确保遵循编码标准。
- 了解了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，以确保始终运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件及其他资源合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
