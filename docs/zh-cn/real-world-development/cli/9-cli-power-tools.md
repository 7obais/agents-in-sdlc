---
title: "练习 9 - GitHub Copilot CLI 中的斜杠命令"
description: "了解用于管理上下文、选择模型、共享会话和可选云端委托的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。它们可以访问高级功能、后台信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来了解其他几个实用命令。

在本课中，你将：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 了解可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和委托给云端智能体的可选命令。

## 场景

核心 CLI 工作流程已完成。现在了解一些额外功能：管理上下文、切换模型、共享会话，以及可选地将工作委托给 [Copilot cloud agent][about-cloud-agent]。

## 查看 Copilot CLI 上下文

处理大型或复杂任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以通过斜杠命令自行检查或压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 使用量，以及系统说明、工具、消息和可用空间之间的上下文分配情况。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口逐渐填满时，Copilot CLI 会自动压缩上下文。想自行决定压缩时机时，可以使用 `/compact`。如果要切换到无关任务并开始全新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，开发者也有不同偏好。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和使用情况信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作并分享经验，有助于提升所有人的 AI 工具使用水平。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中的 `/share` 选项。
2. 如果想共享此会话，请选择适合的目标位置，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送给他人或发布之前，先检查导出的内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要公开团队不打算分享的存储库或对话内容。

## 可选：扩展并行工作或委托任务

核心工作坊内容已完成。Copilot CLI 还提供了适用于更大型任务的命令：

- `/fleet` 可以将独立子任务分配给子智能体，并行执行。
- `/worktree` 可以为单独任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给 Copilot cloud agent，由其异步处理并可能创建拉取请求。

这些命令是可选的，因为它们可能会创建额外的工作树或远程工作。尝试之前，请先开始一项范围明确的新任务，并按照常规流程检查结果。如果想深入了解异步智能体工作，可以继续学习[云端智能体工作坊][cloud-workshop]。

## 总结和后续步骤

Copilot CLI 中的斜杠命令可以帮助你进行配置、共享会话和查看后台信息。在本课中，你：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 了解可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和委托给云端智能体的可选命令。

Copilot CLI 还有更多斜杠命令等待探索！现在通过[回顾所学内容][next-lesson]并了解后续学习资源，为本次学习画上句号。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
