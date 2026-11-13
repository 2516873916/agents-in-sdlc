---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解你想让它做什么，也了解你希望它如何完成。

在本练习中，你将：

- 了解项目专属上下文、编码指南和文档标准如何通过存储库自定义指令和按路径限定的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作流不同，这里不会运行“更新前/更新后”提示。Copilot 云智能体会异步处理 GitHub 议题，因此较难实时并排展示指令的影响。稍后在本工作流中审查云智能体创建的拉取请求时，你会看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 作为一家优秀的软件开发公司，有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明该文件的作用。

通过使用指令文件，你可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它可以引导 Copilot 给出更贴合需求的建议和代码片段。你可以为整个存储库创建指令，也可以针对特定类型的文件创建指令，以提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库发出请求时，都会发送此单一指令文件。内容应是适用于大多数 Copilot 聊天或命令行界面（CLI）请求的项目级信息，例如所用技术栈、构建内容概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可以为特定任务或文件类型创建此类文件。可为 TypeScript、Astro 等特定语言，或创建 UI 组件、新建单元测试等任务提供指南。

> [!NOTE]
> 在 IDE 中工作时，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的方法。总体来说：

- 将 `copilot-instructions.md` 中的指令聚焦于项目级指南，例如构建内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为文件类型（单元测试、Astro 组件、数据层）或特定任务提供专门指令。
- 使用自然语言，保持指南清晰，并提供代码应如何编写或不应如何编写的示例。

创建指令文件没有唯一方法，正如使用 AI 也没有唯一方法。通过实践，你会找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。查看本项目的文件时，你可能会发现其中有适用于多种任务的指令，例如[用户界面（UI）更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；当前界面的课程会在相关处说明。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

花些时间阅读此存储库自带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组适用于各种任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并查看其中的文件。这里有 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置了一个相对于存储库根目录的 glob，用于确定指令适用的文件。此处，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都符合条件。
5. 留意此项目中创建单元测试的专门指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小的可复用文件，并为 Copilot 提供参考示例。（其中的路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求在代码中添加文档。接下来，你将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来只需进行一项编辑：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后在此工作流中将议题分配给云智能体时，它会读取这些规则。

编辑前先创建一个分支（云智能体会读取议题目标分支中的指令，但仍建议将更改整洁地提交）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。注意该部分记录了项目编码标准，但尚无代码文档规则，这正是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如前一课所述，指令文件可以在存储库级别（`.github/copilot-instructions.md`）提供全局指南，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指南。存储库级文件适合放置刚添加的文档注释规则等项目级标准。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一练习中，将议题分配给 Copilot 时，它会从 `main` 分支创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后，云智能体在下一练习处理议题时就会读取这些指令。

> [!TIP]
> 如果想在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处采用分支，是为了模拟真实项目中的处理方式。

## 总结和后续步骤

你了解了 Copilot 如何获取此项目的指令文件上下文，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。在此工作流后续环节中审查云智能体创建的拉取请求时，就能看到该标准的实际影响。

接下来，将第一个议题[分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
