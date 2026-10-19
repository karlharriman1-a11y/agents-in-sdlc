---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、ユーザー体験の改善と機能追加を目的とする3つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールは多数あります。たとえば、GitHub.com の[agents ページ][agents-page]があります。この mission control では、オープンなプルリクエスト (PR) があるエージェントタスクを確認できます。実行された操作を調べ、進行中のセッションを誘導することもできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼して誘導します。

## シナリオ

エージェントにハイコントラストモードを作成させた後、チームはライトモードも追加するのに適したタイミングだと気付きました。サイトのスタイルを更新して切り替え機能を追加する作業がすでに進んでいるため、これも含めるのが自然です。ハイコントラストに加え、ライトモードも実装するようエージェントを誘導します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てられたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインにあるタスクの一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
    - コードベースのドキュメント更新
    - ゲームを変更する API の生成
    - Web サイトへのハイコントラストモードの追加
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような操作が含まれる場合があります。
    - リポジトリからコードをチェックアウトする
    - Copilot が作業する環境を作成する
    - MCP サーバーをセットアップする
    - 割り当てられたタスクを完了するためにさまざまな操作を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用したアプローチによって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更ファイルを表示し、進捗を監視できます。

## cloud agent を誘導する

実行中のタスクを確認できたので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。Copilot が付けた名前や作業状況によって、正確なタイトルは異なります。

    ![mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことが示されるまで数分間セッションを確認します。次のようなメッセージが表示されたら作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents ページでエージェントタスクの「Steer active session while copilot is working」ダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> キーを押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り入れることを確認します。

## Copilot の作業を待つ

前回と同様に、Copilot は更新されたタスクに取りかかります。メッセージを送信した時点で作業中だった手順を終えた後、新しい依頼を取り込みます。

今回も数分かかるため、これまでに学んだことを振り返る時間にできます。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視し、誘導する中心的な場所である Copilot agents ページを確認しました。この mission control では、実行中と完了済みのタスクを追跡し、作業内容をレビューし、進行中のタスクに新しい指示を追加して範囲を調整できます。

次の概念を確認しました。

- mission control と agents ページで cloud agent のタスクを監視する。
- 進行中のセッションを誘導して、機能追加を依頼する。

Copilot がアクセシビリティ機能を実装している間に、次は[Copilot が作成したプルリクエストを反復改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
