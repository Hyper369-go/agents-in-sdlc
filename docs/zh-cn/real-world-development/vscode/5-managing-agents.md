---
title: "练习 5 - 监视和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：自定义代理][previous-lesson]
|:--|

让 GitHub Copilot 进入 Agent 模式后，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作会实时发生在你的计算机上，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接在编辑器中查看每个差异、接受或拒绝单项更改，并通过后续提示引导对话，以改进或扩展工作。

在本练习中，你将：

- 实时监视无障碍代理的工作；
- 审查并接受建议的文件编辑；
- 在会话进行期间引导代理，添加浅色模式切换开关，并保留现有高对比度模式。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作进行期间，设计团队又提出了一项需求：他们也希望为觉得默认主题太暗的用户添加浅色模式切换开关。无需从头开始新对话，你可以引导当前会话扩展现有工作。

## 监视 Copilot 的进度

Copilot Chat 视图会显示代理执行的每个步骤，包括它探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联方式显示在聊天中，并附带 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 代理工作期间，滚动聊天内容，查看每次工具调用。你会看到文件读取、文件编辑以及代理要求运行的终端命令。
3. 代理提出文件编辑时，差异会以内联方式显示：
    - 选择 **Keep** 接受更改；
    - 选择 **Undo** 撤销更改，让代理重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看代理在本次会话中修改的完整文件列表。

> [!NOTE]
> 如果工具调用需要权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待代理完成高对比度模式。完成后，你应该会看到一条总结其更改的最终消息。

## 引导代理添加浅色模式

现在高对比度模式已经就绪，接下来在同一对话中添加浅色模式切换开关。由于继续使用同一聊天会话，代理会保留先前工作的上下文，包括它修改过的组件、采用的样式方案以及设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会接着先前的工作继续执行，探索已有更改并提出添加浅色模式的方案。
3. 像审查高对比度模式一样，逐项查看新编辑，并使用 **Keep** 或 **Undo** 处理每个差异。
4. 如果代理采用了你不希望的方向，例如试图以你不喜欢的方式重构切换开关 UI，可以选择聊天输入框旁的 **Stop** 按钮停止任务，然后发送澄清后的后续消息。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以使用后续提示要求调整（“简化 CSS——使用 CSS 变量，而不是重复选择器”）、纠正方向（“不要添加第三个切换开关，将浅色模式整合到现有设置面板中”），或扩展范围（“还要为两个切换开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查代理修改的全部内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看已更改文件的完整列表。你应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开其中几个更改的文件并查看差异。确认代码遵循无障碍自定义代理中的实践，包括 ARIA 属性、键盘导航、语义化 HTML 和通过本地存储持久化设置。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和下一步

你使用 Copilot Chat Agent 模式监视并引导正在进行的会话，逐项审查文件编辑、接受或拒绝更改，并在不丢失上下文的情况下发送后续提示来扩展工作。

在本练习中，你了解了如何：

- 实时监视代理工作；
- 接受或拒绝单项文件编辑；
- 在会话进行期间通过后续提示引导代理。

在[下一课][next-lesson]中，你将把无障碍工作作为拉取请求发布，并审查本实验中本地自定义代理生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用 Agent 模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
