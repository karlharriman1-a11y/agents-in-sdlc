---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 协作完成了多个任务，改进用户体验并添加功能。你要求 Copilot 为代码添加文档、为设计团队构建相关游戏功能以便迭代改进，还实现了高对比度和浅色模式切换等无障碍访问功能。现在来查看代码更改，必要时向 Copilot 提供反馈以改进成果。

### 场景

如前面多次强调的那样，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。现在，在为团队其他成员启用审查之前，先查看 GitHub Copilot 为代码文档、相关游戏功能和无障碍访问功能提供的建议。

## 安全性和 GitHub Copilot cloud agent

由于 Copilot cloud agent 会在无人监督的情况下异步执行任务，因此设置了一些安全限制来保障工作安全，包括：

- Copilot 对存储库仅有读取权限，对代码仅有其工作分支的写入权限。
- Cloud agent 在 GitHub Actions 中运行，并会创建独立的临时环境来完成工作。
- GitHub Actions 工作流必须经过人工批准才能运行。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先来查看 GitHub Copilot cloud agent 创建的第一个拉取请求 (PR)，即为代码添加文档。你将使用 GitHub.com 标准 PR 界面完成此操作。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器调用。如有需要，可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或内容更具体的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会显示 **[WIP]** 标记。此时请等待 Copilot 完成工作。可能需要几分钟，你可以稍作休息或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新创建的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和注释标头。这些要求来自练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 你应会看到有工作流正在等待批准的提示。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 拉取请求的检查区域应会显示工作流进入队列。如果一切正常，单体 Astro 应用的项目检查应会通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 进行更改

与 Copilot 协作处理拉取请求并非单向交流。你可以像提及团队其他成员一样，在拉取请求评论或代码内联评论中提及 Copilot。Copilot 会读取评论并启动另一个会话来处理请求。由于结果具有非确定性，我们无法提供固定提示文本。你可以要求 Copilot 更新以下内容：

- 在每个代码文件顶部添加简短说明其用途的注释标头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，说明 Astro 应用结构。

1. 添加评论，请求 Copilot 修改生成的文档；像提及其他用户一样提及 **@copilot**。你可以使用上述任一建议，或提出其他你希望代码库中的文档具备的要求。
2. 选择 **View Session**，观察 Copilot 执行工作。留意 Copilot 如何启动新会话来进行更新。
3. 选择 **Back to pull request** 可返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。满意后，可将 PR 从草稿转换为可供审查的状态，再合并到 main 分支。

![将 PR 转为可供审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来，查看 Copilot 为解决“在游戏详情页显示相关游戏” issue 创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或内容更具体的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 你应会看到有工作流正在等待批准的提示。
7. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 拉取请求的检查区域应会显示工作流进入队列。如果一切正常，单体 Astro 应用的项目检查应会通过。此过程可能需要几分钟。
9. **可选：** 你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。前往 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！和之前一样，你可以通过迭代与 Copilot cloud agent 协作并请求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释标头和 TSDoc 文档注释（请记住，分配该任务时，**你还没有**更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中添加评论来提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍访问功能

最后，查看使用无障碍访问自定义代理实现的功能。此 PR 应同时包括练习 3 中分配的高对比度模式，以及练习 4 在任务控制中心中要求添加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或内容更具体的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会显示 **[WIP]** 标记。此时请等待 Copilot 完成工作，可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现，重点留意：
   - 用于切换模式的 UI 组件
   - 用于保存用户偏好的本地存储
   - 高对比度和浅色模式的 CSS 或样式更改
   - 无障碍属性（无障碍富互联网应用 (ARIA) 标签、键盘导航等）
   - 管理模式切换的 JavaScript/TypeScript 代码

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 你应会看到有工作流正在等待批准的提示。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 拉取请求的检查区域应会显示工作流进入队列。如果一切正常，单体 Astro 应用的项目检查应会通过。此过程可能需要几分钟。
10. **可选：** 你也可以在 Codespace 中切换到此分支，手动测试无障碍访问功能。前往 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，确认功能正常且页面重新加载后设置仍然保留。

留意无障碍访问自定义代理如何引导 Copilot 遵循无障碍最佳实践实现这些功能。如果发现无障碍问题或有改进建议，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

当你能将真实的待办事项交给 cloud agent 并转而处理其他工作时，它最能发挥作用。可以在存储库中提交更多 issue 并分配给 Copilot，培养这种协作方式。以下是一些建议：

- 在游戏详情页创建支持者兴趣表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 环境的练习。在这些课程中，你：

- **查看了此存储库附带的自定义指令文件**，以便稍后了解它们对 cloud agent 输出的影响。
- **将 issue 分配给 Copilot cloud agent**，并观察它如何设置环境、规划并异步执行任务。
- **创建并使用了无障碍访问自定义代理**，添加高对比度和浅色模式切换。
- **将 Copilot agents 页面用作任务控制中心**，监视并引导正在运行的无障碍访问会话。
- **审查并迭代改进了 cloud agent 的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 审查和后续步骤

你已完成 Cloud agent 环境的练习。如果想继续探索，其他环境的工作坊可以帮助你拓展本次实践：

- 🖥️ **[VS Code 环境](../../vscode/)** — 在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 在终端中通过 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接功能，交付两个经过审查的更改。

还可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给 cloud agent。
- 为其他领域创建自定义代理。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
