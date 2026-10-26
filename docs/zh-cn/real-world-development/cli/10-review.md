---
title: "练习 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流程、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的完整工作流程中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace、安装了 Copilot CLI、探索了项目，并找到了预置的筛选议题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式下确定方案，在 Autopilot 模式下实现，并在 Interactive 模式下检查。
- 使用自定义说明指导智能体，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议 (MCP) 服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，以检查需求、覆盖率、技能结果和浏览器证据。
- 检查完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于上下文、模型、共享和可选云端委托的斜杠命令。

## 交付的成果

本工作坊包含两个 PR 阶段，每个阶段都从更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流程**：实现筛选、更新说明并将其应用到功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建 PR，你一直使用同一对话和分支。为了简化工作坊，我们将这些工作合并到了一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及使用 Copilot 通过 MCP 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新的格式报告结果。QA 在 PR 创建前综合检查了这些结果、需求和测试覆盖率。

新增测试应弥补实际缺口；如果 QA 运行后发现无需添加测试，也可能是正确结果。授权合并前，请检查代码和证据，并在更改后重新验证受影响的部分。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置，并配置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求进行调整。说明用于设定标准，技能描述可重复执行的任务，自定义智能体定义专门角色，MCP 服务器连接外部工具。请检查实际更改和工具结果，不要只看智能体的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前考虑实现方案，使用 **Interactive** 处理需要持续参与的重点更改，使用 **Autopilot** 完成范围明确的任务。常规编辑可选择速度更快的模型，复杂工作则可选择能力更强的模型。

上下文仍然和基础设施一样重要。清楚描述要构建的内容、原因和方式，会显著影响输出。

## 继续探索

你已完成核心工作流程。还可以了解以下 CLI 功能：

- `/review`：请求代码审查智能体分析更改。
- `/rubber-duck`：梳理问题并获取另一种看法。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot cloud agent。

## 后续步骤

提升工具使用能力的最佳方式就是持续使用！可以将它用于生产代码、个人项目，或多年一直想做但还没开始的小应用。与团队分享经验，也向他们学习。还可以继续探索文档。

如果想在终端中扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。如果想比较其他环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]，或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
