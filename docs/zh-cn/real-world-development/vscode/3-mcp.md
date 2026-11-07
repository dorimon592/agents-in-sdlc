---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应先确认它能在浏览器中正常运行。本练习将连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器并测试功能，而不必手动操作应用，然后发布分支并创建 PR。

本练习将学习如何：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式能够访问编辑器以外的工具时，会变得更加强大。Copilot 通过 Model Context Protocol (MCP) 实现这一点。MCP 是智能体与外部工具和服务通信的标准方式。

![智能体模式内部结构以及它如何与上下文、LLM 和工具交互的示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体与外部工具和服务通信提供了一种方式。借助 MCP，AI 智能体可以实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务（例如现有 API 或 npm 包等本地工具）之间的桥梁。每个 MCP 服务器都提供一组智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 智能体可以创建存储库、更新现有存储库并管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让 AI 智能体可以访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可提供不同的工具和资源。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态系统。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前应仔细审查其源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 文件中应已配置 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

`github` 条目由项目模板提供，因此上一练习中 Copilot 能读取待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，Copilot 首次使用其工具时需要使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，可用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件如下所示：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 参数要求 Playwright 在后台运行浏览器，不显示可视窗口；Codespace 没有可显示窗口的桌面环境，因此必须使用该参数。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装 Playwright 所需的浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需启动** MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步 Copilot 首次需要工具时启动服务器，并询问是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。智能体可以打开页面、导航、应用筛选并读取结果，然后向你总结观察结果，无需离开对话。这是快速确认功能行为是否符合预期的方式。

在底层，Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，智能体会像辅助技术一样，根据带标签的结构化元素（按钮、链接和列表项）进行推理；一次快速功能检查也可以顺便验证基本无障碍性。

连接服务器并启动应用后，要求 Copilot 测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、执行这些步骤并报告结果。根据议题中的验收标准审查其总结。如果发现问题，请提出后续问题，或在创建 PR 前要求它修复代码。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请先重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 所做更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面 (CLI) 创建拉取请求。也可以让 Copilot 创建 PR，它可以查找相关议题并将其关联到 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认智能体下拉菜单中已选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 要求 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，系统可能会要求使用 GitHub 进行身份验证，请按照提示操作。
5. 观察 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 使用该服务器在发布前通过浏览器手动测试了筛选功能。
- 发布了分支，并为筛选工作创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来[创建自定义智能体][next-lesson]，简化无障碍审查等专注任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
