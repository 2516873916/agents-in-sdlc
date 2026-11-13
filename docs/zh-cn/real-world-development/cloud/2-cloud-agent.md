---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题。这可能是尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体能够自主执行代码更新和功能添加等任务。完成后，智能体会创建一个草稿 PR，供开发者审查。这样可以将繁琐任务交由智能体处理，加快开发流程，让开发者专注于更重要的工作。

你将探索 Copilot 云智能体的以下用途：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受聘创建网站的承包商没有留下理想的文档——事实上，代码完全缺少文档。首先，他们希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望每款游戏的详情页都显示相关游戏（即同一类别中的其他游戏），让用户可以继续浏览。他们暂时不需要完善的实现，只需要可用于用户体验验收测试的功能。目前这项工作受阻，但其他议题的优先级更高。

这两项工作都可能很快被降低优先级，因此很适合分配给 Copilot 云智能体。云智能体可以异步处理这些任务，让开发者专注于其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。就像与开发者协作一样，有多种方式可以完成工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求（PR）以跟踪进度、设置环境并开始处理任务。你可以在 Copilot 会话运行期间或完成后查看详情。解决方案准备好供你审查时，Copilot 会在拉取请求中标记你。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法——你仍然需要思考。即使看似简单的操作，深入分析后也会发现复杂性，因此[为 Copilot 云智能体明确界定任务范围][cloud-agent-best-practices]非常重要。应将它视为 AI 结对编程伙伴：分阶段工作、持续学习、尝试和调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令以及它们如何引导 Copilot。如果你已经完成自定义指令的实践练习，就见过 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。将任务分配给 Copilot 云智能体前，先只读查看此存储库已包含的指令文件，以便稍后识别其影响。

在存储库的 GitHub 网页界面中打开以下文件；如果已有运行中的 Codespace，也可以在那里打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` 议题分配给云智能体时，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 规范和文件注释头。这些都来自指令文件。稍后练习中审查 PR 时还会再次提到这一点。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境状态正常。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师（SWE）的方式执行任务。

云智能体使用 [GitHub Actions][github-actions] 环境执行工作。你可以创建一个[专用设置工作流][setup-workflow]，并在 `.github/workflows/copilot-setup-steps.yml` 中进行配置，使其在云智能体开始工作前运行。这样可让它访问所需的开发工具和依赖项。实验已预先配置此工作流，以便顺利完成实验。它会为单体 Astro 应用提供 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几个关键点：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先执行该作业。
- 留意 `workflow_dispatch` 触发器，它允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于验证工作流是否成功运行，而不必等待 Copilot 触发它。

## 添加文档

大家都知道文档的重要性，但大多数项目中的文档要么已经过时，要么完全缺失。这类技术债务往往得不到处理，降低工作效率，也让维护代码库或引入新开发者变得更加困难。Copilot 擅长创建文档，因此很适合将这项任务交给 Copilot 云智能体。它会在后台生成所需文档。后续练习中，你会回来审查其成果。

1. 在新的浏览器选项卡中访问 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 任务分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 创建待办事项列表可能需要几分钟。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳方案。

13. 审查任务列表以及 Copilot 准备完成的工作。
14. 向下滚动拉取请求时间线，应能看到 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适方案，以及编写代码。

这通常需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们能专注于其他工作。接下来，我们将利用这一功能：再分配一项任务给 Copilot 云智能体，同时编写代码为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务，而它处理另一组。为设计团队添加相关游戏部分也许不需要很长时间，但这段时间仍可用于其他工作。现在将此任务分配给 Copilot 云智能体！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，你会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理任务！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理第二项请求！Copilot 云智能体的工作方式类似软件工程师，因此你不需要一直监控它；待任务完成后再审查即可。现在可以开始创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。使用云智能体可以将议题分配给 Copilot 异步处理。你可以让 Copilot 处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

你探索了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

Copilot 云智能体正在后台处理任务，现在可以开始创建和使用自定义智能体。[Copilot 云智能体也可以使用模型上下文协议（MCP）服务器][cloud-agent-mcp]，并且可以读取自定义指令；前面的模块已介绍过这些内容。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
