---
title: "练习 1 - 自定义说明 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 应了解某些背景信息，就需要确保它能获取这些上下文。向 Copilot 提供代码生成指导的方法有很多，其中最重要的是[说明文件][instruction-files]。

本练习将学习如何：

- 通过存储库自定义说明和按路径限定的说明文件，将项目上下文、编码指南和文档标准提供给 Copilot。
- 在当前说明生效时，生成筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不完全遵循设定的标准，因为 Copilot 具有非确定性。本练习旨在观察更新说明后行为的*整体变化趋势*，而非要求每次输出逐字相同。

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
> Copilot 也可以帮助生成说明文件。不同界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`；本练习会在相关步骤说明具体操作。
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

## 为更改创建分支

遵循最佳实践，为更改创建分支。

1. 从上一练习返回 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) 打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动全新对话。模式和模型因练习而异；每个步骤会在相关位置说明。

为了观察自定义说明的影响，先在当前说明文件生效时发送提示。稍后更新说明后，再次发送相同提示并比较结果。

1. 关闭之前练习中打开的编辑器选项卡，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器示例。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并修改代码，通常会同时更新辅助函数文件和测试。
6. 注意生成的辅助函数是类型化函数，以 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有明确要求，也可能添加文档注释。如果发生这种情况也没关系；本练习重点仍是观察添加说明后输出是否更一致。

## 添加新的存储库标准

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

## 重新发送提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除当前对话。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 注意生成的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商的 ID 和名称列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于真实代码。下一练习会基于这些代码继续构建。

## 提交说明并推送分支

说明文件与存储库中的其他资源一样，使用相同的源代码管理流程。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) 打开新终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新，以及 Copilot 为辅助函数基础工作修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他支持性更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求并关联待办事项中的文档标准议题。Copilot 可以使用已连接到项目的 GitHub 工具查找议题、创建并合并 PR。

> [!NOTE]
> 这是首次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用这些工具时，VS Code 可能会提示登录 GitHub。按照提示完成登录。后续练习会介绍此连接的工作原理，即 Model Context Protocol。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) 打开 Copilot Chat 视图，并确认智能体选择器中已选中 **Agent**。
3. 要求 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，说明其中包含说明更新和新的发行商辅助函数基础：

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，请检查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR 并将本地分支切回 `main`：

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求代表你执行 GitHub 操作和 shell 命令时，请检查命令并酌情选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并了拉取请求。

## 总结和后续步骤

你已了解 Copilot 如何从项目说明文件中获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察现有说明产生的结果。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次发送相同提示，观察生成的代码如何采用新标准。
- 将说明更新和辅助函数基础工作提交到 `main`，供下一练习继续使用。

接下来，在[智能体模式][next-lesson]中使用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
