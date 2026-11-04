---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。你可以通过存储库中的 `.github/agents` 文件夹内的 Markdown 文件定义代理，为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识编写成可复用的代理，例如确保符合 [WCAG][wcag] 标准的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，其中至少要有 `name` 和 `description` 字段，随后是定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理**是执行任务的工作者，**技能**则是工具。

自定义代理有自己的上下文窗口，并用于协调技能（甚至其他代理）完成工作。在本实验中，无障碍自定义代理会根据无障碍指南审查并更新网站；在此过程中，它可以调用诸如拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的正确方式。与 AI 的其他用法一样，应在自己的环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，部分用户认为当前暗色主题难以阅读，因为文字与背景颜色之间的对比度不足。为解决这一无障碍问题，设计团队要求实现一种可供用户开启或关闭的高对比度模式。

无障碍至关重要，因此你希望尽快实现此功能。接下来将使用自定义代理生成此功能。

在本练习中，你将：

- 查看现有的无障碍自定义代理；
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

我们已经为你创建了一个无障碍自定义代理。先查看它的内容，了解它将如何引导 Copilot。

返回 codespace，打开终端，然后从 `main` 创建新分支并切换到该分支，以单独开展无障碍工作（练习 3 的筛选 PR 将保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意其中包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 接下来，浏览后续部分，了解其中强调的内容：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见陷阱和错误清单。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 中定义的所有自定义代理。你可以选择自定义代理，让聊天会话使用该代理的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的新会话。每个练习对模式和模型的选择可能不同；需要时会在相应步骤中说明。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**（如果尚未选中）。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉菜单（它会显示当前启用的代理，默认是 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改建议并将编辑应用到项目文件。每项编辑都会以内联方式显示在聊天中，并包含文件路径和供你审查的差异。
6. 随着编辑逐步应用，**Files changed** 指示器会更新，显示代理在此过程中修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改你的存储库，例如 Astro 组件、CSS 和相关测试。

下一练习将介绍如何审查并引导这项正在进行的工作。

## 总结和下一步

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。通过自定义代理，可以将团队的专业知识和标准编写成可复用的代理，引导 Copilot 更有效地完成特定类型的工作。

你了解了以下概念：

- 如何定义自定义代理；
- 如何在 Copilot Chat Agent 模式中使用自定义代理。

接下来，你将[监视并引导代理的工作][next-lesson]，查看更改并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [Awesome Copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监视和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
