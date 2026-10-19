---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、複数ファイルの変更やテストの実行が必要になることがあります。開発者は通常、必要なファイルを探し、変更し、テストし、デバッグし、見落としたファイルを探してさらに更新する、といった作業を繰り返します。

こうした作業に役立つのが Copilot Agent Mode です。

Copilot Agent Mode は IDE 内でより自律的に動作します。プロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行し、検出した問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる体験を提供します。Agent mode は IDE 内でタスクを実行し、すばやいフィードバックと対話を可能にします。Cloud agent はチームのメンバーのように非同期で動作し、issue とプルリクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを特定して、複数ファイルにまたがる変更を行う。
- UI とデータレイヤーにまたがる新機能を実装する。
- 変更と Copilot が生成したテストをレビューし、コードベースにマージする前に確認する。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリーと publisher で絞り込めるようにします。前の演習で publisher ヘルパーを追加しました。Copilot Agent Mode を使って、残りのデータレイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。サイトは単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページは frontmatter から Drizzle オブジェクトリレーショナルマッパー (ORM) を介してローカルの SQLite データベースを直接クエリします。別のバックエンドアプリケーションプログラミングインターフェイス (API) やクライアントサイドのユーザーインターフェイス (UI) フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな機能は標準のドキュメントオブジェクトモデル (DOM) API を使う小規模なスコープ付き Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次のように実行します。

1. 前の演習後に `main` に戻っていることを確認し、codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択します。

> [!NOTE]
> codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Web サーバーをホストする codespace へのプライベートトンネルです。

### Web サイトを確認する

サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、publisher、カテゴリーなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な箇所で説明します。

Web サイトの初期実装は機能しますが、さらに機能を追加します。まずバックログを確認します。テンプレートからプロジェクトをセットアップしたときに issue のバックログが自動作成されています。GitHub Copilot に表示を依頼します。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** agent は自律的に計画し、複数ファイルにわたる変更を実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップ作成時に Claude Sonnet 4.5 を使用したため、体験の一貫性を高める目的で手順に記載しています。LLM は確率的に動作するため、ここに記載したものと異なる提案が返る場合があります。これは正常で、想定される動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP コマンドを使う場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトでバックログについて Copilot に尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、issue 一覧を取得するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーにとって有用だと判断した順に、項目を優先順位付けしたことを確認してください。

## インストラクションファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストとして使うインストラクションファイルを確認します。Web サイトの機能追加方法を説明する [user interface (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) ファイルを使用します。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の全般的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則
   - Astro、Tailwind CSS、Drizzle などのファイル形式に固有のインストラクションへのリンク

> [!TIP]
> インストラクションファイルから、別のインストラクションファイルとプロジェクト内のファイルの両方を参照できます。パスはインストラクションファイルからの相対パスです。指示の再利用、複雑な指示の分割、例やテンプレートの提示に役立ちます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の3つの更新が必要です。

- データレイヤー (`src/lib/`) の残りのフィルタリングロジックを追加または調整する。
- フィルタリング動作のテストを追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

さらに、コードベースにマージする前にテストを実行して成功を確認します。Copilot Agent Mode にこの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、インストラクションファイルを追加します。

   ![UI インストラクションファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 issue の要件に沿って機能を実装するよう、次のプロンプトを送ります。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関係するファイルを特定する様子を確認します。データレイヤーのヘルパー、UI、テストを探した後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調査するスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイル変更、テスト実行などを行います。タスクやコードベースの複雑さによって、数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** が表示される場合があります。作業中に選択する必要はありません。Copilot の完了後、変更ごとに **Keep** または **Undo** を選べます。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![テスト実行の確認を求める Copilot Chat ペインのダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、一部のテストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、エラーを検出しなくなるまで作業します。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューした後、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys Web サイトに機能を追加しました。次の方法を確認しました。

- プロジェクトを調査して関連ファイルを特定し、複数ファイルにまたがる変更を行いました。
- UI とデータレイヤーにまたがる新機能を実装しました。
- 変更と生成されたテストをレビューし、コードベースにマージする前に確認しました。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナスの探索演習 – ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだ内容を使って、Copilot にページネーションを実装するよう依頼してください。次の点を検討します。

- 既存のインストラクションファイルを使うなど、既存のベストプラクティスに従う。
- ユーザーがページサイズを選べるようにするか、固定値にするかを決める。
- 希望どおりに実装するために必要なガイダンスをプロンプトに含める。
- Copilot に変更を依頼し、コンテキストを提供しながら反復改善する。Copilot との通常の作業方法です。

## リソース

- [Copilot の ask、edit、agent modes: 各モードの役割と使いどころ][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
