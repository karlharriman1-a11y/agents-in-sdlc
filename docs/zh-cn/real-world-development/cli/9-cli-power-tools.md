---
title: "课程 9 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可以访问高级功能、后台信息和其他配置选项。你已经使用了 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来了解其他实用命令。

在本课中，你将：

- 使用 `/context` 和 `/compact` 探索 Copilot 如何管理对话上下文。
- 使用 `/model` 了解可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派给 cloud agent 的可选命令。

## 场景

你已完成 CLI 核心工作流程。现在来了解一些其他功能，包括管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot cloud agent][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话，你也可以使用斜杠命令检查或手动压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意模型、当前令牌用量，以及上下文在系统指令、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话原本就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口即将满时，Copilot CLI 会自动压缩上下文。如果想自行决定压缩时间，可以使用 `/compact`。切换到无关任务并希望开始新对话时，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，开发人员也有不同偏好。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和分享经验有助于每个人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看已安装版本中的 `/share` 选项。
2. 如果要共享会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 在发送给其他人或发布之前，检查导出的内容。会话导出可能包含提示、回复和项目详细信息。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展工作规模或委派任务

核心工作坊现已完成。Copilot CLI 还提供了适用于更大任务的命令：

- `/fleet` 可以将独立子任务分配给多个子代理并行执行。
- `/worktree` 可以为不同任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务交给 Copilot cloud agent 异步处理，cloud agent 还可能创建拉取请求。

这些命令是可选的，因为它们可能创建额外工作树或触发远程工作。尝试之前，先启动一个范围明确的新任务，并通过常规工作流程审查结果。如果要深入了解异步代理工作，请继续学习 [cloud agent 工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以配置工具、共享会话，并了解后台运行情况。在本课中，你：

- 使用 `/context` 和 `/compact` 探索了 Copilot 如何管理对话上下文。
- 使用 `/model` 了解了可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行工作、工作树和委派给 cloud agent 的可选命令。

Copilot CLI 还有更多斜杠命令和功能可供探索！接下来，[回顾本次学习内容][next-lesson]并了解后续学习建议。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
