---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几次练习中，你让 Copilot 云智能体承担了三项独立任务，用于改进用户体验和添加功能。云智能体虽然可以异步自主运行，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在此任务控制中心，你可以查看所有拥有未关闭拉取请求（PR）的智能体任务。还可以探索智能体执行的操作，并在会话进行期间引导它。

在本课中，你将：

- 探索智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求添加额外功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到也可以顺便添加浅色模式。由于已经在更新网站样式并添加切换功能，因此加入此功能也很合理。现在，你将引导智能体的工作，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 访问 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的应用程序编程接口（API）。
    - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，审查 Copilot 已执行的操作，包括：
    - 从存储库检出代码。
    - 创建 Copilot 工作环境。
    - 设置模型上下文协议（MCP）服务器。
    - 执行完成分配任务所需的各项操作。

> [!NOTE]
> 根据 Copilot 的工作状态和采用的方法，列出的具体步骤会有所不同。

4. 另请留意右侧的拉取请求（PR）面板。你可以在此查看 PR 及更改的文件，以便进一步监控。

## 引导云智能体

现在你已查看正在进行的任务，可以在 Copilot 实现高对比度模式时，要求它同时添加浅色模式切换开关。

1. 选择有关添加高对比度模式的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。出现与下方类似的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot 智能体页面中突出显示“Steer active session while copilot is working”对话框的任务界面截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！它会在完成你发送消息时正在执行的具体步骤后，将新请求纳入工作流程。

同样，这需要几分钟。现在可以暂停一下，回顾至今所学和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心枢纽。通过此任务控制中心，你可以跟踪所有活动和已完成的任务、审查正在执行的工作，甚至重定向进行中的任务以调整范围或提供额外指导。

你探索了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，要求添加额外功能。

Copilot 正在完成无障碍功能。接下来，[迭代完善 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
