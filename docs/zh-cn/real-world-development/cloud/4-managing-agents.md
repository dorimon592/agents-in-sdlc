---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

前几课中，你已让 Copilot 云智能体处理了三个旨在改善用户体验和添加功能的任务。云智能体会异步自主工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有拥有未关闭拉取请求 (PR) 的智能体任务、探索已执行的操作，甚至引导进行中的会话。

本课将学习如何：

- 探索智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给智能体以实现高对比度模式后，团队意识到也可以顺便添加浅色模式。由于已经在更新网站样式并添加切换功能，将此功能一并纳入似乎很合理。现在引导智能体在实现高对比度模式的同时也添加浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码库文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择其中一个运行中的任务，查看 Copilot 已执行的工作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置 MCP 服务器。
   - 执行完成分配任务所需的其他步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 当前工作状态和执行方案而异。

4. 同时查看右侧显示的 PR 窗格。在此可以查看 PR 和已更改文件，以便进一步监控。

## 引导云智能体

现在查看当前活动的任务，并要求 Copilot 在实现高对比度模式时添加浅色模式切换开关。

1. 选择标题涉及添加高对比度模式的会话。确切标题取决于 Copilot 使用的名称和当前工作状态。

   ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 监控会话几分钟，直到 Copilot 完成设置并开始工作。出现类似下方的消息时，表示它已经开始处理任务。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的云智能体任务截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 观察 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。在完成收到消息时正在执行的具体步骤后，它会将新请求加入工作流程。

这可能需要几分钟，可以利用这段时间回顾到目前为止的学习内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。在此任务控制中心，可以跟踪所有活动和已完成的任务、审查正在进行的工作，甚至重定向正在运行的任务以调整范围或提供额外指导。

本课中，你：

- 使用 mission control 和智能体页面监控了云智能体任务。
- 重定向了进行中的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
