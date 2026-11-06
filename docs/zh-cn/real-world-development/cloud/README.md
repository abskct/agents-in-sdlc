---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云端智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云端智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步运行 GitHub Copilot。你在 GitHub 上分配任务后，云端智能体会在后台接手工作，探索存储库、进行更改并创建拉取请求；与此同时，你可以处理其他事情。

通过这些练习，你将添加云端智能体会遵循的自定义说明，然后分配 GitHub 问题并让它实现任务。你还将审查并使用自定义智能体来塑造其工作方式，在智能体控制面板中监控并指导会话，最后审查它创建的拉取请求并迭代结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加云端智能体会遵循的自定义说明 |
| [2. 云端智能体][ex2] | 异步智能体 | 将问题分配给 Copilot 云端智能体 |
| [3. 自定义智能体][ex3] | 专用智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并指导智能体会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本教程前，请确认你具备以下条件：

- [ ] 拥有启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 账户
- [ ] 已为你的账户或组织启用 Copilot 云端智能体

> [!TIP]
> 没有付费计划？通过验证的学生可通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本教程使用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可完成所有训练流程。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot 云端智能体适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，不适用于 Copilot Free。对于 Copilot Business 或 Enterprise，管理员必须为组织启用此功能。

> [!NOTE]
> 本教程不涉及 MCP。要了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 训练流程](../cli/)或 [VS Code 训练流程](../vscode/)。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
