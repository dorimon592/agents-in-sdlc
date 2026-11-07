---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 应了解某些背景信息，就需要确保它能获取这些上下文。[说明文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解要完成*什么*，还了解应*如何*完成。

本练习将学习如何：

- 通过存储库自定义说明和按路径限定的说明文件，将项目上下文、编码指南和文档标准提供给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习不同，本练习不会比较更新说明前后的提示效果。Copilot 云智能体会异步处理 GitHub 议题，因此较难实时并排展示差异。后续练习中审查云智能体创建的拉取请求时，可以看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 有一套开发指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应添加描述文件用途的注释块。

通过说明文件，可以确保 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。你可以指定编码规范、库，甚至希望添加的注释类型。说明文件可以应用于整个存储库，也可以针对特定文件类型提供任务级上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如所用技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型创建的说明文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

全面介绍如何创建说明文件超出了本研讨会的范围。示例项目展示了一种有代表性的方法：

- 让 `copilot-instructions.md` 专注于项目级指南，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供详细说明。
- 使用自然语言，保持说明清晰，并提供代码应如何编写或不应如何编写的示例。

创建说明文件没有唯一的方法，使用 AI 也是如此。通过尝试可以找出最适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的说明文件。浏览当前项目时，会发现它针对多种任务提供了说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`；相关练习会在适当位置说明具体操作。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

花些时间阅读存储库自带的说明文件：一个核心 `copilot-instructions.md`，以及一组适用于各种任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，了解项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下还有 **GitHub Actions Workflows** 指南。这些说明适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里包含 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段设置相对于存储库根目录的 glob，确定适用的文件；例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都适用。
5. 查看此项目针对单元测试的具体说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。查看指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并提供可参考的示例。这里的路径相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求在代码中添加文档注释。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来只需进行一项编辑：添加一条项目级规则，要求以 TSDoc 文档注释和文件注释头的形式记录代码。后续练习中，云智能体会在处理你分配的议题时使用此规则。

编辑前先创建分支。云智能体会从议题目标分支读取说明，因此仍需将更改合并到 `main`，但先在分支中提交更改：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码标准可以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到约在第 27 行的 **Code formatting requirements** 部分。该部分记录了项目编码标准，但尚未规定代码文档要求，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件可以放在存储库级 `.github/copilot-instructions.md` 中提供全局指南，也可以放在 `*.instructions.md` 中限定到特定语言、文件类型或任务。刚添加的文档注释规则属于项目级标准，因此存储库级文件是合适的位置。

## 提交、推送并合并说明更新

云智能体会从议题目标分支读取说明。下一练习中，将议题分配给 Copilot 时，它会从 `main` 创建分支，因此必须先将说明更改合并到 `main`，让云智能体能够使用这些规则。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并合并。下一练习中处理分配给云智能体的议题时，它就能读取这些说明。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，也可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目的处理方式。

## 总结和后续步骤

你已了解 Copilot 如何从项目说明文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。后续练习中，云智能体创建的拉取请求会体现该标准。

接下来将第一个议题[分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
