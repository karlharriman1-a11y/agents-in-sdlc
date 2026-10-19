---
title: "练习 4 - 监视和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几个练习中，你让 Copilot cloud agent 执行了三个任务，旨在改进用户体验并添加功能。Cloud agent 虽然能够异步自主工作，但监视任务进度仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制中心，你可以查看所有具有未关闭拉取请求 (PR) 的代理任务，了解已执行的操作，甚至引导正在运行的会话。

在本课中，你将：

- 浏览代理页面，监视 cloud agent 任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将任务分配给代理以创建高对比度模式后，团队意识到也可以同时添加浅色模式。既然已经在更新网站样式并添加切换功能，一并加入浅色模式也很合理。你将引导代理，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务状态。

1. 前往 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 为修改游戏生成 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 创建 Copilot 工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和所采用的方法而有所不同。

4. 同时留意右侧显示的拉取请求窗格。你可以在此查看 PR 和已更改文件，以便进一步监视。

## 引导 cloud agent

了解当前正在执行的任务后，在代理处理高对比度模式时，要求 Copilot 也添加浅色模式切换。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而有所不同。

    ![任务控制中心中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示已完成设置并开始工作。看到类似以下的消息时，即表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面上的 cloud agent 任务截图，其中突出显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会开始处理更新后的任务！在你发送消息时，Copilot 会先完成当前步骤，然后将新请求纳入工作流程。

这与之前一样需要几分钟，正好可以暂停一下，回顾至今学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监视和引导 GitHub Copilot cloud agent 任务的中心位置。在这个任务控制中心，你可以跟踪所有活动和已完成的任务、审查正在执行的工作，甚至重定向正在进行的任务以调整范围或提供额外指导。

你了解了以下概念：

- 使用任务控制中心和代理页面监视 cloud agent 任务。
- 重定向正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍访问功能，接下来请[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
