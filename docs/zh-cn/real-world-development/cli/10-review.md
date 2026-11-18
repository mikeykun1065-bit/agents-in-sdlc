---
title: "第 10 课 - 回顾与后续步骤"
description: "回顾 Copilot CLI 工作流程、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预先创建的筛选议题。
- 添加了星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式下确定方案，在 Autopilot 模式下实现，并在 Interactive 模式下审查。
- 使用自定义指令引导代理，然后自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议（MCP）服务器，并在真实浏览器中探索筛选功能。
- 创建并选择质量保证（QA）自定义代理，评估需求、覆盖范围、技能结果和浏览器证据。
- 审查了完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 交付的内容

本研讨会包含两个 PR 里程碑，各自基于更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为简化研讨会流程，我们将这些工作整合到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 会将这些结果与需求和测试覆盖范围审查结合起来，供 PR 审查时参考。

添加的测试应弥补实际缺口；QA 运行无需添加测试也可能是正确的结果。合并前请审查代码和证据；更改后应更新受影响的验证证据。

## 最佳实践

你提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置并设置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需要进行调整。指令定义标准，技能描述可重复执行的任务，自定义代理定义专用角色，MCP 服务器则连接外部工具。应审查实际更改和工具结果，而不只是代理的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前确定方案，使用 **Interactive** 参与范围明确的更改过程，使用 **Autopilot** 处理范围清晰的任务。日常修改可选择速度较快的模型，复杂任务则可选择能力更强的模型。

上下文依然和基础设施同样重要。明确描述希望构建*什么*、*为什么*构建以及*如何*构建，会显著影响输出结果。

## 继续探索

核心工作流程已经完成。还可以了解一些其他 CLI 功能：

- `/review`：让代码审查代理分析更改。
- `/rubber-duck`：讨论问题并获取另一种观点。
- `/fleet`：并行协调独立子任务。
- `/worktree`：隔离单独的任务。
- `/delegate`：将任务发送给 Copilot cloud agent。

## 后续步骤

要提升对任何工具的熟练程度，最好的办法就是继续使用它！可以用它处理生产代码、个人项目，或多年来一直想构建却尚未着手的小应用。与团队分享经验，也从团队成员那里学习。还要继续探索相关文档。

如果想通过终端扩展 Tailspin Toys，可以继续可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot cloud agent 研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
