---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发者，你通常需要查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件，再进行修改……事情往往层出不穷。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可以在集成开发环境（IDE）中更加自主地工作。它的行为类似开发者：先探索现有项目结构，进行必要更新，运行测试等任务，并自动修复发现的问题。现在了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但适用于两种不同的体验。智能体模式在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你互动。

在本练习中，你将了解如何：

- 使用 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 使用 GitHub Copilot 智能体模式跨用户界面（UI）和数据层实现新功能。
- 在将代码合并到代码库前，审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断扩大，你希望允许用户按类别和发行商筛选。上一练习中已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先探索 Tailspin Toys 网站，了解其当前功能。

该网站是一个带有开发者主题的棋盘游戏众筹平台。用户可以浏览游戏列表并查看详情。它是一个单体 Astro 应用，构建时会将页面渲染为静态超文本标记语言（HTML）。页面通过 frontmatter 使用 Drizzle ORM 直接查询本地 SQLite 数据库，没有单独的后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问辅助函数放在 `src/lib/` 中；交互功能通过使用标准文档对象模型（DOM）应用程序编程接口（API）的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。可以按照以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后，你应已切回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端窗口。
3. 为筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，应会显示一个包含 URL 的提示，类似如下内容：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost URL 会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有隧道，此时 Web 服务器正在 Codespace 中运行！

### 探索网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示游戏列表，包括标题、图像和描述。
- **游戏详情页**：选择游戏后，会打开详情页，显示标题、描述、发行商和类别等详细信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板，并选择 **New Chat** 开始干净的新对话。模式和模型选择因练习而异；每个相关步骤都会说明。

网站的初始实现已可正常运行，但我们希望添加新功能。先查看待办事项。使用模板设置项目时，系统已自动创建一组议题；让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某种模型。构建本研讨会时，我们使用了 Claude Sonnet 4.5，因此将其列入说明。希望这样能让代码建议相对一致，提供良好的体验。不过，由于大型语言模型（LLM）具有概率性，你可能会收到与研讨会示例不同的建议。这完全正常，也是预期行为。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，询问 Copilot 有哪些待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有议题的命令。
5. 审查生成的议题列表。

请注意，Copilot 还根据它认为对用户最有用的内容为议题排序。

## 审查指令文件

让智能体生成代码前，先审查用于向 Copilot 提供上下文的指令文件。接下来会使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)文件，其中包含向网站添加功能时的指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意有关添加功能的整体指南，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用指令、将复杂内容拆分成更易管理的部分，并在生成代码时为 Copilot 提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行以下三项更新：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前需要运行测试并确保测试通过。Copilot 智能体模式可以完成这些任务！现在添加筛选功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择模型 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。你应能看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 按钮；工作进行时无需选择这些按钮。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 整个过程中可能会暂停，甚至出现测试失败。这没关系！Copilot 会在生成代码和运行测试之间反复迭代，直至任务完成且未发现错误。

   ![Copilot 智能体模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，你了解了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 使用 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 在将代码合并到代码库前，审查 Copilot 智能体模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 奖励探索练习——实现分页

随着游戏列表不断扩大，需要实现分页。使用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可以考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 考虑分页方式：允许用户选择每页大小，还是固定每页大小。
- 编写提示时提供 Copilot 所需的指导，确保按预期实现功能。
- 你可能需要与 GitHub Copilot 迭代，要求更改并提供上下文。这是使用 Copilot 的正常流程！

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
