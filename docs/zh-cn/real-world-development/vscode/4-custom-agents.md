---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是专为开发工作流中特定任务或领域定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，为 Copilot 提供有针对性的指令、最佳实践、编码模式和领域知识，从而更有效地完成特定类型的工作。团队可以将专业知识编码为可复用代理，例如确保符合 [Web 内容无障碍指南 (WCAG)][wcag] 要求的无障碍访问代理、遵循安全编码实践的安全代理，或维护一致测试模式的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少要有 `name` 和 `description`，后面则是用于定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能的比较

自定义代理和[代理技能][agent-skills-concept]在概念上有一些重叠。两者主要都通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理**负责执行工作，**技能**提供工具。

自定义代理拥有自己的上下文窗口，并通过协调技能（甚至其他代理）来完成工作。在本实验中，无障碍访问自定义代理会根据无障碍指南审查并更新网站；在执行过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 创建自定义代理没有唯一的“正确”方式。与 AI 的其他用法一样，应在自己的环境和场景中测试并迭代改进。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于确保所有用户都能使用其众筹平台，不受视觉能力或偏好的限制。近期用户反馈指出，由于文本与背景颜色对比度不足，一些用户觉得当前暗色主题难以阅读。为解决这一无障碍访问问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍访问至关重要，因此需要尽快实现此功能。你将使用自定义代理生成相关功能。

在本练习中，你将：

- 查看现有的无障碍访问自定义代理。
- 在 Copilot Chat 中使用该代理实现高对比度模式。

## 查看无障碍访问自定义代理

已为你创建了一个无障碍访问自定义代理。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后打开终端并从 `main` 创建新的分支，用于单独处理无障碍访问功能（筛选功能的 PR 会继续保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意其中包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 接下来，浏览并查看以下部分：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍访问最佳实践。
   - 超文本标记语言 (HTML)、层叠样式表 (CSS) 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉列表中显示 `.github/agents` 中定义的所有自定义代理。你可以选择自定义代理，让聊天会话使用该代理的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。不同练习的模式和模型选择可能不同；相关步骤会在需要时说明。

1. 如果 Chat 视图的代理下拉列表中尚未选中 **Agent**，请先选择它。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉列表（其中显示当前代理；默认值为 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍访问代理发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并编辑项目文件。每次编辑都会在聊天中以内联形式显示，包括文件路径和差异，供你审查。
6. 随着编辑进行，**Files changed** 指示器会更新，让你查看代理修改的工作文件集。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，包括 Astro 组件、CSS 和相关测试。

下一课中，你将审查并引导正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即专为特定任务和领域定制的 AI 助手。借助自定义代理，你可以将团队专业知识和标准编码为可复用代理，让 Copilot 更有效地完成特定类型的工作。

你了解了以下概念：

- 自定义代理的定义方式。
- 在 Copilot Chat 代理模式中使用自定义代理。

接下来，[监视并引导代理的工作][next-lesson]，审查实时更改，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监视和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
