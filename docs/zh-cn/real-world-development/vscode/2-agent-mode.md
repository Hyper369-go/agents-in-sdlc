---
title: "练习 2 - 使用 Copilot Agent 模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发者，你通常需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件，再进行修改……这样的工作会不断重复。

Copilot Agent 模式正是为此而设计的。

Copilot Agent 模式可以在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。接下来，我们将了解如何使用 Agent 模式为网站添加新功能。

> [!NOTE]
> Agent 模式和 cloud agent 名称相似，但适用于两种不同的体验。Agent 模式在 IDE 中执行任务，让你可以快速反馈和交互。Cloud agent 则像团队中的同事一样异步工作，通过问题和拉取请求与你协作。

在本练习中，你将了解如何：

- 让 Copilot Agent 模式探索项目、识别相关文件并协调多处更改；
- 使用 GitHub Copilot Agent 模式跨 UI 和数据层实现新功能；
- 在将更改合并到代码库之前，审查 Copilot Agent 模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户能够按类别和出版社筛选游戏。上一练习中已经添加了出版社辅助函数，现在将使用 Copilot Agent 模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改之前，先浏览 Tailspin Toys 网站，了解它目前的功能。

该网站是一个以开发者为主题的棋盘游戏众筹平台。用户可以浏览游戏列表并查看游戏详情。网站是一个 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过精简、范围明确的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器，让网站更易于运行。按以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应该已经切换回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新的终端窗口。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示一个横幅并提供 URL，内容类似这样：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示框，询问是否要在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，在 Codespace 终端中选择 localhost URL 链接会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 codespace 的私有隧道，网站服务器现在就在其中运行。

### 浏览网站

网站运行后，可以浏览它的功能。主要功能包括：

- **主页**：显示棋盘游戏列表，包括游戏标题、图片和简介。
- **游戏详情页**：选择游戏后会打开详情页，其中包含游戏标题、简介、出版社和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的新会话。每个练习对模式和模型的选择可能不同；需要时会在相应步骤中说明。

网站的初始实现已经可以正常使用，不过我们还想增加一些功能。先查看待办事项。你从模板创建项目时，系统已经自动为你创建了一组问题；请让 GitHub Copilot 列出这些事项。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时使用的是 Claude Sonnet 4.5，因此说明中选用了该模型，希望由此获得相对一致的代码建议，确保良好的学习体验。不过，由于 LLM 的输出具有概率性，你可能会发现得到的建议与研讨会中展示的内容不同。这完全正常，也是预期的情况。

> [!NOTE]
> 由于 LLM 输出具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办事项列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有问题的命令。
5. 查看生成的问题列表。

留意 Copilot 还根据它认为对用户最有用的程度，为这些事项排定了优先级。

## 查看说明文件

让代理生成代码之前，先查看将用于为 Copilot 提供上下文的说明文件。我们将使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 文件，其中包含如何为网站添加功能的相关上下文。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的整体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍的原则；
   - 指向各类文件专用说明的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目中的文件。路径相对于说明文件所在位置，因此可以复用内容、将大型说明拆分成更小且易于管理的部分，还可以提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项不同的更新：

- 在数据层（`src/lib/`）添加或完善其余的筛选逻辑；
- 添加或更新筛选行为的测试；
- 更新游戏列表页面，加入筛选 UI。

此外，在将更改合并到代码库之前，还需要运行并通过测试。Copilot Agent 模式可以帮你完成这些任务！现在开始添加此功能。

1. 可以继续当前与 Copilot 的对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单中仍选中 **Agent**。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍选中 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中相关问题的要求实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找与目标功能相关的文件。你应该会看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示相应的 **Keep** 或 **Undo** 按钮，因此无需在工作进行期间选择它们。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 整个过程中可能会出现暂停，也可能有测试失败。这没有关系！Copilot 会在代码生成和测试之间反复迭代，直到完成任务并且不再检测到错误。

   ![Copilot Agent 模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，看看是否存在潜在问题。

> [!CAUTION]
> 请记住，务必审查 Copilot 或任何 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和下一步

恭喜！在本练习中，我们了解了如何使用 GitHub Copilot Agent 模式为 Tailspin Toys 网站添加新功能。我们学习了如何：

- 使用 GitHub Copilot Agent 模式跨 UI 和数据层实现新功能；
- 让 Copilot Agent 模式探索项目、识别相关文件并协调多处更改；
- 在将更改合并到代码库之前，审查 Copilot Agent 模式生成的更改和测试。

接下来，让我们使用 [Playwright MCP 服务器测试功能][next-lesson]，然后为其创建拉取请求。

### 加分探索练习——实现分页

随着游戏列表不断增长，需要启用分页。运用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用已有的说明文件；
- 考虑如何实现分页，例如是否允许用户选择每页大小，还是将其固定为某个值；
- 编写提示时，提供 Copilot 所需的指导，确保实现符合你的预期；
- 可能需要与 GitHub Copilot 反复迭代，提出更改要求并提供上下文。这是使用 Copilot 的正常工作流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：各自的作用及适用时机][choose-mode]
- [VS Code 中的 Agent 模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
