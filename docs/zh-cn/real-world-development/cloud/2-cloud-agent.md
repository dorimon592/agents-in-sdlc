---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发人员专注于更宏观的工作。

本练习将使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要解决的技术债务。最初创建网站的承包商没有留下完善的文档，事实上，代码完全缺少文档。团队希望首先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验。他们希望在每款游戏的详情页显示相关游戏，即同一类别中的其他游戏，方便用户继续浏览。当前不需要完善的实现，只需要供团队进行用户体验验收测试。此项工作目前受阻，而且优先级低于其他议题。

这两项任务都可能长期被搁置，因此很适合分配给 Copilot 云智能体。它可以异步处理任务，让开发人员先处理其他工作，之后再回来审查 Copilot 的成果并确认符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。无论会话仍在运行还是已完成，都可以查看详情。建议方案准备好供审查时，Copilot 会在拉取请求中标记你。

## 明确界定说明范围的重要性

GitHub Copilot 并非魔法工具，不能替代思考。看似简单的操作也可能包含复杂细节，因此[应仔细界定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段工作、学习、尝试并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明以及它们如何引导 Copilot。如果已经完成自定义说明实践练习，就会看到 `.github/copilot-instructions.md` 的作用；如果尚未学习，现在可以快速查看。将工作分配给 Copilot 云智能体前，先只读查看存储库中已有的说明文件，以便稍后观察它们的影响。

在 GitHub 网页界面或已运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：检查 **Code standards** 部分，特别是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将说明限定到 `**/*.test.ts` 文件。

在下一节中，将标题为 `Code lacks documentation` 的议题分配给云智能体。查看生成的拉取请求，留意 TSDoc 文档注释、TypeScript 约定和文件注释头；这些都来自说明文件。后续练习审查 PR 时还会再次检查。

## 设置 Copilot 云智能体的开发环境

编写代码通常需要特定环境并运行一些设置脚本，以确保环境准备就绪。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师的方式工作。

云智能体使用 [GitHub Actions][github-actions] 执行任务。可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]自定义环境，让它在开始工作前运行，并访问所需开发工具和依赖项。实验已预先配置此工作流，以方便完成练习。它会为单体 Astro 应用安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及经过迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许从存储库的 “Actions” 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
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

该文件与其他 GitHub 工作流文件类似，但有几个关键点：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，以便在等待 Copilot 运行前先验证工作流是否成功。

## 添加文档

大家都知道文档很重要，但多数项目的文档要么已过时，要么完全缺失。这类技术债务经常无人处理，会拖慢效率，使代码库更难维护，也让新开发人员更难加入项目。Copilot 擅长创建文档，因此这是分配给 Copilot 云智能体的理想任务。它会在后台生成必要文档，之后可在后续练习中审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 标题输入 `Code lacks documentation`。
6. 描述输入：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 任务分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，标题可能类似 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定合适的处理方案。

13. 查看列表和它计划完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并选择合适计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们有时间处理其他工作。现在利用这一特性，再分配一项任务给云智能体，同时开始为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分任务，让你处理一部分工作，而它负责另一部分。为设计团队添加相关游戏部分或许不需要太多时间，但也可以把这段时间用于其他工作。现在将任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 标题输入 `Show related games on the game details page`。
6. 描述输入：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，议题的第一条评论旁会出现 👀，表示 Copilot 已开始处理。

![Copilot 用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。Copilot 云智能体的工作方式类似软件工程师，因此无需一直主动监控；等它完成后再审查即可。现在可以开始创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理。可以让 Copilot 解决技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

本课中，你：

- 自定义了代码生成环境。
- 了解了如何确保安全地执行操作。
- 了解了明确界定议题范围的重要性。
- 将议题分配给了 Copilot。

云智能体正在后台处理任务，现在可以开始创建并使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可使用之前课程中介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
