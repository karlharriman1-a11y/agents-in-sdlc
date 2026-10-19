---
title: "课程 2 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查结果，并通过第一个拉取请求合并更改。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以开始修改项目了。先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示评分。你将要求代理显示评分，在浏览器中审查更改，并将它作为第一个拉取请求合并。

在本课中，你将：

- 在功能分支上启动有针对性的 Copilot 对话。
- 要求代理对项目进行小幅修改。
- 使用 `/diff` 审查更改。
- 在转发的浏览器中运行应用并确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，游戏详情页已经会显示评分，但主页上的游戏卡片目前只显示标题、类别、publisher 和说明。作为热身练习，你将让代理在每张卡片上显示现有评分。这是一项小型、独立的更改，非常适合首次使用会话。

## 对话的组成部分

**对话**是你使用 Copilot CLI 处理任务的工作空间。与 Copilot app 不同，常规 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可供日后继续使用，文件和分支则仍是磁盘上的普通 Git 状态。

对话中会显示三类内容：你的提示和代理回复、代理探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

现在开始新对话，实现此功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，执行以下命令启动：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保启动新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到用于显示游戏详情的文件并编写所需代码。你已使用 Copilot CLI 添加了一项新功能！

## 审查差异

所有 AI 生成的更改都应在合并前审查，即使更改很小也不例外。现在直接在 Copilot CLI 中查看更改。

1. 输入 `/diff` 并检查所有更改文件。
2. 确认游戏卡片在有评分时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 查看 Copilot 运行的检查结果，并要求它修复任何失败项。
5. 完成审查后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> Copilot 和其他生成式 AI 工具一样，结果具有概率性而非确定性，因此具体代码可能有所不同。应审查功能行为，而不是期待某种固定实现。

## 检查更改

不能只阅读代码就假设它能正常工作。请让 Copilot 启动网站，以便在 Codespaces 转发的浏览器中查看更新后的用户界面 (UI)。

1. 要求 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了五分制评分。
4. 返回 Copilot 并要求它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

现在已创建功能，可以创建拉取请求 (PR)，将新代码合并到项目中。

1. 要求默认代理提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。创建 PR 时，Copilot CLI 会推送现有提交并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），选择 Copilot CLI 显示的 URL，打开 PR。
4. 审查更改文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜你！你使用 GitHub Copilot CLI 交付了第一个更改！具体来说，你：

- 在功能分支上启动了有针对性的 Copilot 对话。
- 引导代理修改游戏卡片。
- 使用 `/diff` 审查更改。
- 在转发的浏览器中运行应用并确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，从筛选 issue 开始，使用 Plan 和 Autopilot 模式[构建更大的功能][next-lesson]。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
