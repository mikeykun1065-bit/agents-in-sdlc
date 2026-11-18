---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索仓库指令、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应让 Copilot 能获取相应指导。[指令文件][instruction-files]不仅描述你希望生成什么代码，也说明代码应如何组织。现在筛选功能已完成，你将探索 Copilot 使用的指令、添加文档标准，并将其应用于代码。

本课将完成以下任务：

- 了解仓库指令和按路径应用的指令文件如何提供给代理。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 注释其用途、参数和返回值；如果函数接受可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 应保留现有格式和 lint 指导。

通过使用指令文件，可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

## 指令文件

自定义指令用于向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。借助此功能，可以引导 Copilot 提供更符合需求的建议和代码片段。你可以指定偏好的编码约定、库，以及希望在代码中加入的注释类型。可以为整个仓库创建指令，也可以为特定文件类型创建指令，提供任务级上下文。

指令文件有两种类型：

- `.github/copilot-instructions.md`：每次针对该仓库提出请求时，都会将这个单独的指令文件发送给 Copilot。文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 探索此项目中的自定义指令文件

起始项目中已包含一组指令文件，可以先探索已有文件，再进行更改并观察效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，注意项目简介和编码指导。这些指令适用于本仓库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并查看其中内容。注意这里有关于 Astro 文件、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件顶部的 `applyTo` 字段，它设置 glob 来决定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可重复使用的文件，并为 Copilot 提供可参考的示例。

## 更新指令文件以符合团队指导

现有文件是不错的起点，但仍有缺漏。接下来修改核心 `copilot-instructions.md` 文件，确保为新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 章节，该章节应位于第 35 行附近。
2. 将以下内容作为最后一条项目添加：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载仓库指令。保存更新后的指令后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在仓库根目录的 `game-filters-cli` 分支中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选对话并确认当前检出的是 `game-filters-cli` 分支。
3. 使用以下提示，让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，查看已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从指令文件获取上下文，并将新标准应用于功能代码。具体来说，你：

- 了解了仓库指令和按路径应用的指令文件如何提供给代理。
- 更新了指令文件，确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
