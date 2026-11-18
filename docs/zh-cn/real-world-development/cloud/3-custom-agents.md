---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。你可以在仓库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业经验整理为可重复使用的代理，例如确保符合 [WCAG][wcag] 要求的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局 `~/.copilot/agents` 目录中定义。每个文件都包含 YAML frontmatter，至少指定 `name` 和 `description`，后面是用于定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理与[代理技能][agent-skills-concept]有些相似。两者主要通过 Markdown 文件定义，并会告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理**负责工作，**技能**提供工具。

自定义代理拥有独立的上下文窗口，能够在工作中协调技能（甚至其他代理）。在本实验中，无障碍自定义代理会依据无障碍指南审查并更新网站；执行过程中，它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的正确方式。和 AI 的其他用法一样，需要针对实际环境和场景进行测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将介绍自定义代理的以下内容：

- 如何定义自定义代理。
- 如何向自定义代理分配任务。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，由于文字和背景颜色对比度不足，有些用户觉得当前深色主题难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍访问至关重要，因此希望尽快实现此功能。接下来将使用自定义代理生成所需功能。

## 审查无障碍自定义代理

已经为你创建了一个无障碍自定义代理。先审查其内容，了解它将如何引导 Copilot。

返回 codespace，然后审查无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 接下来浏览并审查后续章节，其中包括：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 创建并分配议题

任务控制中心是管理环境中所有代理的中心位置。你可以在这里向 Copilot cloud agent 分配任务、监控任务，甚至重定向会话并提供额外指导。接下来，将创建高对比度模式的任务分配给 Copilot。

1. 打开仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，创建新议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![Cloud agent 分配界面的截图，其中高亮显示了自定义代理和无障碍代理](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务！

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。借助自定义代理，可以将团队的专业知识和标准整理为可重复使用的代理，更有效地引导 Copilot 完成特定类型的工作。

本课介绍了以下内容：

- 如何定义自定义代理。
- 如何向自定义代理分配任务。

Copilot 正在实现高对比度模式，接下来可以转到[任务控制中心监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [为组织使用自定义代理做好准备][org-custom-agents]
- [为企业使用自定义代理做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
