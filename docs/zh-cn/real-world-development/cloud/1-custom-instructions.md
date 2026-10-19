---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须按特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能获取这些上下文。[指令文件][instruction-files]可以提供指导，让 Copilot 不仅了解要完成的任务，还了解应如何完成。

在本练习中，你将：

- 了解如何通过存储库自定义指令和按路径限定范围的指令文件，向 Copilot 提供项目专属上下文、编码指南和文档规范。
- 在 `.github/copilot-instructions.md` 中添加一项新的全局存储库规范。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，此处不会运行提示的“前后对比”。Copilot cloud agent 会异步处理 GitHub issue，因此难以实时并排展示影响。稍后审查 cloud agent 创建的拉取请求时，你会看到指令文件的作用。

## 指令文件

### 场景

Tailspin Toys 和其他优秀的开发团队一样，有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式，并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加注释块，说明文件的用途。

通过使用指令文件，你可以确保 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。这项强大的功能可以引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码约定、库，甚至喜欢添加的注释类型。你可以为整个存储库创建指令，也可以为特定类型的文件创建任务级上下文。

指令文件有两种类型：

- `.github/copilot-instructions.md` 是一个指令文件，会发送给 Copilot，用于该存储库的**每一条**请求。此文件应包含项目级信息，即大多数发送给 Copilot Chat 或 CLI 的请求都会用到的上下文，例如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md` 文件可用于特定任务或文件类型。你可以用它们为特定语言（如 TypeScript 或 Astro）或任务（如创建 UI 组件或一组单元测试）提供指南。

> [!NOTE]
> 在集成开发环境 (IDE) 中工作时，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一处编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

全面讨论如何创建指令文件超出了本工作坊的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的内容集中在项目级指南上，例如项目简介、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为文件类型（单元测试、Astro 组件、数据层）或特定任务提供具体指令。
- 使用自然语言，确保指南清晰，并说明代码应当和不应当采用的形式。

创建指令文件没有唯一的正确方式，正如使用 AI 也没有唯一的正确方式。你可以通过试验找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件。在查看本项目的指令文件时，你可能会发现其中包括多种任务的文件，例如[用户界面 (UI) 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助你生成指令文件。不同使用环境的操作方式各不相同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；当前环境对应的课程会在相关步骤中说明。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中收集了指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目中的自定义指令文件

花点时间阅读此存储库附带的指令文件：其中有一个核心文件 `copilot-instructions.md`，以及一组用于不同任务的 `*.instructions.md` 文件。请在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。特别注意 **Code standards** 下的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。留意 Astro 文件、Drizzle 数据层、测试等指令文件。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；该字段设置相对于存储库根目录的 glob，用于确定指令适用的文件。此处，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会符合条件。
5. 留意此项目针对单元测试创建方式的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件末尾。留意它指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分为较小的可复用文件，并为 Copilot 提供生成代码时可参考的示例。（其中的路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来，你将添加规则，要求使用 TSDoc 文档注释和文件注释标头。

## 添加新的存储库规范

接下来是本练习中唯一需要编辑的内容：添加一条项目级规则，要求通过 TSDoc 文档注释和文件级注释标头记录代码。稍后在本环境中分配 issue 时，cloud agent 会采用这条规则。

编辑前，先创建并切换到一个分支（cloud agent 会从 issue 指向的分支读取指令；此处仍会先提交更改，以保持整洁的工作流程）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在将编码标准记录在文件中，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。留意此处记录了项目编码标准，但尚未说明代码文档要求，因此生成的辅助函数没有文档注释。
3. 在现有规范下方添加以下 Markdown 行，要求 Copilot 添加文件注释标头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指令，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务创建指令。存储库级文件适合放置项目级标准，例如刚刚添加的文档注释规则。

## 提交、推送并合并指令更新

Cloud agent 会从 issue 目标分支读取指令。下一练习中将 issue 分配给 Copilot 时，Copilot 会从 `main` 创建分支，因此必须先将指令更改合并到 `main`，cloud agent 才能使用它们。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。下一练习中处理分配的 issue 时，cloud agent 就会读取这些指令。

> [!TIP]
> 如果想在本工作坊中直接使用 `main`，可以跳过分支并直接提交到 `main`。创建分支是为了让工作坊流程更接近实际项目。

## 总结和后续步骤

你了解了 Copilot 如何从本项目的指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了一项新的全局存储库规范。稍后，你会在 cloud agent 创建的拉取请求中看到这项规范的作用。

接下来，将[第一个 issue 分配给 Copilot cloud agent][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
