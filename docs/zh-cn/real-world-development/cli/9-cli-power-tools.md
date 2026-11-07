---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索管理上下文、选择模型、共享会话和可选委派到云智能体的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可以访问高级功能、查看后台信息并配置其他选项。此前已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。接下来探索一些其他实用命令。

本课将学习如何：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派到云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。现在了解一些其他功能：管理上下文、切换模型、共享会话，以及选择是否将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会达到模型上下文窗口的上限。Copilot CLI 会在需要时自动压缩对话，也可以使用斜杠命令检查或手动压缩。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及上下文在系统说明、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能并不明显。

> [!NOTE]
> 上下文窗口接近填满时，Copilot CLI 会自动压缩上下文。想自行决定压缩时机时，可使用 `/compact`。如果要切换到无关任务并开始全新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发人员也有不同偏好。Copilot CLI 支持列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作并分享经验有助于提升所有人使用 AI 工具的能力。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的选项。
2. 如果要共享此会话，请选择合适的目标，例如 `/share file`，将会话导出为本地 Markdown 文件。
3. 发送给他人或发布前，请检查导出的内容。会话导出可能包含提示、响应和项目详情。

发布链接或 gist 是可选的。不要发布团队无意共享的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心研讨会已完成。Copilot CLI 还为大型任务提供了以下命令：

- `/fleet` 可以将独立子任务分配给多个子智能体并行运行。
- `/worktree` 可以为单独任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给 Copilot 云智能体，由其异步处理并可能创建拉取请求。

这些命令可能会创建其他工作树或远程任务，因此均为可选。尝试前，请开始一项新的、范围明确的任务，并按照常规工作流审查结果。若要深入了解异步智能体工作，可继续学习[云智能体研讨会][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以配置工具、共享会话并了解后台运行情况。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览了可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行工作、工作树和委派到云智能体的可选命令。

Copilot CLI 还有更多斜杠命令和功能可供探索。现在，[回顾已学内容][next-lesson]并了解后续学习步骤。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
