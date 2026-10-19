---
title: "练习 5 - 监视和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

启用 GitHub Copilot 代理模式后，它会自主探索代码库、提出更改、编辑文件并运行命令。由于 Copilot 在你的计算机上实时工作，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以在聊天中查看差异、接受或拒绝单项更改，并通过后续提示引导对话，以便在不离开编辑器的情况下完善或扩展工作。

在本练习中，你将：

- 实时监视无障碍访问代理的工作。
- 审查并接受建议的文件编辑。
- 在会话进行期间引导代理，同时添加浅色模式切换和高对比度切换。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍访问自定义代理正在实现高对比度模式。工作尚未完成时，设计团队又提出了新要求：他们还希望为觉得默认主题过暗的用户提供浅色模式切换。你无需重新开始对话，而是引导现有会话继续完成工作。

## 监视 Copilot 的进度

Copilot Chat 视图会显示代理执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每个文件编辑都会在聊天中以内联形式显示差异，并提供 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 代理工作时滚动聊天记录，查看每次工具调用。你会看到文件读取、文件编辑以及代理请求运行的终端命令。
3. 代理提出文件编辑时，差异会以内联形式显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让代理再次尝试。
4. 你还可以打开聊天顶部的 **Files changed** 指示器，查看代理在此会话中修改的完整工作文件集。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你确认安全的命令。

5. 等待代理完成高对比度模式。代理应会发送总结已完成更改的最终消息。

## 引导代理添加浅色模式

高对比度模式完成后，在同一对话中添加浅色模式切换。继续同一聊天会话时，代理会保留之前工作的上下文，包括已修改的组件、采用的样式方式和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会接续现有工作，查看先前做出的更改并提出浅色模式相关的补充更改。
3. 与高对比度模式一样，逐项审查新更改，并使用 **Keep** 或 **Undo** 处理每个差异。
4. 如果代理的方向不符合预期（例如，它尝试以你不喜欢的方式重构切换 UI），可以选择聊天输入框旁边的 **Stop** 按钮停止代理，然后发送澄清提示。

> [!TIP]
> 引导正在运行的会话比从头开始更快。可以使用后续提示请求调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式整合到现有设置面板中”）或扩展范围（“还要为这两个开关添加键盘快捷键”）。

## 审查工作文件集

提交工作前，快速检查代理修改的全部内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看更改文件列表。你应会看到 Astro 组件、样式和相关测试的更新。
3. 打开若干更改过的文件并查看差异。确认代码体现了自定义代理要求的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。你将在[下一课][next-lesson]中发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监视并引导正在进行的会话，审查每次文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

在本课中，你了解了：

- 实时监视代理的工作。
- 接受和拒绝单项文件编辑。
- 使用后续提示引导正在进行的会话。

在[下一课][next-lesson]中，你将把无障碍访问工作作为拉取请求发布，并审查本实验中本地自定义代理生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
