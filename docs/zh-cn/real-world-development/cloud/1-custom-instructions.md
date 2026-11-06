---
title: "练习 1 - 自定义说明（云端智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云端智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 应了解某些背景信息，就应确保它能够获取这些上下文。[说明文件][instruction-files]可以提供指导，让 Copilot 不仅了解你想做**什么**，还了解**如何**完成。

本练习将完成以下任务：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定的说明文件提供给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的全局存储库标准。

> [!NOTE]
> 与 VS Code 和 CLI 训练流程不同，本练习不会比较提示修改前后的结果。Copilot 云端智能体会在 GitHub 问题上异步工作，因此较难实时并排展示说明的影响。稍后审查云端智能体创建的拉取请求时，你会看到说明文件的作用。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件用途。

通过说明文件，可以确保 Copilot 获取正确上下文，并按照这些实践完成任务。

### 自定义说明

自定义说明可以提供上下文和偏好，帮助 Copilot 理解你的编码风格和要求。你可以指定编码约定、库以及偏好的注释类型。说明既可以适用于整个存储库，也可以针对特定文件类型或任务。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。文件应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对具体任务或文件类型创建的文件。你可以为 TypeScript、Astro 等语言提供指导，也可以为 UI 组件或单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云端智能体在生成代码时，都会使用存储库级说明文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他规范的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

关于创建说明文件的完整讨论超出了本教程的范围。不过，示例项目展示了常见做法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持说明清晰，并提供代码应如何编写或不应如何编写的示例。

说明文件没有唯一的正确写法，AI 工具的使用方式也不止一种。通过实践可以找出适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的说明文件。查看本项目说明时，你可能会发现有适用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的文件。
>
> Copilot 也可以帮你生成说明文件。不同产品的操作各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应教程会介绍相关操作。
>
> 需要模板或起点？请查看 [awesome-copilot][awesome-copilot]，其中包含说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

花些时间查看存储库自带的说明文件：一个核心 `copilot-instructions.md` 和一组用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介和 **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** 等部分。在 **Code standards** 下，注意其中的 **GitHub Actions Workflows** 子部分。这些说明适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹，查看其中的文件，包括 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意 `applyTo` 字段，它指定相对于存储库根目录的 glob。此处会匹配所有 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看该文件中针对本项目单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它指向其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件。这样可以拆分较大的说明集以便复用，并指向代码生成示例。（这些路径相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求为代码添加说明。接下来会添加 TSDoc 文档注释和文件头注释的规则。

## 添加新的存储库标准

接下来只需编辑一次：添加项目级规则，要求使用 TSDoc 文档注释，并添加文件级注释头。后续本教程中云端智能体处理问题时会遵循这项规则。

编辑前先创建分支（云端智能体会读取问题目标分支中的说明，但仍应以常规方式提交更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，约在第 27 行。注意其中记录了项目编码标准，但还没有代码文档规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准后添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 上一课介绍了说明文件可以放在存储库级 `.github/copilot-instructions.md` 中，提供全局指导；也可以放在 `*.instructions.md` 文件中，适用于特定语言、文件类型或任务。项目级标准（例如刚添加的文档注释规则）适合放在存储库级文件中。

## 提交、推送并合并说明更新

云端智能体会读取问题目标分支中的说明。下一课将把问题分配给 Copilot；它会从 `main` 创建分支，因此必须先将说明更改合并到 `main`，云端智能体才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后云端智能体处理问题时就会读取这些说明。

> [!TIP]
> 如果希望在本教程中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何获取本项目说明文件中的上下文，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续课程中，你会看到云端智能体创建的拉取请求如何遵循此标准。

接下来，[将第一个问题分配给 Copilot 云端智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
