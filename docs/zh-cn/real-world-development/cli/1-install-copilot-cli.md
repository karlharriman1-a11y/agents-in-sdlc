---
title: "课程 1 - 安装 GitHub Copilot CLI"
description: "在 Codespace 中安装并验证 Copilot CLI，熟悉其用法并找到预置的筛选 issue。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] 是一个功能强大的代理式编码助手，可在终端中运行，让你通过命令行探索代码库、生成代码、运行命令并与外部工具交互。它可以帮助分担任务、请求更改并保持专注。首先需要安装该工具；可以使用熟悉的工具完成安装。

在本课中，你将：

- 使用 npm 安装 GitHub Copilot CLI。
- 使用 GitHub 账户完成验证。
- 信任工作坊存储库并尝试快速对话。
- 通过内置 GitHub 模型上下文协议 (MCP) 服务器查找筛选 issue。

## 场景

团队开始使用 AI 代理处理不断增长的待办事项。Copilot CLI 将此能力带到终端中，而许多开发人员的主要工作环境正是终端。本课将完成安装和验证，为后续工作坊中的使用做好准备。

## 安装 Copilot CLI

可以通过 [npm][install-npm]、[WinGet][install-winget] 和 [Homebrew][install-homebrew] 安装 Copilot CLI。由于 GitHub Codespaces 已预装 Node.js，本课将使用 npm 安装 Copilot CLI。

1. 返回 Codespace 并打开终端。
2. 确认 Node.js 已安装，并满足版本要求：

   ```bash
   node --version
   ```

   应看到版本 24 或更高。

3. 使用 npm 全局安装 Copilot CLI：

   ```bash
   npm install -g @github/copilot
   ```

4. 通过查看版本验证安装：

   ```bash
   copilot --version
   ```

   应看到显示的版本号。

## 使用 GitHub 完成验证

首次启动时，Copilot CLI 会提示使用 GitHub 账户完成验证。

1. 启动 Copilot CLI：

   ```bash
   copilot
   ```

2. 如果出现提示，请按照设备代码说明完成身份验证并授权 Copilot CLI。
3. Copilot CLI 会显示以下提示：

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 确认显示的路径是 Tailspin Toys 存储库，然后选择 **Yes, and remember this folder for future sessions** 表示信任。

> [!NOTE]
> 在 Codespace 中，你可能已通过 GitHub 会话完成身份验证。如果 Copilot CLI 启动时没有提示验证，就可以直接使用。

## 熟悉 Copilot CLI

普通 shell 提示符中的命令会直接在 Codespace 中运行。启动 Copilot CLI 后，自然语言会发送给代理，斜杠命令则用于控制对话。

1. 输入 `/model`，使用方向键选择 **Auto**，按 <kbd>Enter</kbd>，然后再次按 <kbd>Enter</kbd> 确认。
2. 输入 `/help` 查看已安装版本中可用的命令，然后按 <kbd>Esc</kbd> 关闭帮助界面。
3. 向 Copilot 提一个简单问题，确认一切正常：

   ```plaintext
   What are the key files in this project?
   ```

4. 阅读回复，留意 Copilot 如何先探索存储库再作答。
5. 输入 `/mcp list`，确认内置 GitHub MCP 服务器可用。
6. 使用以下提示让 Copilot 查找筛选 issue：

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 打开 URL 并阅读 issue。完成第一个快速更改后会用到它。

> [!TIP]
> 常规 Copilot CLI 会话使用终端当前检出的分支，不会自动创建工作树。每次更改前都需要创建功能分支。

## 使用工作坊快捷方式

通常，Copilot CLI 在使用已获准权限范围以外的工具前会先征求许可。本工作坊将使用经用户批准的 `--yolo` 快捷方式重新启动 CLI，在 Codespace 中省去这些批准提示，让你专注于练习。

> [!CAUTION]
> `--yolo` 会启用完整的自动权限（`--allow-all-tools`、`--allow-all-paths` 和 `--allow-all-urls`）。只能在 Codespace 或虚拟机等隔离环境中使用，切勿将其设为日常开发的默认别名。详情请参阅[允许和拒绝工具使用][allow-all-warning]。
>
> 本工作坊使用 `--enable-all-github-mcp-tools` 启用后续课程中用于处理 issue 和拉取请求的 GitHub MCP 读写工具。Codespace 会限制 Copilot 对本地计算机的访问，但经过身份验证的 GitHub 资源仍是真实资源。发布或合并之前，请审查更改。

1. 输入 `/exit` 退出 Copilot CLI。
2. 在存储库根目录重新启动：

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 再次询问一个有关项目的简单问题，确认对话正常运行，然后输入 `/exit` 退出。

Copilot 会自动保存对话。稍后更改指令或添加代理后，可以使用 `copilot --resume` 返回同一功能对话和分支。

## 总结和后续步骤

恭喜！在本课中，你：

- 使用 npm 安装 Copilot CLI。
- 使用 GitHub 账户完成验证。
- 信任工作坊存储库并尝试快速对话。
- 通过内置 GitHub MCP 服务器查找了筛选 issue。

接下来，[开始第一个有针对性的更改][next-lesson]，使用 Copilot CLI 在游戏卡片上显示星级评分。

## 资源

- [安装 GitHub Copilot CLI][install-copilot-cli]
- [关于 Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-copilot-cli]: https://docs.github.com/copilot/how-tos/set-up/install-copilot-cli
[install-npm]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli#installing-with-npm-all-platforms
[install-winget]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli#installing-with-winget-windows
[install-homebrew]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli#installing-with-homebrew-macos-and-linux
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
