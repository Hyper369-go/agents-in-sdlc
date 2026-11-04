---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

几乎所有组织都面临技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置在待办列表中的功能请求。GitHub Copilot cloud agent 能够自主执行更新代码和添加功能等任务。代理完成工作后，会创建一个草稿 PR，供开发者审查。这样可以将繁琐任务交给代理，加快开发流程，让开发者专注于更重要的事项。

你将在本练习中探索以下内容：

- 自定义代码生成环境；
- 确保操作安全；
- 明确界定问题范围的重要性；
- 将问题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档——事实上，代码完全没有文档。首先，他们希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现功能。他们希望每个游戏详情页都显示相关游戏，也就是同一类别中的其他游戏，让用户可以继续浏览。他们暂时不需要完善的实现，只需要一个可用于验收测试用户体验的版本。目前这项工作受到了阻碍，不过还有其他优先级更高的问题。

这些都是容易被降低优先级的任务，也很适合交给 Copilot cloud agent。代理可以在后台异步处理，让开发者专注于其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 介绍 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发者一样在后台执行任务。就像与人类开发者协作一样，有多种方式可以委派工作，包括[将 GitHub 问题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建一个用于跟踪进度的草稿拉取请求，设置环境，然后开始处理任务。无论任务是否完成，你都可以查看 Copilot 会话。提出的解决方案准备好供你审查时，Copilot 会在拉取请求中提及你！

## 说明范围明确的重要性

GitHub Copilot 并没有什么魔法——它无法让你跳过思考。即使看似简单的操作，深入分析后也会发现其复杂性，因此[为 Copilot cloud agent 界定任务范围时要谨慎][cloud-agent-best-practices]。可以把它视为 AI 结对编程助手：分阶段工作、不断学习、试验并调整。生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明以及它们如何指导 Copilot。如果你已经完成自定义说明的实践练习，就会看到 `.github/copilot-instructions.md` 的实际效果；如果还没有，现在正好快速查看一下。将工作分配给 Copilot cloud agent 之前，请只读查看此存储库中已有的说明文件，以便稍后观察它们的作用。

在 GitHub 网站上打开存储库，然后在 GitHub 网页界面中打开以下文件；如果已经启动 codespace，也可以在那里查看：

- `.github/copilot-instructions.md` - 查看 **Code standards** 部分，尤其留意 TypeScript 约定和 TSDoc 文档注释方面的要求。
- `.github/instructions/unit-tests.instructions.md` - 注意 `applyTo` frontmatter，它会将这些说明限定为适用于 `**/*.test.ts` 文件。

在下一节中，将 `Code lacks documentation` 问题分配给 cloud agent 后，留意生成的拉取请求是否采用了 TSDoc 文档注释、TypeScript 约定和注释头；这些都来自上述说明文件。稍后的练习会再次介绍如何审查 PR。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境正常。将任务分配给 Copilot 时也是如此，因为它的工作方式类似软件工程师。

Cloud agent 使用 [GitHub Actions][github-actions] 作为工作环境。你可以通过创建[专用设置工作流][setup-workflow]来自定义环境，将其配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，并在 Copilot 开始工作前运行。这样可以让它访问所需的开发工具和依赖项。为便于实验流程并提供学习机会，我们已预先配置好此文件。它会确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及为单个 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅为*你的步骤*设置所需的最低权限。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要授予 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端 / 应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件看起来与其他 GitHub 工作流文件相似，但有几个关键点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器，它允许你从存储库的 **Actions** 选项卡手动运行工作流。这很适合用来测试工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档很重要，但多数项目的信息要么过时，要么完全缺失。这类技术债务往往长期得不到处理，降低工作效率，也让代码库更难维护或让新开发者更难上手。幸运的是，Copilot 很擅长创建文档，因此非常适合将这项工作交给 Copilot cloud agent。它会在后台生成所需文档。在未来的练习中，你会回来审查其成果。

1. 在新的浏览器选项卡中，打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建问题对话框。
4. 选择 **Blank issue**，创建空白问题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将问题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，你应该会看到 Copilot 创建了一个待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看列表及其计划完成的任务。
14. 向下滚动拉取请求的时间线，你应该会看到 Copilot 已开始处理该问题。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 你可能需要刷新窗口才能看到更新后的指示器。

16. 留意你可以查看实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并决定合适的方案，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是让它能够异步执行任务，使我们可以专注于其他工作。现在就利用这一特性：先将另一项任务分配给 Copilot cloud agent，然后把注意力转向编写代码，为应用添加功能。

## 为游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你专注于一组任务，而它处理另一组任务。设计团队要求添加相关游戏部分，虽然不一定需要很长时间，但这段时间仍可用于处理其他工作。我们把这项任务分配给 Copilot cloud agent 吧！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建问题对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将问题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久之后，你应该会看到问题的第一条评论旁边出现一组 👀，表示 Copilot 已经开始处理！

![Copilot 使用眼睛表情符号表示正在处理问题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理你的第二项请求！Copilot cloud agent 的工作方式类似软件工程师，因此你不必主动监视它，而可以等任务完成后再进行审查。现在先继续了解如何创建和使用自定义代理。

## 总结和下一步

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。使用 cloud agent，可以将问题分配给 Copilot，让它异步处理。你可以用 Copilot 处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

你探索了以下概念：

- 自定义代码生成环境；
- 确保操作安全；
- 明确界定问题范围的重要性；
- 将问题分配给 Copilot。

现在 cloud agent 正在后台认真工作，我们可以开始创建和使用自定义代理。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以访问自定义说明；这些内容已在之前的模块中介绍。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
