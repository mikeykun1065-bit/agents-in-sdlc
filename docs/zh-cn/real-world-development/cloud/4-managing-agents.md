---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot cloud agent 承担了三项独立任务，用于改进用户体验和添加功能。cloud agent 虽然以异步、自主的方式工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制中心，可以查看所有带有未关闭拉取请求（PR）的代理任务，了解它执行的操作，甚至引导正在进行的会话。

本课将完成以下任务：

- 探索代理页面并监控 cloud agent 任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给代理来创建高对比度模式后，团队意识到也可以趁此机会添加浅色模式。由于已经在更新网站样式并添加切换功能，将浅色模式一并加入很合适。你将引导代理的工作，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务的状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。这里应会显示你分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从仓库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 此处显示的具体步骤会根据 Copilot 当前的工作状态和所采用的方法而变化。

4. 同时留意右侧的拉取请求（PR）窗格。可以在这里查看 PR 和已更改的文件，以便进一步监控。

## 引导 cloud agent

查看正在进行的任务后，现在可以要求 Copilot 在处理高对比度模式时添加浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。会话标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成设置并开始工作。出现类似以下消息时，就表示已经开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面上的 cloud agent 任务截图，其中高亮显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！收到消息时，它正在进行的步骤完成后，就会把新要求纳入工作流程。

这同样需要几分钟。你可以利用这段时间回顾到目前为止的学习和探索内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监控和引导 GitHub Copilot cloud agent 任务的中心位置。借助这个任务控制中心，你可以跟踪所有活动中和已完成的任务、审查正在进行的工作，甚至重定向进行中的任务，以调整范围或提供额外指导。

本课介绍了以下内容：

- 探索任务控制中心和代理页面，监控 cloud agent 任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来可以[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
