---
title: "第 10 课 - 回顾与后续步骤"
description: "回顾 Copilot CLI（命令行界面）工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续工作流中使用了 GitHub Copilot CLI（命令行界面）。你：

- 准备了 Codespace，安装 Copilot CLI，探索项目，并找到预置的筛选议题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体，随后定制现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议（MCP）服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择了质量保证（QA）自定义智能体，用于评估需求、覆盖率、技能结果和浏览器证据。
- 审查了完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 交付内容

本研讨会包含两个 PR 里程碑，每个里程碑都基于更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流：**实现筛选、更新指令并将其应用于功能、定制 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建其 PR，你一直使用同一个对话和分支。为了简化研讨会，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查了代码：自动化测试、亲自检查浏览器，以及通过 MCP 使用 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。提交 PR 前，QA 还汇总了这些结果，并审查需求和测试覆盖率。

新增测试应弥补真实的覆盖缺口；QA 运行后无需添加测试也可能是正确结果。授权合并前，请审查代码和证据；更改后应更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、定制了技能、创建了 QA 配置，并配置了 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。指令设定标准，技能描述可重复的任务，自定义智能体定义专门角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前思考方案，使用 **Interactive** 协作完成范围明确的更改，使用 **Autopilot** 处理边界清晰的任务。日常编辑可选择响应更快的模型，复杂工作则选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建**什么**、**为什么**以及**如何**构建，会显著影响输出。

## 更多探索

你已完成核心工作流。还可以了解一些有用的 CLI 功能：

- `/review`：让代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离一项单独任务。
- `/delegate`：将任务交给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最佳方法就是持续使用它！可以用它编写生产代码、兴趣项目代码，或实现你多年来一直想做却尚未着手的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

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
