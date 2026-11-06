---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、说明、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置文件和测试都保存在同一分支。现在应一起审查这些更改并创建拉取请求。你之前手动合并了星级评分拉取请求（PR）；这次将让 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分到几个独立 PR 中。为简化本教程，这些筛选和质量工作流相关工作都保留在同一会话和分支，并放入同一个 PR。

本课将完成以下任务：

- 了解 Agent Merge 以及它如何自动处理合并生命周期。
- 检查完整的功能 PR 和验证证据。
- 审查后才授权 Agent Merge，并确认 PR 已合并。

## 场景

在筛选工作流中，你使用 Copilot 规划、实现并验证了功能。Tailspin Toys 现在希望自动处理剩余 PR 工作，同时让开发者掌控合并授权。

## 介绍 Agent Merge

**Agent Merge** 可在 GitHub Copilot app 中自动处理合并拉取请求所需的剩余工作。启用后，应用会话会读取 PR 并处理阻碍合并的事项，包括修复失败的持续集成（CI）检查、回复审查意见，以及在需要时执行 rebase；GitHub 允许后便会合并。它会在后台运行，应用重启后仍会继续，并在 PR 合并后自动关闭。

此前一直由你选择 **Merge pull request**。Agent Merge 可以代为处理，但它编辑代码和合并的权限仍需要你明确授权。授予合并权限前，请先审查它允许执行的操作和已有工作。

## 使用 Agent Merge 管理 PR

代码已全部完成并审查，现在让 Agent Merge 管理 PR 流程。

1. 在智能体选择器中选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会显示为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮，启动流程。仅选择下拉列表中的选项并不会启动该功能。

Agent Merge 流程会开始运行，并执行以下操作：

- 创建包含标题和描述的拉取请求。
- 如果会话从问题启动，则在描述正文中引用相关问题。
- 对目标分支执行 rebase 或处理可能的合并冲突。
- 监控 CI 流程，确保所有检查通过。
- 监控其他开发者或 Copilot 代码审查对 PR 的反馈，并进行更新以解决意见。
- 可以选择在所有检查成功后自动合并 PR。

现在让 Agent Merge 在所有检查通过后合并 PR。

5. 授予合并权限前，审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求智能体合并：

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 跟进智能体进度并处理报告的阻碍因素。继续之前，确认 PR 状态为 **Merged**；仅启用合并权限或智能体仍在运行，不代表合并已完成。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护或缺失的权限。继续前请先解决这些阻碍因素。

## 总结和后续步骤

你已自动化开发流程中的多个部分，包括生成、测试和验证代码，以及处理拉取请求。本课中，你：

- 了解了 Agent Merge 以及它如何自动处理合并生命周期。
- 检查了完整的功能 PR 和验证证据。
- 在审查后才授权 Agent Merge，并确认 PR 已合并。

接下来，你将[使用现有画布并创建分类画布][next-lesson]，体验与智能体一起检查、规划和可视化工作的更多方式。

## 资源

- [使用 GitHub Copilot app 管理问题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
