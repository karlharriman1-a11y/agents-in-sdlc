---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会面临技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 的 cloud agent 可以自主执行更新代码和添加功能等任务。完成工作后，它会创建草稿 PR，供开发人员审查。这既能卸下繁琐任务、加快开发进度，也能让开发人员专注于更重要的工作。

你将了解 Copilot cloud agent 如何：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 处理范围明确的 issue。
- 接收 issue 分配。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受聘开发网站的承包商没有妥善编写文档——实际上，代码完全缺少文档。团队希望首先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望每款游戏的详情页都显示相关游戏，即同类别的其他游戏，以便用户继续浏览。目前不需要完善的实现，只需要一个可用于用户体验验收测试的版本。虽然这项工作目前阻碍了进度，但其他 issue 的优先级更高。

这类任务很容易被降级为低优先级，非常适合交给 Copilot cloud agent。它可以异步处理任务，让开发人员先专注于其他工作，之后再回来审查 Copilot 的成果，确认一切符合预期。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发人员一样在后台执行任务。你可以通过多种方式委派工作，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看详情。准备好审查建议的解决方案时，Copilot 会在拉取请求中提及你。

## 明确指令范围的重要性

GitHub Copilot 并非魔法，不能替你省去思考。即使看似简单的操作，深入分析后也可能包含复杂性，因此[应谨慎界定 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。把它当作 AI 结对编程伙伴：分阶段工作、持续学习、试验并适时调整。生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令以及它们如何引导 Copilot。如果你已经完成了自定义指令实践，就看过 `.github/copilot-instructions.md` 的效果；否则，现在可以快速查看。在将工作分配给 Copilot cloud agent 之前，只读查看此存储库中的指令文件，以便之后观察它们的影响。

在存储库的 GitHub 网页界面中打开以下文件；如果 codespace 已运行，也可以在那里打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，特别是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将这些指令限定到 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` issue 分配给 cloud agent 时，留意生成的拉取请求是否遵循 TSDoc 文档注释、TypeScript 约定和注释标头要求。这些要求来自指令文件。稍后的练习中审查 PR 时会再次提到这一点。

## 设置 Copilot cloud agent 的开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保环境状态正常。将任务分配给 Copilot 时也一样，它的工作方式与软件工程师 (SWE) 类似。

Cloud agent 在工作环境中使用 [GitHub Actions][github-actions]。你可以创建一个[专用设置工作流][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 中，并在 cloud agent 开始工作前运行，以便它访问所需的开发工具和依赖项。本实验已预先配置此工作流，以简化操作并提供实践机会。它会为单体 Astro 应用安装 Node.js、项目依赖项和端到端测试所需的浏览器二进制文件，并迁移和填充 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设置为这些步骤所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果设置步骤中不克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现采用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试提供数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件看起来与其他 GitHub 工作流文件类似，但有几个重点：

- 它包含名为 `copilot-setup-steps` 的单个作业。Copilot 开始处理拉取请求前，GitHub Actions 会执行该作业。
- 留意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，便于测试工作流，而不必等待 Copilot 启动它。

## 添加文档

虽然大家都知道文档的重要性，但许多项目的文档要么过时，要么根本不存在。这类技术债务经常得不到处理，拖慢开发效率，也让代码库维护和新成员入职变得更加困难。Copilot 擅长创建文档，因此很适合把这项工作分配给 Copilot cloud agent。它会在后台生成所需文档。后续练习中，你会回来审查结果。

1. 在新的浏览器选项卡中前往 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建 issue 对话框。
4. 选择 **Blank issue**，创建空白 issue。
5. 在 **Title** 中输入 `Code lacks documentation`。
6. 在 **Description** 中输入：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求 (PR)，标题可能类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表及其准备完成的任务。
14. 向下滚动拉取请求时间线，应能看到 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 你可能需要刷新页面，才能看到更新后的状态指示器。

16. 浏览实时会话，查看 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并制定合适计划，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们可以专注于其他工作。接下来就利用这一功能：再分配一项任务给 Copilot cloud agent，然后开始编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你处理一组任务，它处理另一组任务。为设计团队添加相关游戏部分也许不会花太长时间，但这些时间仍可用于其他工作。现在将它分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建 issue 对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，你会在 issue 的第一条评论中看到 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理你的第二个请求！Copilot cloud agent 的工作方式类似软件工程师，因此你无需主动监视它，可以等任务完成后再审查。接下来，我们来创建并使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将 issue 分配给 cloud agent，让 Copilot 异步处理。可以用 Copilot 解决技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

你了解了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

Copilot cloud agent 正在后台工作，现在可以开始创建和使用自定义代理。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以使用前面模块介绍的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
