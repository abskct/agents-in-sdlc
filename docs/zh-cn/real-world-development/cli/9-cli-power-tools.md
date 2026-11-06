---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "了解用于管理上下文、选择模型、共享会话和可选委派到云端的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可用于访问高级功能、查看后台信息和配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令，接下来看看其他实用命令。

本课将完成以下任务：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 了解用于并行工作、工作树以及委派任务到云端智能体的可选命令。

## 场景

核心 CLI 工作流已完成。现在了解其他能力：管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云端智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会接近模型的上下文窗口上限。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令自行查看或压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及系统说明、工具、消息和可用空间的上下文分配情况。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话原本就很短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口逐渐占满时自动压缩内容。需要自行选择压缩时机时，可使用 `/compact`。切换到无关任务并希望使用全新对话时，可使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者的偏好也不同。Copilot CLI 支持列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型及其用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作并分享经验有助于提升大家使用 AI 工具的能力。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的选项。
2. 如需共享此会话，请选择合适的目标，例如 `/share file`，导出到本地 Markdown 文件。
3. 发送或发布导出内容前，请先审查。会话导出内容可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队无意公开的存储库或对话内容。

## 可选：扩展或委派工作

核心教程已完成。Copilot CLI 还为更大规模的任务提供以下命令：

- `/fleet` 可将相互独立的子任务分配给子智能体并行执行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot 云端智能体；它会异步工作，并可能创建拉取请求。

这些命令是可选的，因为它们可能创建额外工作树或远程工作。尝试前，请开始一个范围明确的新任务，并通过常规工作流审查结果。要进一步了解异步智能体工作，请继续学习[云端智能体教程][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以配置工具、共享会话并查看其内部运行状态。本课中，你：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 了解用于并行工作、工作树和委派到云端智能体的可选命令。

Copilot CLI 还有更多斜杠命令值得探索。接下来，[回顾已学内容][next-lesson]并了解后续学习方向。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云端智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
