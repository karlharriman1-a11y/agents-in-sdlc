---
title: "课程 7 - 创建并使用 QA 代理"
description: "创建 QA 自定义代理，整合 issue 要求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并通过 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到自定义代理中，并为其定义明确的 QA 流程。

在本课中，你将：

- 了解自定义代理如何使用指令、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 代理，根据筛选 issue 审查其调查结果。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 之前，以一致方式审查要求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程并生成可复用报告。

## 什么是自定义代理？

自定义代理是通过 Markdown 配置文件定义的专用 Copilot。配置文件会说明代理用途、指令和可用工具。在本工作坊中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你已使用的自定义项各有不同用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何使用这些能力来评估要求并报告结果。它不会取代这些功能，也不需要开始新的对话。

## 创建 QA 配置文件

创建功能 PR 前，先让 Copilot 创建可复用的 QA 配置文件。该配置文件将定义 QA 要执行的检查及其必须遵循的边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 要求默认代理创建新的自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新代理前，先审查其配置文件，确认 Copilot 准确记录了 QA 工作流和边界。这样可以避免代理不完整或范围过大，在你只想验证功能时却修改功能实现。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需字段；`name` 是可选字段，但添加后可为代理提供清晰的显示名称。
3. 阅读配置文件中的指令，确认 QA 会从要求开始，遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、在更改实现代码前征求许可，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件遗漏了任何职责或边界，请要求默认代理先修改，再继续。

## 根据 issue 运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置文件后，恢复同一筛选对话并选择 QA，以便它使用已有的 issue 和规划决定。

1. 输入以下命令启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的代理可能不会出现在可用代理列表中，但它已经存在，上述命令可以激活它。

2. 使用以下提示要求 QA 代理审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 代理会开始工作。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你已在工作流程中添加可复用的专用角色并审查其工作。在本课中，你：

- 了解了自定义代理如何使用指令、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 代理，并根据筛选 issue 审查了调查结果。

现在，你已准备好实现、技能更新、QA 配置文件、测试和验证报告。接下来，[将它们整合到功能 PR 中并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
