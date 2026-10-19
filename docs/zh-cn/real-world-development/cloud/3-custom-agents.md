---
title: "练习 3 - 自定义代理"
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

使用自定义代理时，你将了解：

- 自定义代理的定义方式。
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于确保所有用户都能使用其众筹平台，不受视觉能力或偏好的限制。近期用户反馈指出，由于文本与背景颜色对比度不足，一些用户觉得当前暗色主题难以阅读。为解决这一无障碍访问问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍访问至关重要，因此需要尽快实现此功能。你将使用自定义代理生成相关功能。

## 查看无障碍访问自定义代理

已为你创建了一个无障碍访问自定义代理。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后查看无障碍访问自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意其中包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 接下来，浏览并查看以下部分：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍访问最佳实践。
   - 超文本标记语言 (HTML)、层叠样式表 (CSS) 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 创建并分配 issue

任务控制中心是管理环境中所有代理的中心位置。你可以在此向 Copilot cloud agent 分配任务、监视任务，甚至重定向代理并提供补充指导。现在将创建高对比度模式的任务分配给 Copilot。

1. 前往存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建 issue 对话框。
4. 选择 **Blank issue**，创建空白 issue。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![Cloud agent 分配界面截图，其中突出显示了自定义代理和 Accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即专为特定任务和领域定制的 AI 助手。借助自定义代理，你可以将团队专业知识和标准编码为可复用代理，让 Copilot 更有效地完成特定类型的工作。

你了解了以下概念：

- 自定义代理的定义方式。
- 将任务分配给自定义代理。

Copilot 正在实现高对比度模式，接下来请前往任务控制中心，[监视并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [在组织中使用自定义代理的准备工作][org-custom-agents]
- [在企业中使用自定义代理的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监视和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
