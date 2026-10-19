---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を確認し、Autopilot で issue のフィルタリング機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。より大きな変更には、さらに堅牢なプロセスが必要です。GitHub Copilot CLI は組織の既存フローに沿って動作し、適切な方法で必要な機能を構築できるよう支援します。このレッスンから、issue を起点に新機能を作成し、コードの妥当性と機能の動作を確認して、最後にプロジェクトへマージする、一般的なエージェント主導の開発プロセスに取り組みます。

> [!NOTE]
> 機能のワークフロー全体で、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチや PR を分けますが、ここでは中核的な概念に集中するため、手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot 会話を開始する。
- Plan モードで要件を定義する。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動確認する。

機能開発を続けながら、リポジトリのインストラクションを更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが拡大し、訪問者がカテゴリーや publisher でゲームを絞り込める必要があります。バックログの issue には機能の説明がありますが、カテゴリーの組み合わせなど、実装前に合意が必要な詳細があります。Plan モードで決定事項を整理してから、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本的な開発プロセスは変わりません。むしろ、より重要になります。多くの開発者は、次のようなフローで作業します。

1. 実装内容を説明する issue から始めます。
2. 作成するものの計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功した後、コードをマージします。

> [!NOTE]
> チームや組織によって細部は異なりますが、多くの場合、上記の流れに沿っています。

この標準的なアプローチに従うことで、AI が生成したコードが要件を満たし、手書きのコードと同じ検証プロセスを経ていることを確認できます。

## 会話モード

**会話モード**は、エージェントに許可する自律性の程度を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: 開発者とエージェントが協力して作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントはまず計画を作成し、プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントは自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から開始する

フィルタリング作業を始める前に、Codespace とリポジトリ、ターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルタリング用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと GitHub の issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、Copilot に作業を任せる前に、機能実装やタスク実行のアプローチを定義できます。複雑な作業は時間をかけて計画することをお勧めします。Plan モードに切り替え、計画を作成するよう Copilot に依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 先ほど追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適なアプローチを検討して計画を作成します。

3. フィルタリングの動作について質問された場合は、希望に沿って回答します。正解はひとつではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画をスクロールして確認します。
6. 決定事項と異なる部分があれば、計画の修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に移ります。Autopilot モードで Copilot に作業を進めてもらいます。

Autopilot は、完了したと判断するまで問題を反復して解決します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様のラベルの選択肢を選びます。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、実装がすぐに始まる場合があります。事前に計画を確認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と見なす前にセットアップの問題を解消します。

## 実装をレビューして検証する

生成されたコードは、他のコードと同じようにマージ前にレビューします。コードとサイトを確認し、正常に動作することを確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モードに **Plan** や **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力してフィルタリングの実装とテストを調べます。
3. issue と計画時に決めた内容に照らして結果を確認します。
4. コードをレビューしたら、<kbd>Esc</kbd> を押して diff ビューを終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードに問題がなくても、動作することを確かめます。前と同じようにアプリを起動し、Codespaces の転送ポート経由でサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces にポート `4321` が使用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリー、publisher、およびその組み合わせによるフィルタリングを試します。
4. リセットと結果が空になる場合の動作が、issue と決定事項に一致することを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot 会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[カスタムインストラクションを使って][next-lesson]コード生成の詳細を確認し、文書化されたプラクティスに沿っていることを確かめます。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
