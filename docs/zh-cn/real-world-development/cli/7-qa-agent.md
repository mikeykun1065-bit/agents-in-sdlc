---
title: "第 7 课 - 创建并使用 QA 代理"
description: "创建 QA 自定义代理，整合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在，将这些能力整合到一个自定义代理中，并为其制定明确的 QA 流程。

本课将完成以下任务：

- 了解自定义代理如何配合指令、技能和 MCP 工具工作。
- 创建并检查可复用的质量保证配置。
- 选择 QA 代理，并根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）之前，始终一致地审查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程并提供可复用的报告。

## 什么是自定义代理？

自定义代理是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明代理的用途、指令和可用工具。本研讨会中，你将通过 `.github/agents/qa.agent.md` 定义 QA 角色，并在 Copilot CLI 中选择它。

你使用过的自定义项各有不同用途：仓库指令说明团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 如何使用这些能力评估需求并报告发现。它不会取代其他自定义项，也不需要开始另一个对话。

## 创建 QA 配置

创建功能 PR 前，让 Copilot 创建一个可复用的 QA 配置。该配置会定义 QA 执行的检查及其必须遵循的限制。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 让默认代理创建新的自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新代理前，先审查其配置，确认 Copilot 捕获了预期的 QA 流程和边界。这样可以避免在你只希望验证功能时，代理却更改了功能或扩大工作范围。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必需字段；`name` 为可选字段，但添加后可以为代理提供清晰的显示名称。
3. 阅读配置中的指令，确认 QA 会从需求出发、遵循仓库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持性证据，在更改实现代码前先询问，而且不会提交更改或创建拉取请求。
5. 如果生成的配置遗漏了任何职责或边界，请先要求默认代理修改，再继续。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置文件后，恢复同一筛选对话，再选择 QA，这样它就能使用已有的议题和规划决定。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚刚创建了代理，它可能不会立即出现在可用代理列表中。不过它已存在，上面的命令仍会启用它。

2. 使用以下提示，要求 QA 代理审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 代理会开始工作！
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你已向工作流添加可复用的专用角色，并审查了它的工作成果。本课中，你：

- 了解了自定义代理如何配合指令、技能和 MCP 工具工作。
- 创建并检查了可复用的质量保证配置。
- 选择了 QA 代理，并根据筛选议题审查了它的发现。

现在，功能实现、技能更新、QA 配置、测试和验证报告都已准备就绪，可以进行审查。接下来，[将这些内容整合到功能 PR 中并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
