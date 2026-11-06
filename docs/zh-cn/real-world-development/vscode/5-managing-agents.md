---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

在智能体模式中，GitHub Copilot 会自主探索代码库、提出更改、编辑文件并运行命令。由于 Copilot Chat 在 VS Code 中实时运行，你可以查看每次工具调用和文件编辑。还可以在编辑器中检查每个差异、接受或拒绝单项更改，并通过后续提示指导对话，改进或扩展工作。

本练习将完成以下任务：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中指导智能体，同时添加浅色模式切换和高对比度模式。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队提出了额外需求：为觉得默认主题太暗的用户添加浅色模式切换。无需从头开始新对话，你将指导现有会话扩展工作范围。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和建议的编辑。每项文件编辑都会以内联方式显示差异和 **Keep** / **Undo** 控件。

1. 返回练习 4 使用的 Copilot Chat 视图。
2. 智能体工作时，在聊天中查看各项工具调用。你会看到文件读取、文件编辑以及需要运行的终端命令。
3. 智能体建议文件编辑时，差异会以内联方式显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看该会话中智能体修改的所有文件。

> [!NOTE]
> 如果工具调用需要权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你确认安全的命令。

5. 等待智能体完成高对比度模式。它会显示总结所做更改的最终消息。

## 指导智能体添加浅色模式

高对比度模式已完成，现在在同一对话中添加浅色模式切换。由于继续使用同一聊天会话，智能体会保留之前工作的上下文，包括涉及的组件、样式方法和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续工作，探索已有更改并提出浅色模式实现方案。
3. 像审查高对比度更改一样，逐项检查新增编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体的方向不符合预期（例如尝试重构切换 UI），可以选择聊天输入框旁的 **Stop** 按钮停止任务，然后发送澄清提示。

> [!TIP]
> 在正在进行的会话中提供指导，比重新开始更快。可以用后续提示提出调整（例如“简化 CSS，改用 CSS 变量，不要重复选择器”）、纠正方向（例如“不要添加第三个切换项，把浅色模式整合到现有设置面板”）或扩展范围（例如“为两个切换项都添加键盘快捷键”）。

## 审查工作集

提交前快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看已更改文件的完整列表。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开一些更改的文件并检查差异。确认自定义智能体要求的无障碍模式已经体现在代码中，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课将发布这些更改。

## 总结和后续步骤

你已使用 Copilot Chat 智能体模式监控并指导正在进行的会话：逐项审查文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

本课中，你：

- 实时监控智能体工作。
- 接受或拒绝单项文件编辑。
- 使用后续提示指导正在进行的会话。

在[下一课][next-lesson]中，你将把无障碍工作发布为拉取请求，并审查本地自定义智能体在本实验中完成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
