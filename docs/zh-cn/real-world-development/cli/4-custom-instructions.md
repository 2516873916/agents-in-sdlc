---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "了解存储库指令，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能够获取相应指导。[指令文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。现在你已构建筛选功能，接下来将了解 Copilot 使用的指令、添加文档标准，并将其应用于代码。

在本课中，你将：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 了解指令文件对代码的影响。

## 场景

Tailspin Toys 作为一家优秀的软件开发公司，有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持最新。
- 应保留现有格式和 lint 指南。

通过使用指令文件，你可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它可以引导 Copilot 给出更贴合需求的建议和代码片段。你可以为整个存储库创建指令，也可以针对特定类型的文件创建指令，以提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库发出请求时，都会发送此单一指令文件。内容应是适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指南。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

入门项目已包含一组指令文件。修改前先了解现有内容，以便之后观察更改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指南。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件。这里有 Astro 文件、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置了一个 glob，用于确定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意其中指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成较小的可复用文件，并为 Copilot 提供参考示例。

## 更新指令文件以符合团队指南

现有文件是不错的起点，但仍有不足。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含文档注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 将以下内容添加为该部分的最后一项：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指南

Copilot CLI（命令行界面）会在对话开始时加载存储库指令。保存更新后的指令后，退出并恢复筛选对话，使 Copilot 在保留功能上下文的同时加载新指南。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指南：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查发生更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认其准确解释了代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何读取指令文件中的上下文，并将新的标准应用于功能。具体来说，你：

- 了解了存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新了指令文件，确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来，你将[定制并运行可复用的 quality-checks 技能][next-lesson]，确保以一致的方式运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
