---
title: "练习 5：迭代完善 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在整个实验中，你与 GitHub Copilot 协作完成了多项任务，以改进用户体验并添加功能。你让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换开关在内的无障碍功能。现在检查代码更改；必要时提供反馈，帮助 Copilot 改进成果。

### 场景

如前所述，加入生成式人工智能（AI）并不会改变软件设计和开发运维（DevOps）的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。接下来，在为团队其他成员启用审查前，先审查 GitHub Copilot 提出的文档、相关游戏功能和无障碍功能更改。

## 安全与 GitHub Copilot 云智能体

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，因此设置了一些安全限制，以确保操作安全。这些限制包括：

- Copilot 只有存储库读取权限，并且**仅**能写入它用于修改代码的分支。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时环境进行工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括对模型上下文协议（MCP）服务器的访问。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），即为代码添加文档。此处使用 GitHub.com 上的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求，也可能是其他更具体的标题。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会包含 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，可以稍作休息，或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 规范和文件注释头。这些内容来自你在练习 2 开始时审查的自定义指令文件。

6. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 你应会看到某些工作流正在等待批准的指示。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 应能看到工作流在拉取请求的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作处理拉取请求并非单向流程。你可以像标记团队成员一样，在拉取请求评论或代码行内评论中标记 Copilot。Copilot 会读取这些评论，并启动新会话来处理请求。由于结果具有非确定性，我们无法提供固定的提示文本。你可以要求 Copilot 更新以下内容：

- 在每个代码文件顶部添加简要说明其用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论，请求修改生成的文档，并像标记其他用户一样标记 **@copilot**。可以使用上述任一建议，也可以提出其他有关代码库文档的要求。
2. 选择 **View Session**，观察 Copilot 执行工作。留意 Copilot 如何启动新会话来进行更新。
3. 可以选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

如果还不满意，可以继续迭代。满意后，可以将 PR 从草稿转换为准备审查状态，并合并到主分支。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR，也可能是其他更具体的标题。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
6. 你应会看到某些工作流正在等待批准的指示。
7. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 应能看到工作流在拉取请求的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，你可以通过迭代让 Copilot 云智能体继续更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你尚未更新自定义指令！）。与之前一样，可以在 **Conversation** 选项卡添加评论来提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含你在练习 3 中分配的高对比度模式，以及在练习 4 的任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR，也可能是其他更具体的标题。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会包含 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现时，特别留意：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（无障碍富互联网应用（ARIA）标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 你应会看到某些工作流正在等待批准的指示。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 应能看到工作流在拉取请求的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，确认功能正常，并且刷新页面后设置仍然保留。

请注意，无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现了这些功能。如果发现任何无障碍问题或改进点，可以像处理前面几个 PR 一样，在评论中标记 **@copilot** 请求更新。

## 可选练习 — 继续委派任务

云智能体最适合处理待办事项中的真实任务，让你能腾出精力处理其他工作。养成这种习惯的方法是：在存储库中创建更多议题并分配给 Copilot。可以尝试以下内容：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体工作流。通过这些课程，你：

- **检查了此存储库附带的自定义指令文件**，以便之后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**处理无障碍工作，添加高对比度和浅色模式切换开关。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导正在进行的无障碍会话。
- **审查并迭代云智能体创建的拉取请求**，标记 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体工作流。如果想继续探索，其他工作流可以补充你在此处练习的内容：

- 🖥️ **[VS Code 工作流](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作流](../../cli/)** — 在终端中交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

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
