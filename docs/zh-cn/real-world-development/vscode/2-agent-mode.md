---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发人员的工作流程通常包括：查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次更新……后续工作还会不断增加。

Copilot 代理模式正是为此而设计的。

代理模式可以在 IDE 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，再执行必要更新、运行测试等任务，并自动修复发现的问题。接下来，了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和 cloud agent 名称相似，但它们针对两种不同的体验。代理模式在 IDE 中执行任务，支持快速反馈和交互。Cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你互动。

本练习将介绍以下内容：

- Copilot 代理模式如何探索项目、识别相关文件并协同修改。
- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能。
- 合并到代码库之前，如何审查 Copilot 代理模式生成的更改和测试。

## 场景

游戏列表逐渐变长，因此你希望允许用户按类别和 publisher 筛选。上一练习已添加 publisher 辅助函数，现在将使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始更改之前，先探索 Tailspin Toys 网站，了解其现有功能。

该网站是一个以开发者为主题的桌游众筹平台。用户可以浏览游戏列表并查看游戏详情。网站是单个 Astro 应用程序，构建时会将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库——没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能则通过使用标准 DOM API 的精简 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤，在 GitHub Codespace 中运行网站：

1. 返回 codespace。上一练习结束后，你应该已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅，其中包含网站 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，从 Codespace 终端选择 localhost URL 会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 codespace 的专用隧道，网站服务器现在就在其中运行。

### 探索网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示桌游列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后，会打开详情页，显示游戏标题、说明、publisher 和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新的对话。不同练习所用的模式和模型可能不同；每个步骤会在相关位置说明。

网站的初始实现已经可用，但我们希望添加新功能来改进它。先查看待办事项列表。通过模板设置项目时，系统已自动创建议题列表——请 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此说明中也选用了该模型。这样有助于保持代码建议相对一致，提供良好的体验。不过，大型语言模型（LLM）的行为具有概率性，因此你收到的建议可能与研讨会示例不同。这是正常的预期行为。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，让 Copilot 显示待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 还为你排列了议题优先级，依据是它判断这些议题对用户的帮助程度。

## 审查指令文件

让代理生成代码前，先审查将用于提供工作上下文的指令文件。这里将使用 [UI（用户界面）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)文件，其中包含向网站添加功能的指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于添加功能的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各种文件类型专用指令文件的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置，因此可以复用内容，将复杂指令拆分为更易管理的小文件，并提供示例和模板。

## 实现筛选功能

至少需要对应用程序进行三项更改，才能完成筛选：

- 在数据层（`src/lib/`）添加或完善其余筛选逻辑。
- 添加或更新筛选行为的测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并到代码库前必须运行并通过测试。Copilot 代理模式可以为你完成这些任务，接下来就添加筛选功能。

1. 可以继续当前与 Copilot 的对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图代理下拉菜单中仍选中了 **Agent**。

   ![显示 Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办列表中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。你会看到它查找数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会提供 **Keep** 或 **Undo** 选项，因此不需要在工作进行期间选择它们。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板中要求用户确认是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有几次暂停，甚至出现测试失败。这没关系！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且不再发现错误。

   ![Copilot 代理模式完整聊天会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行着网站的浏览器，探索新功能！
11. 确认所有功能正常且已审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你已了解以下内容：

- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能。
- Copilot 代理模式如何探索项目、识别相关文件并协同修改。
- 合并到代码库之前，如何审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，然后为其创建拉取请求。

### 加分探索练习——实现分页

随着游戏列表不断增长，需要添加分页。运用本练习学到的技巧，提示 Copilot 更新网站并实现分页。可以考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 规划分页的实现方式，决定由用户选择每页大小，还是使用固定值。
- 编写提示时，提供足够指导，让 Copilot 按照你的预期实现功能。
- 可能需要与 GitHub Copilot 多次迭代，要求更改并补充上下文。这是使用 Copilot 时的正常工作流程！

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
