---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须按特定方式完成，就应确保 Copilot 能获取相关指导。[指令文件][instruction-files]是提供代码生成指南的重要方式。

在本练习中，你将：

- 了解如何通过存储库自定义指令和按路径限定范围的指令文件，向 Copilot 提供项目专属上下文、编码指南和文档规范。
- 使用当前指令生成用于筛选功能的第一个数据切片（publisher 辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加一项新的全局存储库规范。
- 再次发送相同提示，观察生成的代码如何采用新规范。
- 将指令更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指南。

> [!CAUTION]
> 生成的代码可能不完全遵循你设置的标准。Copilot 的结果具有非确定性。本练习的目的是观察更新指令后行为的*趋势*变化，而非要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 和其他优秀的开发团队一样，有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面 (UI) 应采用暗色模式，并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加注释块，说明文件的用途。

通过使用指令文件，你可以确保 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。你可以指定偏好的编码约定、库，甚至希望添加的注释类型。你可以为整个存储库创建指令，也可以为特定类型的文件创建任务级上下文。

指令文件有两种类型：

- `.github/copilot-instructions.md` 是一个指令文件，会发送给 Copilot，用于该存储库的**每一条**请求。此文件应包含项目级信息，即大多数发送给 Copilot 的请求都会用到的上下文，例如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md` 文件可用于特定语言、文件类型或任务，例如创建 UI 组件或一组单元测试。

> [!NOTE]
> 在集成开发环境 (IDE) 中工作时，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一处编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本工作坊的范围，不过示例项目展示了一种有代表性的方法：

- 将 `copilot-instructions.md` 中的内容集中在项目级指南上，例如项目简介、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为文件类型（单元测试、Astro 组件、数据层）或特定任务提供具体指令。
- 使用自然语言，保持指南清晰，并为代码应当和不应当采用的形式提供示例。

创建指令文件没有唯一的正确方式，正如使用 AI 也没有唯一的正确方式。你可以通过试验找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件。在查看本项目的指令文件时，你可能会发现其中包括多种任务的文件，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助你生成指令文件。不同使用环境的操作方式各不相同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；当前环境对应的课程会在相关步骤中说明。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中收集了指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目中的自定义指令文件

先查看入门项目中已有的指令文件，了解修改前的效果。

1. 返回上一练习创建的 codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指南。这些指令适用于你在此存储库中与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览文件。留意 Astro 文件、Drizzle 数据层、测试等指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；该字段设置一个 glob，用于确定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意它指向其他指令文件和项目现有文件的链接。这可以将较大的指令集拆分为较小的可复用文件，并为 Copilot 提供生成代码时可参考的示例。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来，你将添加 TSDoc 文档注释和文件注释标头规则。

## 为更改创建分支

先按照最佳实践为更改创建分支。

1. 返回上一练习创建的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。不同练习的模式和模型选择可能不同；相关步骤会在需要时说明。

先在当前指令文件生效的情况下发送提示。之后更新指令，再次发送同样的提示，观察差异。

1. 关闭之前练习中打开的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会涉及辅助函数文件及其测试。
6. 留意建议的辅助函数是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回 publisher 类型数组。这来自 `.github/instructions/drizzle.instructions.md` 中针对数据层的约定，该文件适用于 `src/lib/*.ts`。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释标头。

> [!CAUTION]
> Copilot 使用概率模型，因此即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系；重点是更新指令后行为的一致性会有所提升。

## 添加新的存储库规范

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在将编码标准记录在文件中，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。留意此处记录了项目编码标准，但尚未说明代码文档要求。
3. 在现有规范下方添加以下 Markdown 行，要求 Copilot 添加文件注释标头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指令，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务创建指令。存储库级文件适合放置项目级标准，例如刚刚添加的文档注释规则。

## 再次发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清空上下文。
2. 重新打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 再次发送相同提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议的文件现在会以类似以下内容的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目规范，并将其应用于后续练习会继续使用的真实代码。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，需要通过源代码管理进行维护。现在提交更改并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改均已包含：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新及 Copilot 为辅助函数基础实现更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求，并将其关联到待办列表中的文档规范 issue。你可以手动完成，也可以使用 Copilot 已连接到项目的 GitHub 工具。让 Copilot 查找 issue、创建用于关闭 issue 的 PR，然后合并该 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。稍后的练习会介绍此连接的工作方式，即模型上下文协议 (MCP)。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认代理选择器中选中了 **Agent**。
3. 请 Copilot 查找与更新指令文件有关的 issue，并从当前分支创建 PR，说明指令更新和新的 publisher 辅助函数基础实现：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. Copilot 提示你允许其代表你执行 GitHub 操作时，先检查命令，再酌情选择 **Allow**。
6. 创建 PR 后，提示 Copilot 合并 PR 并将本地分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 提示你允许其执行 GitHub 操作和 shell 命令时，先检查命令，再酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何从本项目的指令文件获取上下文，并在 VS Code 的 Copilot Chat 中：

- 发送代码生成提示，观察 Copilot 使用**现有**指令生成的内容。
- 在 `.github/copilot-instructions.md` 中添加一项新的全局存储库规范。
- 再次发送相同提示，观察建议的代码如何采用新规范。
- 将指令更新和辅助函数基础实现提交到 `main`，供下一练习继续使用。

接下来，在[代理模式][next-lesson]中使用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
