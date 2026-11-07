---
title: "练习 4 - 自定义智能体"
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

返回 Codespace，打开终端，并从 `main` 创建新分支，用于无障碍功能工作（筛选 PR 将与此工作保持分离）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览并审查后续部分，了解其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见错误和问题。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，该聊天会话会使用其指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动全新对话。模式和模型因练习而异；每个步骤会在相关位置说明。

1. 如果 Chat 视图的智能体下拉菜单中尚未选择 **Agent**，请选择它。

   ![Chat 视图中的智能体选择器示例。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单。该菜单显示当前使用的智能体；默认情况下为 **default**。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并修改项目文件。每项编辑都会以内联方式显示在聊天中，并包含文件路径和可审查的差异。
6. 随着编辑进行，**Files changed** 指示器会更新，显示智能体已修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际更改存储库，例如更新 Astro 组件、CSS 和相关测试。下一课会检查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 中的自定义智能体][custom-agents]，也就是专用于特定任务和领域的 AI 助手。使用自定义智能体可以将团队专业知识和标准整理为可复用的智能体，引导 Copilot 更有效地完成工作。

本课中，你：

- 了解了自定义智能体的定义方式。
- 在 Copilot Chat 智能体模式中使用了自定义智能体。

接下来[监控并引导智能体的工作][next-lesson]，审查实时更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
