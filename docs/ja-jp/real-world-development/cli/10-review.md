---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、関連リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、事前登録されたフィルタリング issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング issue から始め、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタムインストラクションでエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証根拠を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR に Agent Merge を使いました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## リリースした変更

このワークショップでは、更新後の `main` から作成した別々のブランチで、2つの PR マイルストーンを達成します。

1. **星評価:** 既存の `starRating` と、未評価であることを明示する表示をゲームカードに追加します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、インストラクションを更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルと関連テストを追加します。

フィルタリングを計画して PR を開くまで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、この作業を1つの PR にまとめています。

## 複数の方法で検証する

自動テスト、自分でのブラウザー確認、MCP を使った Copilot のブラウザー検証など、複数の方法でコードをチェックしました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は PR の前にそれらの結果と要件およびテストカバレッジのレビューをまとめました。

追加テストは実際の不足を補う必要があります。QA の実行で新しいテストが不要と判断されることもあります。マージを許可する前にコードと根拠を確認し、変更後は影響を受ける検証結果を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが作業内容を左右します。このワークショップでは、インストラクションを更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを複数の会話で再利用し、チームのニーズに合わせて調整してください。インストラクションは標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果をレビューしてください。

タスクに応じて**モードとモデル**を選択します。構築前に方針を検討するには **Plan**、範囲を絞った変更で作業に参加し続けるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じく、コンテキストも重要です。作成するもの、理由、実現方法を明確に説明すると、出力が大きく変わります。

## さらに試す

コアのワークフローを確認しました。Copilot CLI には、他にも次の機能があります。

- `/review` でコードレビューエージェントに変更の分析を依頼する。
- `/rubber-duck` で問題を話し合い、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送る。

## 次のステップ

ツールの使い方を上達させるには、継続して使うのが最善です。本番コード、趣味のコード、何年も作ろうと思っていた小さなアプリなどで活用してください。チームで学びを共有し、他のメンバーからも学びましょう。そして、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張するには、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
