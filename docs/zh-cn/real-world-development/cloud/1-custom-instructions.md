---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]可为 Copilot 提供指导，让它了解你希望完成什么以及如何完成。

本练习将学习如何：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径应用的指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 开发环境不同，这里不会运行 *更改前/更改后* 的提示。Copilot cloud agent 会异步处理 GitHub 问题，因此难以实时并排展示指令的影响。稍后审查 cloud agent 创建的拉取请求时，你会看到指令文件的作用。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用暗色模式并具有现代风格。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明该文件的用途。

使用指令文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。你可以指定编码约定、库以及注释风格。可以为整个存储库创建指令，也可以针对特定文件类型或任务提供指引。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含适用于大多数 Copilot 聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指引。
- `.github/instructions/*.instructions.md`：可针对具体任务或文件类型创建的指令，例如 TypeScript、Astro、UI 组件或一组单元测试。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，[Copilot 支持其他规范的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

创建指令文件有很多内容，本练习无法一一介绍。示例项目展示了常见方法，核心原则包括：

- 将 `copilot-instructions.md` 的内容限定为项目级指导，例如项目结构、正在构建的内容和全局编码标准。
- 使用 `*.instructions.md` 文件编写针对特定文件类型（例如单元测试、Astro 组件或数据层）或任务的指引。
- 使用自然语言，让指导清晰易懂，并提供正反示例。

自定义指令没有唯一正确的写法，与使用 AI 的其他方面一样，需要通过尝试找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件。探索此项目中的文件时，你可能会看到针对多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助你生成指令文件。不同开发环境的操作方式各异，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。在相关课程中会介绍适用的操作方式。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

花些时间阅读此存储库附带的指令文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，了解项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。Copilot 与你交互时都会遵循这些指引。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了决定指令适用文件的 glob。例如，任何符合 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 查看此项目的单元测试专用指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看它指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并提供可参考的示例。这些路径相对于指令文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求添加代码内文档。下一步将添加 TSDoc 文档注释和文件头注释的规则。

## 添加新的存储库标准

接下来，你将进行本练习唯一的编辑：添加一条项目级规则，要求代码中的文档说明使用 TSDoc 文档注释和文件级注释。cloud agent 处理本开发环境后续分配的问题时，会遵循此规则。

编辑前，先创建一个分支（cloud agent 会从问题目标分支读取指令，但仍建议单独提交更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码标准可以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意该部分记录了项目编码标准，但尚无代码内文档要求，因此生成的 helper 没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，以要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件（`.github/copilot-instructions.md`）中编写全局指引，也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务编写指引。存储库级标准（例如文档注释规则）应放在存储库级文件中。

## 提交、推送并合并指令更新

cloud agent 会读取问题目标分支上的指令。下一练习中将问题分配给 Copilot 时，它会从 `main` 创建分支，因此必须先将指令更改合并到 `main`，cloud agent 才能读取。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。这样，在下一练习中分配问题时，cloud agent 就能读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 分支上完成本工作坊，可以跳过分支操作并直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何读取指令文件，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。接下来的练习中，你会看到 cloud agent 创建的拉取请求如何遵循该标准。

接下来，将[第一个问题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
