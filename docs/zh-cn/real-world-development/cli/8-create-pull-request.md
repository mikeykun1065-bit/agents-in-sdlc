---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置和测试都已保存在同一个分支中。现在可以一起审查这些更改并创建拉取请求。之前，你亲自合并了星级评分拉取请求（PR）；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 代理拆分为几个独立的 PR。为简化研讨会流程，筛选和质量工作流都保留在同一个对话和分支中，并通过此 PR 一并提交。

本课将完成以下任务：

- 了解 Agent Merge 以及它如何自动管理合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动处理剩余的 PR 工作，同时由开发人员掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 可以自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的事项——修复失败的持续集成（CI）检查、回应审查评论，并在必要时变基——然后在仓库允许的情况下启用 GitHub 自动合并。

到目前为止，你都是亲自选择 **Merge pull request**。Agent Merge 可以代为处理这项工作。启用之前，请先审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

所有代码都已创建，现在一起审查更改、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。一切成功后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的批准、分支保护、合并队列、仓库设置或权限限制。如果合并受阻，请阅读报告的原因；在仓库允许的情况下，完成审查后手动合并。

## 总结和后续步骤

你已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，现在还包括拉取请求流程。本课中，你：

- 了解了 Agent Merge 以及它如何自动管理合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
