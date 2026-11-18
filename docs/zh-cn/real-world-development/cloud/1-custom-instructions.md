---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]可以提供这些指导，让 Copilot 不仅理解你希望它做什么，也理解应该如何完成。

本练习将完成以下任务：

- 了解项目上下文、编码准则和文档标准如何通过仓库自定义指令及按路径应用的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加一项新的仓库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习不同，本练习不会运行前后提示进行对比。Copilot cloud agent 会在 GitHub 议题上异步工作，因此很难实时并排展示指令的影响。稍后审查 cloud agent 创建的拉取请求时，你会看到指令文件发挥的作用。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明该文件的用途。

通过使用指令文件，可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

### 自定义指令

自定义指令用于向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。借助此功能，可以引导 Copilot 提供更符合需求的建议和代码片段。你可以指定偏好的编码约定、库，以及希望在代码中加入的注释类型。可以为整个仓库创建指令，也可以为特定文件类型创建指令，提供任务级上下文。

指令文件有两种类型：

- `.github/copilot-instructions.md`：每次针对该仓库提出请求时，都会将这个单独的指令文件发送给 Copilot。文件应包含项目级信息，即适用于大多数 Copilot Chat 或 CLI 请求的上下文，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建这些文件。例如，为 TypeScript 或 Astro 等语言，或创建 UI 组件、新建单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时，都会使用仓库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

关于如何创建指令文件的完整介绍超出了本研讨会的范围。不过，示例项目展示了具有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的指令集中在项目级指导上，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码应如何编写或不应如何编写的示例。

创建指令文件没有唯一的方法，正如使用 AI 也没有唯一的方法。通过不断尝试，你会找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件集合。探索此项目中的指令文件时，你可能会发现有针对许多任务的文件，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助你生成指令文件。不同产品界面的操作方式不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；当前界面的课程会在相关位置说明具体操作。
>
> 想查找模板或起步示例？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索此项目中的自定义指令文件

花些时间阅读此仓库提供的指令文件——其中包括一个核心 `copilot-instructions.md` 文件，以及一系列针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中简要的项目说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并查看其中内容。注意这里有关于 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意文件顶部的 `applyTo` 字段——它设置一个相对于仓库根目录的 glob，用于确定指令适用的文件。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 留意专门用于在此项目中创建单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可重复使用的文件，并为 Copilot 提供生成代码时可参考的示例。（这些路径是相对于指令文件的，而不是仓库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目编码标准，但目前尚未要求在代码中添加文档注释。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的仓库标准

接下来要进行唯一一次编辑：添加一项项目级规则，要求在代码中使用 TSDoc 文档注释，并在文件开头添加注释头。稍后在本练习中将议题分配给 cloud agent 时，它会应用这项规则。

编辑前，先创建一个分支（cloud agent 会从议题目标分支读取指令；同时，单独提交这些更改更便于管理）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来添加项目编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 章节，该章节应位于第 27 行附近。注意其中记录了项目的编码标准，但还没有关于代码文档的规则，这就是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在仓库级别（`.github/copilot-instructions.md`）创建指令文件以提供全局指导，也可以创建 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。你刚添加的文档注释规则属于项目级标准，应放在仓库级文件中。

## 提交、推送并合并指令更新

Cloud agent 会从议题目标分支读取指令。在下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建新分支。因此，必须先将指令更改合并到 `main`，cloud agent 才能使用这些指令。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中，cloud agent 处理分配给它的议题时就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main` 分支，可以跳过创建分支的步骤，直接提交到 `main`。此处保留分支步骤，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何从项目中的指令文件获取上下文，并向 `.github/copilot-instructions.md` 添加了一项新的仓库级标准。在本练习的后续课程中，你将看到 cloud agent 创建的拉取请求如何应用这项标准。

接下来，将[第一个议题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
