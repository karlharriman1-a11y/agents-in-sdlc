---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、任意の cloud agent 委任に使えるスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

一般的な CLI ツールと同様に、GitHub Copilot CLI にはさまざまな操作を行うスラッシュコマンドがあります。高度な機能、内部情報、追加の構成オプションを利用できます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、他の便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot の会話コンテキスト管理を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキストウィンドウに達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュコマンドで内容を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、まだ Copilot CLI を開いていない場合はリポジトリルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システムインストラクション、ツール、メッセージ、空き領域にコンテキストがどのように分割されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. 再度 `/context` を入力し、結果を比較します。会話が短い場合は大きな変化がないことがあります。

> [!NOTE]
> コンテキストウィンドウがいっぱいになると、Copilot CLI は自動的に圧縮します。圧縮のタイミングを自分で決めるには `/compact` を使います。無関係なタスクに切り替える場合は `/clear` または `/new` で新しい会話を開始します。

## モデルを選択する

モデルごとに得意分野が異なり、開発者によって好みも違います。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールの活用を改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、ローカルに Markdown を出力する `/share file` など、目的に合った保存先を選択します。
3. エクスポート内容を誰かに送ったり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する意図のないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を拡大または委任する

コアのワークショップは完了です。Copilot CLI には大規模なタスク向けのコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに分け、並行実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は Copilot cloud agent にタスクを送信します。cloud agent は非同期で作業し、プルリクエストを作成する場合があります。

これらのコマンドは追加の worktree やリモート作業を作成する可能性があるため、任意です。試す場合は、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューします。非同期エージェントの作業を詳しく確認する場合は、[cloud agent のワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドで構成を変更したり、セッションを共有したり、内部の状態を確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot の会話コンテキスト管理を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への任意の委任コマンドを確認しました。

Copilot CLI には他にもスラッシュコマンドがあり、さらに探索できます。最後に、[学んだことと次のステップを確認します][next-lesson]。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
