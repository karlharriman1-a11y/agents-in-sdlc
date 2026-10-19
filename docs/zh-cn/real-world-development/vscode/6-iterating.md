---
title: "练习 6 - 迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← 上一课：监视和管理代理][previous-lesson] |
| :-- |

## 审查工作成果

在本实验中，你与 GitHub Copilot 协作完成了多项任务，以改进用户体验。你使用代理模式跨客户端和服务器添加筛选功能，使用 Playwright MCP 服务器在浏览器中手动测试，然后使用自定义代理实现高对比度和浅色模式切换等无障碍访问功能，并在会话进行期间引导代理扩展工作。现在将本地工作发布出去，并像团队成员一样审查它。

### 场景

生成式 AI 不会改变软件设计和 DevOps 的基本原则。对于 Copilot 生成的任何内容，你仍应进行完整审查。现在将无障碍访问更改从 codespace 推送到分支、创建拉取请求，并在团队其他成员参与前仔细查看差异。

## 发布无障碍访问功能

你已在[练习 4][exercise-4]和[练习 5][exercise-5]中使用无障碍访问自定义代理实现高对比度和浅色模式切换，这些更改已提交在 codespace 中。现在将它们推送到分支并创建拉取请求，供团队审查。

1. 返回 codespace。
2. 在 VS Code 中打开 **Source Control** 视图。
3. 确认无障碍访问更改已提交。如果练习 5 后仍有未提交的更改，现在暂存并提交，并使用描述性消息，例如 `Add high-contrast and light-mode toggles`。
4. 选择 **Publish Branch** 发布分支（也可以选择 **...** 菜单 → **Push**）。
5. VS Code 会询问是否在 github.com 上打开新分支。接受提示，或手动前往存储库并选择分支横幅中的 **Compare & pull request**。
6. 设置清晰的标题（例如 `Add high-contrast and light-mode toggles`），并简要说明更改内容和原因。
7. 选择 **Create pull request**。
8. 打开 PR 后，选择 **Files changed** 选项卡，全面审查更改。重点留意：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（无障碍富互联网应用 (ARIA) 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

9. 返回 **Conversation** 选项卡。
10. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

11. 等待工作流完成。如果一切正常，检查应会通过。

> [!TIP]
> 想听听别人对无障碍访问工作的意见吗？在 PR 评论中提及 `@copilot`，并提出类似“审查此 PR 是否存在其他 Web 内容无障碍指南 (WCAG) 问题”或“建议改进键盘导航”的请求。Copilot 会启动新会话来处理评论。

## 可选练习 - 继续在本地探索

在 IDE 中与代理迭代协作是一项技能，只能通过反复练习来提升。你可以在 VS Code 中尝试以下后续任务：

- 在游戏详情页添加支持者兴趣表单。
- 在游戏列表页实现分页。
- 为 `src/lib/` 中的数据访问辅助函数添加输入验证和错误处理。
- 扩展无障碍访问代理的范围，例如审查整个网站的键盘焦点顺序。

## 总结

恭喜你，已完成 VS Code 环境的练习！在本实验中，你：

- **使用 Playwright MCP 手动测试了功能。** 你添加了 Playwright MCP 服务器，并让 Copilot 驱动浏览器，在创建拉取请求前验证筛选功能。
- **通过代理模式协调跨技术栈的更改。** 你在单个会话中添加了涉及客户端、服务器和测试的筛选功能。
- **使用了自定义代理。** 你从代理选择器中选中了专注于无障碍访问的自定义代理，并观察它根据存储库实现高对比度模式。
- **管理并引导代理会话。** 你以内联方式审查建议的更改、接受所需内容，并通过后续提示扩展会话以添加浅色模式。
- **通过拉取请求完成了工作闭环。** 你发布了本地工作，并像团队成员一样全面审查了更改。

## 审查和后续步骤

必需的 VS Code 环境练习到此结束。工作坊已完成，你可以在此停止。

如果想进一步探索 Copilot 代理功能，其他环境会通过不同界面介绍相关场景：

- 💻 **[CLI 环境](../../cli/)** — 在终端中通过 Plan 和 Autopilot 模式、技能、自定义 QA 代理、Playwright MCP、Agent Merge 和上下文衔接交付两个经过审查的更改。
- ☁️ **[Cloud agent 环境](../../cloud/)** — 重点了解如何将 issue 分配给 cloud agent、通过代理页面监视会话，以及异步迭代拉取请求。

你也可以继续完善刚刚完成的工作。[awesome-copilot][awesome-copilot] 收集了更多可用于自己项目的指令文件、自定义代理和技能。

如果想进行可选扩展，[可选：集成 Foundry][exercise-7] 将使用 VS Code 和 Microsoft Foundry Toolkit 准备模型、部署 Backer Concierge 并将其连接到网站。

## 资源

- [GitHub Copilot][github-copilot]
- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
