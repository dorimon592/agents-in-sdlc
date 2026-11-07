---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

[GitHub Copilot 中的自定义智能体][custom-agents-concept]让你可以创建专用于开发工作流中特定任务或领域的 AI 助手。通过在存储库的 `.github/agents` 文件夹中添加 Markdown 文件，可以为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定工作。团队可以将专业知识整理成可复用的智能体，例如执行 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后面是用于定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体和智能体技能的区别

自定义智能体与[智能体技能][agent-skills-concept]有些相似。两者主要通过 Markdown 文件定义，并告知 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是工作者，**技能**是工具。

自定义智能体拥有自己的上下文窗口，能够协调技能（甚至其他智能体）完成工作。本实验中的无障碍自定义智能体会根据无障碍指南审查并更新网站；它可以在工作过程中调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的正确方法。与其他 AI 功能一样，应通过测试和迭代，找到适合环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将学习如何：

- 了解自定义智能体的定义方式。
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，由于文字和背景颜色的对比度不足，有些用户难以阅读当前的深色主题。为解决无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍性至关重要，因此需要尽快实现此功能。你将使用自定义智能体生成该功能。

## 检查无障碍自定义智能体

项目已提供无障碍自定义智能体。先检查内容，了解它将如何引导 Copilot。

返回 Codespace，并检查无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览并审查后续部分，了解其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见错误和问题。

## 创建并分配议题

Mission control 是管理环境中所有智能体的中心位置。可以在这里将任务分配给 Copilot 云智能体、监控任务，也可以重定向并提供额外指导。先将实现高对比度模式的任务分配给 Copilot。

1. 打开自己的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 标题输入 `Add high contrast mode to website`。
6. 描述输入：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

   ![Copilot 云智能体分配界面中突出显示自定义智能体和 Accessibility agent](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 中的自定义智能体][custom-agents]，也就是专用于特定任务和领域的 AI 助手。使用自定义智能体可以将团队专业知识和标准整理为可复用的智能体，引导 Copilot 更有效地完成工作。

本课中，你：

- 了解了自定义智能体的定义方式。
- 将任务分配给了自定义智能体。

Copilot 正在实现高对比度模式。接下来可以在 mission control 中[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [在组织中使用自定义智能体前的准备工作][org-custom-agents]
- [在企业中使用自定义智能体前的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
