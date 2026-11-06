---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云端智能体执行了三项不同任务，用于改进用户体验和添加功能。云端智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云端智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制台中，可以查看所有包含未关闭拉取请求（PR）的智能体任务、查看它执行的操作，甚至在会话进行时提供指导。

本课将完成以下任务：

- 查看智能体页面，监控云端智能体任务。
- 在会话进行中提供指导，请求添加功能。

## 场景

分配高对比度模式任务后，团队意识到也可以同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此加入此功能也很合理。现在，你将指导智能体同时添加浅色模式和高对比度模式。

## 查看 Copilot 云端智能体任务

查看当前分配给 Copilot 云端智能体的所有任务的状态。

1. 转到 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到已分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择正在运行的任务之一，查看 Copilot 已执行的步骤，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成任务所需的操作。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的工作状态和执行方案而变化。

4. 还可以查看右侧显示的拉取请求（PR）面板，监控 PR 和更改的文件。

## 指导云端智能体

现在你已查看当前任务，请求 Copilot 在实现高对比度模式时也添加浅色模式。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前进度而异。

    ![任务控制台中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 监控会话几分钟，直到设置完成并开始执行任务。此时应会看到类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot 智能体页面中的任务截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示并将其加入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务。它会先完成发送提示时正在执行的步骤，然后将新要求加入工作流程。

这可能需要几分钟，可以趁此时间回顾已学和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和指导 GitHub Copilot 云端智能体任务的中心位置。你可以跟踪所有进行中和已完成的任务、审查智能体执行的工作，也可以重定向正在运行的任务，调整范围或补充指导。

本课探讨了以下内容：

- 使用任务控制台和智能体页面监控云端智能体任务。
- 重定向正在运行的会话，请求添加功能。

Copilot 正在完成功能工作。接下来，[迭代审查 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
