---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发人员通常需要找到所有相关文件、进行更改、运行测试、调试、发现遗漏的文件并再次修改，整个流程会不断重复。

Copilot 智能体模式可以帮助处理这些工作。

Copilot 智能体模式能够在 IDE 中更自主地工作。它的工作方式类似开发人员：先探索项目结构，执行必要的更新，运行测试等任务，并自动修复发现的问题。接下来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体名称相似，但适用于不同的体验。智能体模式在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队中的同事一样异步工作，通过议题和拉取请求与你协作。

本练习将学习如何：

- 使用 Copilot 智能体模式探索项目、查找相关文件并协调更改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在更改合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断扩大，你希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成数据层、UI 和测试的剩余工作。

## 运行 Tailspin Toys 网站

进行更改前，先探索 Tailspin Toys 网站，了解它目前的功能。

该网站是一个带有开发者主题的桌游众筹平台，可列出游戏并显示其详情。网站是一个单独的 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有独立后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能通过使用标准 DOM API 的小型、限定范围的 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应已位于 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux)，在 Codespace 中打开新终端。
3. 返回 `main` 并创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示，询问是否在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd> (Mac) 或 <kbd>Ctrl</kbd> (Windows/Linux)，然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有通道，网站服务器正在其中运行。

### 浏览网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示游戏列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、描述、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动全新对话。模式和模型因练习而异；每个步骤会在相关位置说明。

网站的初始实现可以正常运行，但我们希望添加新功能。先查看待办事项。通过模板设置项目时，系统已自动创建议题待办事项；让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。Agent 智能体可以自主规划并修改多个文件、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器示例。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此将其列为步骤，以便尽量保持建议一致并提供良好体验。不过，由于 LLM 具有概率性，收到的建议可能与示例不同，这是正常且预期的情况。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还会根据它认为对用户最有用的程度为项目排序。

## 审查说明文件

让智能体生成代码前，先审查将用于向 Copilot 提供上下文的说明文件。此处会使用[用户界面 (UI) 说明][ui-instructions]，了解如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能的整体指南，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向多种文件类型说明的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目中的文件。路径相对于说明文件所在位置，因此可以复用说明、将复杂内容拆分为更易管理的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行以下三项更新：

- 添加或完善数据层 (`src/lib/`) 中剩余的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，测试必须通过后才能将更改合并到代码库。Copilot 智能体模式可以执行这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例。](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单仍选中 **Agent**。

   ![Chat 视图中的智能体选择器示例。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，要求 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。它应该会找到数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件。](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，可能需要几分钟。过程中，代码编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会提供 **Keep** 或 **Undo** 选项，因此不必在它工作过程中选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中的对话框，询问用户是否允许运行测试。](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 可能会暂停一段时间，甚至在迭代过程中出现测试失败。这没有关系。Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未检测到错误。

   ![包含完整 Copilot 智能体模式会话的 Chat 窗口。](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，留意潜在问题。

> [!CAUTION]
> 请始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你已了解如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 使用 Copilot 智能体模式探索项目、查找相关文件并协调更改。
- 在更改合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分探索练习 - 实现分页

游戏列表不断增大时，需要支持分页。运用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 决定分页的实现方式，例如是否允许用户选择页面大小，还是使用固定页面大小。
- 编写提示时提供足够指导，让 Copilot 按预期实现功能。
- 你可能需要与 GitHub Copilot 反复协作，要求更改并补充上下文；这是使用 Copilot 的正常工作方式。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：用途及使用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
