---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。你可以通过存储库中的 `.github/agents` Markdown 文件定义代理，并提供重点明确的指令、最佳实践、编码模式和领域知识，帮助 Copilot 更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的代理，例如确保符合 [WCAG][wcag] 要求的无障碍访问代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在 `~/.copilot/agents` 中进行全局定义。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description`，后面是用于定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]有些相似。两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理是执行工作的角色，技能是工具**。

自定义代理拥有独立的上下文窗口，负责协调技能（甚至其他代理）来完成任务。在本练习中，无障碍访问自定义代理会根据无障碍指南审查并更新网站；执行过程中，它可以调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一正确的编写方式。与 AI 的其他用途一样，需要通过测试和迭代，找到适合环境和场景的方法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

自定义代理练习将介绍：

- 如何定义自定义代理。
- 如何向自定义代理分配任务。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视力状况或偏好如何。近期用户反馈指出，当前暗色主题的文本与背景对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍访问至关重要，因此你希望尽快实现此功能。接下来将使用自定义代理生成此功能。

## 审查无障碍访问自定义代理

系统已为你创建了一个无障碍访问自定义代理。先查看其内容，了解它将如何指导 Copilot。

返回 Codespace，然后查看无障碍访问自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 接着浏览后续部分，了解其中介绍的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍访问最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见错误和问题。

## 创建并分配问题

任务控制中心是管理环境中所有代理的中心位置。你可以在此为 Copilot cloud agent 分配任务、监控任务，还可以重定向并提供额外指导。现在，将创建高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue** 创建空白问题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从列表中选择 **Accessibility agent**。

    ![Cloud agent 分配界面截图，突出显示自定义代理和无障碍访问代理](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 中的自定义代理][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。自定义代理可以将团队专业知识和标准整理成可复用的配置，指导 Copilot 更有效地完成特定类型的工作。

你了解了以下内容：

- 如何定义自定义代理。
- 如何向自定义代理分配任务。

Copilot 正在后台实现高对比度模式。接下来，你将从任务控制中心[监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [在组织中准备使用自定义代理][org-custom-agents]
- [在企业中准备使用自定义代理][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
