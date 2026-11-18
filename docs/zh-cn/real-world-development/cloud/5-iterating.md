---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 协作完成了多项任务，以改进用户体验并添加功能。你要求 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，并实现高对比度和浅色模式切换等无障碍功能。接下来，探索代码更改，并在需要时向 Copilot 提供反馈，改进其工作成果。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。基于这一点，在为团队其他成员启用审查之前，先审查 GitHub Copilot 对文档、相关游戏功能和无障碍功能的建议。

## 安全性和 GitHub Copilot cloud agent

Copilot cloud agent 会异步、无人监督地执行任务，因此设有一些安全限制，确保整个过程安全：

- Copilot 对仓库只有读取权限，对其创建的工作分支才有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并在单独的临时环境中开展工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求（PR），其中包含为代码添加文档的更改。通过 GitHub.com 上的标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你也可以[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。请等待 Copilot 完成工作。这可能需要几分钟，可以先休息一下，或回顾迄今为止学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新创建的 TSDoc 文档注释和其他文档。具体更改可能会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些来自你在练习 2 开始时审查过的自定义指令文件。

6. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入拉取请求的检查队列。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作处理拉取请求并非单向过程。你可以像在拉取请求中提及团队成员一样，在评论或代码行内评论中提及 Copilot。Copilot 会看到这些评论，并启动另一个会话来处理请求。由于结果具有非确定性，无法提供一条适用于所有情况的固定提示。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建说明 Astro 应用结构的 README。

1. 添加评论，请求 Copilot 更改生成的文档，并像提及用户一样标记 **@copilot**。可以使用上面的建议，也可以提出其他文档相关要求。
2. 选择 **View Session**，观察 Copilot 执行任务。留意 Copilot 会启动新会话来进行更新。
3. 选择 **Back to pull request**，返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

可以继续迭代，直到满意为止。确认满意后，可以将 PR 从草稿转换为准备审查状态，然后合并到 main 分支。

![将 PR 转为可供审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来，查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流提示。
7. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会进入拉取请求的检查队列。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！和之前一样，可以通过迭代让 Copilot cloud agent 持续更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令！）。和之前一样，可以在 **Conversation** 选项卡中添加评论来提出要求，Copilot 会看到评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义代理实现的功能。这个 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心要求的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 查看实现时，重点留意：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保存用户偏好的方式。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入拉取请求的检查队列。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用程序，并在浏览器中测试高对比度和浅色模式切换，确认功能符合预期且在刷新页面后仍保留设置。

注意，无障碍自定义代理帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现任何无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派工作

当你能把真实待办事项交给 cloud agent 并转而处理其他工作时，它的效果最好。可以在自己的仓库中创建更多议题并分配给 Copilot，培养这种工作习惯。例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 练习。在这些课程中，你：

- **检查了仓库提供的自定义指令文件**，以便之后观察它们对 cloud agent 输出的影响。
- **将议题分配给 Copilot cloud agent**，并观察它如何设置环境、制定计划并异步执行任务。
- **创建并使用无障碍自定义代理**，添加高对比度和浅色模式切换。
- **将 Copilot agents 页面用作任务控制中心**，监控并引导无障碍会话。
- **审查并迭代改进 cloud agent 的拉取请求**，提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 练习。如果想继续探索，其他练习也可以补充本次所学内容：

- 🖥️ **[VS Code 练习](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 练习](../../cli/)** — 在终端中使用 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 桥接功能，将两项更改交由云端代理审查和交付。

在自己的仓库中，可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义代理。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
