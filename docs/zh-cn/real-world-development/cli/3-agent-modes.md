---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一项小功能。较大的更改则需要更稳妥的流程。GitHub Copilot CLI 可以融入组织现有的开发流程，帮助团队以合适的方式构建正确的功能。本课将开始一组课程，带你体验典型的智能体开发流程：从议题生成新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 继续实现功能时，将使用同一对话和分支。实际工作中，通常会为不同类型的文件创建不同分支或 PR；此处为聚焦核心概念而简化流程。

本课将学习如何：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

后续课程还会更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的议题描述了该功能，但组合类别等细节需要先达成一致。先用 Plan 模式确定这些决策，再授权 Autopilot 在明确范围内实现功能。

## 背景

将 AI 编码智能体引入开发流程，并不会改变基本原则。事实上，这些原则变得更加重要。大多数开发人员的工作流程大致如下：

1. 从已提交的议题开始，了解待完成的工作。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循标准流程，可以确保 AI 生成的代码符合需求，并经过与手动编写代码相同的审查流程。

## 对话模式

**对话模式**决定智能体可以自主执行多少工作。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可循环切换模式：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并不断迭代，直到认为任务已完成。

先进入 Plan 模式并审查计划，然后使用 Autopilot 实现计划。

## 从议题开始

开始筛选功能之前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重启。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux)。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，并返回 **Session** 选项卡。

现在可以看到提示以 `#7`（或类似编号）开头。`#` 可将 GitHub 议题或拉取请求 (PR) 加入上下文。

## 规划筛选功能

规划让你有机会在交给 Copilot 实现功能或执行任务前确定方案。对于复杂工作，花些时间规划通常很有帮助。切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定合适的实现方案，逐步制定计划。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有唯一正确的答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) 展开计划。
5. 上下滚动审查计划。
6. 如果计划中的任何内容与你的决定不符，请要求 Copilot 修改。

## 批准 Autopilot

计划已编写并审查完毕，现在可以实现功能。使用 Autopilot 让 Copilot 开始工作。

Autopilot 会持续处理问题，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何根据计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，应像其他代码一样先审查再合并。审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 对照议题和规划时做出的决定检查结果。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目检查的输出，并要求 Copilot 解决任何失败项。

## 体验新功能

代码看起来没问题，但实际运行情况如何？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别、发行商筛选，以及按计划确定的组合方式筛选。
4. 确认重置和无结果时的行为符合议题和既定决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

本课使用不同的对话模式构建并审查功能。你已经：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中确定了需求。
- 使用 Autopilot 模式实现新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，深入了解如何让代码遵循记录的规范：[使用自定义说明][next-lesson]。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
