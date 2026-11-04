---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你使用 GitHub Copilot 完成了多项任务，重点是改善用户体验并添加功能。你让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换开关在内的无障碍功能。接下来，我们将查看代码更改，并在必要时向 Copilot 提供反馈，改进其工作。

### 场景

如前所述，生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。基于这一点，在启用其他团队成员的审查之前，先查看 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改。

## 安全性与 GitHub Copilot cloud agent

由于 Copilot cloud agent 会在没有监督的情况下异步执行任务，因此我们设置了一些安全限制，以确保整个过程安全无虞。这些限制包括：

- Copilot 只有存储库的读取权限，并且**只能**写入它用于修改代码的分支。
- Cloud agent 在 GitHub Actions 中运行，并创建一个独立的临时环境来执行工作。
- 任何 GitHub Actions 工作流都必须经过人工批准后才能运行。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求 (PR)，其中包含为代码添加的文档。你将使用 GitHub.com 上的标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到一条警告，提示 GitHub Copilot 被防火墙阻止。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括调用外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或者描述更具体的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。你可以趁此休息一下，或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡，查看更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新添加的 TSDoc 文档注释和其他文档。具体更改可能会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自你在练习 2 开始时查看的自定义说明文件。

6. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到提示，表明有一些工作流正在等待批准。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 你应该会看到工作流已加入拉取请求的检查队列。如果一切正常，单个 Astro 应用的项目检查就会通过。完成检查可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像提及团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新的会话来处理请求。由于结果具有非确定性，我们无法提供确切、固定的提示文本。你可以要求 Copilot 更新以下内容：

- 为每个代码文件添加注释头，简要说明文件的用途；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样提及 **@copilot**。可以使用上面的任一想法，也可以提出其他你希望 Copilot 在代码库中处理的文档需求。
2. 选择 **View Session**，观察 Copilot 执行修改。留意 Copilot 会启动新会话来完成更新。
3. 可以选择 **Back to pull request** 返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应该会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

可以继续迭代，直到满意为止。确认满意后，可以将 PR 从草稿转换为准备审查状态，然后合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在回到 Copilot 为解决游戏详情页显示相关游戏的问题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或者描述更具体的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到提示，表明有一些工作流正在等待批准。
7. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. 你应该会看到工作流已加入拉取请求的检查队列。如果一切正常，单个 Astro 应用的项目检查就会通过。完成检查可能需要几分钟。
9. **可选：** 你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建好相关游戏功能！与之前一样，你可以通过迭代让 Copilot 更新功能。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明！）。和之前一样，可以在 **Conversation** 选项卡中添加新评论提出这些要求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义代理实现的无障碍功能。此 PR 应该同时包含你在练习 3 中分配的高对比度模式，以及你在练习 4 的任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或者描述更具体的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 查看实现，重点留意：
   - 用于切换模式的 UI 组件；
   - 用于保存用户偏好的本地存储；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到提示，表明有一些工作流正在等待批准。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 你应该会看到工作流已加入拉取请求的检查队列。如果一切正常，单个 Astro 应用的项目检查就会通过。完成检查可能需要几分钟。
10. **可选：** 你可以在 Codespace 中切换到此分支，手动测试无障碍功能。在 Codespace 中打开终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换开关，确认功能正常且设置在页面重新加载后仍然保留。

留意无障碍自定义代理如何引导 Copilot 按照无障碍最佳实践实现这些功能。如果发现任何无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习——继续委派任务

Cloud agent 最适合处理真实的待办事项，让你可以把注意力转移到其他工作上。为了养成这种习惯，可以再为存储库创建几个问题并分配给 Copilot。以下是一些想法：

- 在游戏详情页上创建支持者意向表单；
- 在游戏列表页实现分页；
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你完成了 Cloud agent 系列练习。在这些课程中，你：

- **查看了此存储库自带的自定义说明文件**，以便之后观察它们对 cloud agent 输出的影响；
- **将问题分配给 Copilot cloud agent**，观察它设置环境、制定计划并异步执行任务；
- **创建并使用自定义代理**来实现无障碍功能，添加高对比度和浅色模式切换开关；
- **将 Copilot agents 页面用作任务控制中心**，在无障碍会话进行期间监视并引导代理；
- **审查并迭代改进 cloud agent 创建的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和下一步

你已完成 Cloud agent 系列练习。其他系列可以补充你刚刚实践的内容：

- 🖥️ **[VS Code 系列](../../vscode/)**——直接在 IDE 中探索 Copilot Chat Agent 模式和 MCP 集成。
- 💻 **[CLI 系列](../../cli/)**——在终端中交付两项经过审查的更改，学习 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接。

在你自己的存储库中，可以尝试以下后续练习：

- 将重构或测试覆盖率问题分配给 cloud agent；
- 为其他领域创建自定义代理；
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot agents][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
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
