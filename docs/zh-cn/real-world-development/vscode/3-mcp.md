---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：Agent 模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用 Agent 模式构建了筛选功能。在创建拉取请求之前，应先确认它在浏览器中能够正常工作。你无需亲自逐个操作应用，而是可以连接 **Playwright MCP 服务器**，让 Copilot 控制真实浏览器为你测试功能，然后发布分支并创建 PR。

在本练习中，你将：

- 了解什么是模型上下文协议 (MCP)，以及 MCP 服务器如何通过新工具扩展 Copilot；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能；
- 发布分支，并为筛选功能创建拉取请求。

## 什么是模型上下文协议 (MCP)？

当 Agent 模式可以访问编辑器之外的资源时，它的能力会大大增强。Copilot 通过模型上下文协议 (MCP) 实现这一点——MCP 是一种代理与外部工具和服务通信的标准方式。

![图示：Agent 模式的内部工作方式，以及它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 代理提供了与外部工具和服务通信的方式。通过 MCP，AI 代理可以实时访问外部工具和服务，从而获取最新信息（使用资源）并代表你执行操作（使用工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁，负责管理双方之间的通信，例如现有 API 或 NPM 包等本地工具。每个 MCP 服务器都代表 AI 代理可访问的一组工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 代理可以执行创建存储库、更新现有存储库以及管理问题和拉取请求等操作。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让 AI 代理可以执行浏览网页、填写表单和选择按钮等操作。

还有许多其他 MCP 服务器可以提供对不同工具和资源的访问。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，帮助用户发现并为生态系统做出贡献。

> [!CAUTION]
> 出于安全考虑，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用 MCP 服务器前，请仔细查看其源代码、验证发布者，并考虑相关安全影响。只使用你信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可使用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

此 `github` 条目由项目模板提供，因此上一练习中 Copilot 才能读取待办问题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容；VS Code 会通过 HTTP 连接服务器，并在 Copilot 首次使用其工具时要求你使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正适合用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记会让 Playwright 在没有可见窗口的情况下运行浏览器。codespace 中没有可显示浏览器的桌面，因此必须使用此标记。

> [!NOTE]
> Tailspin Toys 项目已经在端到端测试中使用 Playwright，因此通常已经安装了 Playwright 所需的浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器——Copilot 首次需要某个服务器提供的工具时，VS Code 会启动服务器，并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问你是否信任该服务器，请先查看配置，然后选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要这些工具时启动服务器，并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 控制真实浏览器。代理可以代替你打开页面、导航、应用筛选条件并读取结果，然后向你总结观察到的内容。无需离开当前对话，这是确认功能行为符合预期的最快方式。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。这意味着代理会像辅助技术一样，根据结构化且带标签的元素（按钮、链接和列表项）进行推理。因此，快速的功能检查也能兼作简单的无障碍检查。

连接服务器并启动应用后，让 Copilot 测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行测试，并报告结果。请根据问题中的验收标准检查它的总结；如果发现异常，可以继续提问，或让它在创建拉取请求前修复代码。

> [!NOTE]
> 测试时，应用必须在 `http://localhost:4321` 上运行。如果开发服务器已经停止，请在发送提示前重新启动。如果 Copilot 首次使用 Playwright MCP 服务器时需要下载浏览器，它可能会报告缺少浏览器；此时让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，让团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，查看 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 所做更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求有多种方式，包括使用 github.com 和 GitHub 命令行界面 (CLI)。既然你正在使用 GitHub Copilot，不妨让它代为创建 PR！它可以找到相关问题，并创建与该问题关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确保在代理下拉菜单中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 提示 Copilot 为你创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 如有提示，选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你进行 GitHub 身份验证；按照提示完成授权即可。
5. 留意 Copilot 如何搜索问题、找到正确的问题并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但**暂时不要合并**。

## 总结和下一步

恭喜！在本练习中，你：

- 了解了模型上下文协议 (MCP)，以及 MCP 服务器如何通过新工具扩展 Copilot；
- 将 Playwright MCP 服务器添加到工作区；
- 使用它在浏览器中手动测试筛选功能，然后再发布；
- 发布分支，并为筛选工作创建拉取请求。

你刚刚使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，我们将[创建自定义代理][next-lesson]，简化无障碍审查等专注型任务。

## 资源

- [MCP 到底是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
