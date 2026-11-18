---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主完成更新代码、添加功能等任务。代理完成工作后，会创建一个草稿 PR，供开发人员审查。这样可以将繁琐任务交给代理处理，加快开发进度，让开发人员专注于更重要的整体事项。

本练习将使用 Copilot cloud agent 探索以下内容：

- 为代码生成自定义环境。
- 确保安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受聘创建网站的承包商留下的文档质量不佳——实际上完全没有文档。团队希望首先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望每个游戏的详情页面都能显示相关游戏，也就是同一类别中的其他游戏。目前不需要完善的实现，只需要一个可用于用户体验验收测试的版本。此任务目前受阻，但还有其他优先级更高的议题。

这两项任务都可能很快被降级为低优先级，因此很适合分配给 Copilot cloud agent。Copilot cloud agent 可以在后台异步处理任务，让开发人员专注于其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发人员一样在后台执行任务。和与人类开发人员协作一样，有多种方式可以交付任务，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度、设置工作环境并开始处理任务。你可以在会话进行期间或完成后查看 Copilot 的会话。准备好审查建议的解决方案时，Copilot 会在拉取请求中提及你。

## 指令范围的重要性

GitHub Copilot 没有魔法——它不能让你跳过思考过程。即使看似简单的操作，深入分析后也会发现其复杂性，因此[为 Copilot cloud agent 明确界定任务范围][cloud-agent-best-practices]非常重要。应将它视为 AI 结对程序员：分阶段开展工作、学习、尝试并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此仓库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果你已经完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的实际效果；如果还没有，也可以趁此机会快速查看。在将工作分配给 Copilot cloud agent 之前，先以只读方式查看仓库中已有的指令文件，以便稍后观察它们的影响。

在仓库的 GitHub Web 界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 章节，尤其留意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件中。

在下一节中，将 `Code lacks documentation` 议题分配给 cloud agent 时，请留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和注释头——这些要求来自上述指令文件。稍后的练习中审查 PR 时还会再次提到这一点。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保环境状态良好。将任务分配给 Copilot 也是如此，因为它会以类似软件工程师（SWE）的方式执行工作。

Cloud agent 使用 [GitHub Actions][github-actions] 环境开展工作。你可以在 `.github/workflows/copilot-setup-steps.yml` 文件中创建[专用设置工作流][setup-workflow]，自定义它开始工作前的环境。这可以让它访问所需的开发工具和依赖项。为确保实验流程顺利并提供学习机会，我们已预先配置好此工作流。它会为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用程序已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许从仓库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅设置步骤必需的最低权限。Copilot 执行操作时会获得自己的令牌。
    permissions:
      # 例如，如果设置步骤需要克隆仓库来安装依赖项，则需要 `contents: read` 权限。如果设置步骤不克隆仓库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在由 Astro + Drizzle/libSQL 构成）
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

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流文件相同，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器，它允许你从仓库的 **Actions** 选项卡手动运行工作流。这很适合测试工作流是否可以成功运行，无需等待 Copilot 启动它。

## 添加文档

每个人都明白文档的重要性，但大多数项目的信息要么已过时，要么完全缺失。这类技术债务经常被忽视，降低工作效率，也使维护代码库或帮助新开发人员加入团队更加困难。幸运的是，Copilot 很擅长生成文档，因此非常适合将这类议题分配给 Copilot cloud agent。它会在后台生成所需文档。之后的练习中，你会回来审查它的工作。

1. 在新的浏览器选项卡中打开 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，创建新议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建工作环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看待办事项列表和即将执行的任务。
14. 向下滚动拉取请求时间线，应能看到 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 留意你可以滚动查看实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并决定合适的方案，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们能专注于其他工作。接下来会利用这一点：先再向 Copilot cloud agent 分配一项任务，然后转而编写代码，为应用程序添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你专注于一组任务，代理则处理另一组。即使为设计团队添加相关游戏部分不会花太长时间，这段时间也可以用于其他任务。现在就把工作分配给 Copilot cloud agent！

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，使用空白模板。
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

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，你会在议题的第一条评论中看到一组 👀，表示 Copilot 已经开始处理！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二项请求！Copilot cloud agent 的工作方式类似软件工程师（SWE），因此无需主动监控；等它完成后再审查即可。接下来，了解如何创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将议题分配给 cloud agent，让它异步处理任务。可以用 Copilot 处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

本课介绍了以下内容：

- 为代码生成自定义环境。
- 确保安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

cloud agent 正在后台处理任务，接下来可以创建并使用自定义代理。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并能使用前面模块介绍的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
