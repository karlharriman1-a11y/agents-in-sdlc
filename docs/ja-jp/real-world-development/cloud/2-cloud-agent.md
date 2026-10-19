---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタムインストラクション][previous-lesson] |
|:--|

技術的負債に悩まされていない組織はほとんどありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフトプルリクエストを作成します。面倒な作業を任せて開発を加速できるため、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境のカスタマイズ
- 操作を安全に実行する方法
- 明確に範囲を定めた issue の重要性
- issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys には解消したい技術的負債があります。サイトの初期バージョンを作成した請負業者は、ドキュメントを適切に整備しませんでした。実際、ドキュメントはほとんどありません。まず、アプリケーションのエクスポート関数すべてに TSDoc doc comment を追加したいと考えています。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに、同じカテゴリーの他のタイトルなど、関連ゲームを表示する要望があります。まだ完成度の高い実装でなくても、ユーザー体験 (UX) の受け入れテストに使えるものを必要としています。この作業は現在ブロックされていますが、優先度の高い他の issue もあります。

どちらも優先順位が下がりがちなタスクであり、Copilot cloud agent に割り当てるのに適しています。cloud agent が非同期で作業する間、開発者は別のタスクに取り組み、後から戻って成果物をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。たとえば、[GitHub issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると Copilot は進捗を記録するドラフトプルリクエストを作成し、環境をセットアップしてタスクに取りかかります。実行中でも完了後でもセッションを確認できます。解決策をレビューできる状態になると、プルリクエストで通知されます。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]は慎重に決めてください。人工知能 (AI) ペアプログラマーとして扱い、段階的に進め、学び、試し、状況に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタムインストラクション

前の演習では、カスタムインストラクションと Copilot への影響を確認しました。すでに実習で `.github/copilot-instructions.md` を使った場合は、動作を見ているはずです。まだの場合は、この機会に確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれるインストラクションファイルを読み取り専用で確認し、後で影響を見分けられるようにします。

cloud agent に作業を割り当てる前に、次のインストラクションファイルを開いて内容を確認します。後で生成された変更に与える影響を見分けられます。リポジトリの GitHub Web UI、または起動済みの codespace で操作してください。

1. `.github/copilot-instructions.md` を開き、**Code standards** セクション、特に TypeScript の規約と TSDoc doc comment に関する要件を確認します。
2. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` フロントマターによりインストラクションが `**/*.test.ts` ファイルを対象としていることを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てるとき、生成されるプルリクエストに TSDoc doc comment、TypeScript の規約、コメントヘッダーが含まれるか確認します。これらはインストラクションファイルによるものです。後の演習で PR をレビューするときにも、改めて取り上げます。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを作成する場合でも、通常は必要な環境を用意し、適切な状態にするセットアップスクリプトを実行します。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェアエンジニア (SWE) と同じような方法で作業します。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を利用できるように環境をカスタマイズできます。ラボをスムーズに進め、学習機会を提供するため、この環境は事前に構成済みです。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンドテスト用ブラウザー、マイグレーションとシードを済ませた SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順に必要な範囲で* 最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルと同じですが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストで作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーにより、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の実行を待たず、正常に動作するか確認するのに役立ちます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古かったり、ドキュメントがなかったりするプロジェクトは少なくありません。この種の技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成を得意とするため、cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを作成してもらい、後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような内容です。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかる場合があります。Copilot は前述のワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されていない場合は、ウィンドウを更新してください。

16. 実行中のセッションをスクロールし、Copilot の作業を確認します。コードを調査して状況を把握し、計画を決めるために一時停止し、その後コードを作成する様子が見られます。

通常、この作業には数分かかります。Copilot cloud agent の主な目的のひとつは、作業を非同期で実行し、開発者が別のタスクに集中できるようにすることです。その機能を活用して、もうひとつのタスクを cloud agent に割り当ててから、アプリケーションの機能追加に取りかかります。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の利点のひとつは、作業を分担できることです。デザインチームが求める関連ゲームのセクションは短時間で実装できるかもしれませんが、その時間を別の作業に使うこともできます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく issue の最初のコメントに 👀 が表示され、Copilot が作業中であることがわかります。

![issue で作業中であることを目の絵文字で示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組み始めました。Copilot cloud agent は SWE と同様に動作するため、積極的に監視する必要はありません。完了後にレビューできます。ここでいったん、カスタムエージェントの作成と使用に進みます。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に issue を割り当てて、非同期に作業させることができます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに利用できます。

次の概念を確認しました。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確に定めた issue の重要性を理解する。
- issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタムインストラクションも利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
