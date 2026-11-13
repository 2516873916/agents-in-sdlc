---
title: "第 8 课 - 创建并合并功能拉取请求（PR）"
description: "一起审查筛选功能、指令、技能更新、质量保证（QA）配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置和测试都保存在同一个分支上。现在应一起审查这些内容并创建拉取请求。你之前亲自合并了星级评分拉取请求（PR）；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立 PR 中。为了简化研讨会，你将所有筛选和质量工作都保留在同一个对话和分支中，并通过此 PR 一并提交。

在本课中，你将：

- 了解 Agent Merge 及其如何自动化 PR 生命周期中的合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后才启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了一项功能。现在，Tailspin Toys 希望自动化剩余的 PR 工作，同时让开发者掌控合并授权。

## 认识 Agent Merge

**Agent Merge** 会自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的事项，包括修复失败的持续集成（CI）检查、回复审查意见，以及在需要时变基；之后，如果存储库允许，则启用 GitHub 自动合并。

到目前为止，你都是亲自选择 **Merge pull request**。Agent Merge 可以代为处理。在启用 Agent Merge 前，先审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码都已创建，现在一起审查代码、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 激活 Agent Merge。
6. Copilot 将监视 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认看到 Copilot 发出的类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限要求。如果流程受阻，请阅读报告的原因；在存储库允许的情况下，完成审查后手动合并。

## 总结和后续步骤

你已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。你：

- 了解了 Agent Merge 及其如何自动化 PR 生命周期中的合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，你将[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
