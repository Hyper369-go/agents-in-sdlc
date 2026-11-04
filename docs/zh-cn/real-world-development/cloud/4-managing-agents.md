---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot cloud agent 承担了三项不同任务，帮助改进用户体验并添加功能。cloud agent 虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制中心，你可以查看所有创建了开放拉取请求（PR）的代理任务、探索代理执行的操作，还可以引导正在运行的会话。

本课将学习如何：

- 探索代理页面并监控 cloud agent 任务。
- 引导正在运行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给代理后，团队意识到也可以同时添加浅色模式。由于代理已经在更新网站样式并添加切换功能，将此功能一并纳入似乎很合理。你将引导代理的工作，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看分配给 Copilot cloud agent 的所有任务的当前状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 为游戏修改功能生成 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，包括：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行其他任务步骤。

> [!NOTE]
> 显示的具体步骤会因 Copilot 当前的工作状态和执行方式而异。

4. 同时注意右侧显示的拉取请求（PR）窗格。你可以在此查看 PR 和已更改的文件，以便进一步监控任务。

## 引导 cloud agent

现在你已了解正在运行的任务，可以在 Copilot 实现高对比度模式时，让它同时添加浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制中心中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 监控会话几分钟，直到代理完成设置并开始工作。出现类似以下内容的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中 cloud agent 任务的截图，突出显示“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示并将其纳入工作流程。

## 等待 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务！发送消息时，它会先完成当前步骤，然后再将新要求纳入工作流程。

与之前相同，此过程需要几分钟。你可以利用这段时间回顾到目前为止的学习和探索内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心位置。通过此任务控制中心，你可以跟踪正在运行和已完成的任务、审查代理所做的工作，也可以重定向正在运行的任务，调整范围或提供额外指导。

你了解了以下内容：

- 使用任务控制中心和代理页面监控 cloud agent 任务。
- 重定向正在运行的会话，请求添加其他功能。

Copilot 正在完成无障碍访问功能。接下来，你将[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
