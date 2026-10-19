---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、さまざまな作業があります。手動で動作を確認し、インストラクションファイルで標準への準拠を支援できました。次はテストや lint など、継続的インテグレーション (CI) の他の作業を扱います。

このようなタスクには **エージェントスキル** が適しています。スキルは、Copilot がこうした操作を適切に実行する方法を理解できるようにします。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys はユニットテスト、lint、型チェックに `quality-checks` スキルを使用しています。結果を読みやすくするため、チームはレポートを改善したいと考えています。

## インストラクション、スクリプト、リソース

エージェントスキルは、エージェントが必要に応じて読み込む再利用可能なタスク指示、実行可能なスクリプト、補助リソースをまとめたものです。基本的にはスキル名のフォルダーで、その中に `SKILL.md` という Markdown ファイルがあります。この Markdown にはスキルの名前と説明を定義するフロントマター、機能の概要、呼び出すタイミングに関するガイダンスが含まれます。フォルダーにスクリプトやその他のリソース用のサブフォルダーを追加することもできます。

> [!NOTE]
> スキルに他のファイルやフォルダーは必須ではありません。Tailspin Toys の `quality-checks` スキルはプロジェクトの既存コマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、チームで共有して再利用するリポジトリアセットとしてプロジェクトの `.github/skills` フォルダーに配置するか、ユーザーのスキルフォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明によって Copilot はスキルを呼び出すタイミングを判断します。
3. 指示を読み、テストと lint の手順をどのように案内しているか確認します。
4. スキルに **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出すことも、自然言語で依頼することもできます。3つのチェックを実行するよう Copilot に依頼します。

1. Interactive モードのフィルタリング会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行し、成功したか、ツールが実際にどのような結果を報告したかが分かるレポートに改善します。スキルを更新します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイル末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、動作を確認します。Copilot CLI は会話を再起動せずに編集済みスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 前と**完全に同じプロンプト**を入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェックを行う `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、機能 PR でフィルタリングの変更とともに提出します。次は [Playwright MCP サーバー][next-lesson]を使って、Copilot がサイトを直接操作できるようにします。

## スキルの例

コミュニティによる次の例は参考資料であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプトを含む `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
