---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本教程中，你已与 GitHub Copilot 一起完成多项任务，以改进用户体验并添加功能：让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。现在查看代码更改；必要时向 Copilot 提供反馈，进一步改进其工作。

### 场景

正如前面多次强调的，软件设计和 DevOps 的基本原则不会因生成式 AI 而改变。始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来，在为团队其他成员启用审查前，先检查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能所做的更改。

## 安全和 GitHub Copilot 云端智能体

Copilot 云端智能体会异步且无人监督地执行任务，因此设置了一些安全限制：

- Copilot 对存储库只有读取权限，对自身工作分支才有写入权限。
- 云端智能体在 GitHub Actions 中运行，并在独立、短暂的环境中工作。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- [默认情况下，外部资源访问受限](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云端智能体创建的第一个拉取请求（PR），其中包含代码文档。通过 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器调用。如有需要，可以[自定义或禁用 Copilot 云端智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在执行任务，PR 标题会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。可能需要几分钟，可以趁此休息一下或回顾已学内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新代码中的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    查看代码时，留意 TSDoc 文档注释、TypeScript 约定和文件头注释。这些要求来自练习 2 开始时查看的自定义说明文件。

6. 审查完毕并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入 PR 检查队列。顺利的话，针对这个单体 Astro 应用的项目检查将全部通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作处理拉取请求不是单向的。你可以像标记团队成员一样，在 PR 评论或代码行内评论中标记 Copilot。它会看到评论，并启动新的会话来处理请求。由于结果具有非确定性，无法提供适用于所有情况的固定提示。可以要求 Copilot：

- 在每个代码文件顶部添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论并像标记其他用户一样标记 **@copilot**，请求它修改生成的文档。可以使用上面任意建议，或提出其他希望添加到代码库的文档要求。
2. 选择 **View Session**，观察 Copilot 执行工作。它会启动新会话来进行更新。
3. 选择 **Back to pull request** 返回 PR。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。准备就绪后，可将 PR 从草稿改为可审查状态，再合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏”问题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查完毕并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会进入 PR 检查队列。顺利的话，针对这个单体 Astro 应用的项目检查将全部通过。此过程可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到该分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能。和之前一样，可以继续迭代，让云端智能体进行调整。例如，可以要求 Copilot 修改显示的相关游戏数量，或添加文件头注释和 TSDoc 文档注释（请记住，分配此任务时你还没有更新自定义说明）。和之前一样，可在 **Conversation** 选项卡中添加评论，Copilot 会看到并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制台请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在执行任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成工作，可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现，重点检查：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查完毕并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入 PR 检查队列。顺利的话，针对这个单体 Astro 应用的项目检查将全部通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认页面重新加载后仍能保留设置。

注意，无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可像处理其他 PR 一样，在评论中标记 **@copilot** 请求更新。

## 可选练习 — 继续委派任务

云端智能体适合处理实际待办任务，让你能同时处理其他事情。可以在自己的存储库中创建更多问题并分配给 Copilot，逐步培养这种工作方式。例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云端智能体训练流程。本教程中，你：

- **查看了存储库自带的自定义说明文件**，以便之后观察它们对云端智能体输出的影响。
- **将问题分配给 Copilot 云端智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**实现无障碍功能，添加高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为任务控制台**，监控并指导无障碍会话。
- **审查并迭代云端智能体创建的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云端智能体训练流程。其他训练流程可以帮助你继续实践：

- 🖥️ **[VS Code 训练流程](../../vscode/)** — 在 IDE 中体验 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 训练流程](../../cli/)** — 在终端中交付两个经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云端智能体桥接。

在自己的存储库中，还可以尝试以下后续任务：

- 将重构或测试覆盖率问题分配给云端智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot 云端智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云端智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
