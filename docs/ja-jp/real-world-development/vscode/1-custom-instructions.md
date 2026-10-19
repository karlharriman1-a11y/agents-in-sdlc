---
title: "演習 1 - カスタムインストラクション (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクの実行方法に決まりがある場合や、Copilot に背景情報を伝える必要がある場合は、その情報にアクセスできるようにします。Copilot に具体的なコンテキストを伝える方法はいくつかあります。特に重要なのが[インストラクションファイル][instruction-files]です。コード生成に関する指針を提供します。

この演習では、次のことを行います。

- リポジトリのカスタムインストラクションとパスを対象とするインストラクションファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在のインストラクションのもとで、フィルタリング機能の最初のデータスライス (publisher ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映される様子を確認します。
- 次の演習で Copilot が更新済みの指針を使えるよう、インストラクションの更新とフィルタリングスライスを `main` にコミットします。

> [!CAUTION]
> 生成コードは、指定した標準から逸脱する場合があります。Copilot の出力は確定的ではありません。この演習では、出力の文字単位での一致ではなく、インストラクションを更新した後に動作がどのように変わるかを確認します。

## インストラクションファイル

### シナリオ

Tailspin Toys には、開発の実践方法に関するガイドラインと要件があります。

- データレイヤーには必ずユニットテストを用意します。
- ユーザーインターフェイス (UI) はダークモードで、現代的なデザインにします。
- TSDoc doc comment の形式でコードにドキュメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

インストラクションファイルを使い、Copilot がこれらの実践方法に沿ってタスクを実行できるようにします。

### カスタムインストラクション

カスタムインストラクションを使うと、コーディングスタイルや要件のコンテキストと好みを Copilot に伝えられます。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定し、より適切な提案やコードスニペットが得られるようにできます。リポジトリ全体、またはタスクに応じた特定の種類のファイルにインストラクションを作成できます。

インストラクションファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する **すべて** のリクエストで Copilot に送信される単一のインストラクションファイルです。Copilot に送る大半のチャットや CLI リクエストに関係するプロジェクトレベルの情報を記載します。技術スタック、開発対象の概要、ベストプラクティス、全体に適用する指針などが該当します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に対するインストラクションです。TypeScript や Astro などの言語、UI コンポーネントやユニットテストの作成などのタスクについてガイドできます。

> [!NOTE]
> IDE で作業する場合、インストラクションファイルがコード生成に使われるのは Copilot Chat のみです。コード補完や次の編集の提案には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルのインストラクションと `applyTo` フロントマターを含む `*.instructions.md` ファイルの両方をコード生成に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[他の標準形式のインストラクションファイル][custom-instructions-support]もサポートしています。

### インストラクションファイルを管理するためのベストプラクティス

インストラクションファイル作成の全容はこのワークショップの範囲外ですが、サンプルプロジェクトには一般的なアプローチの例があります。

- `copilot-instructions.md` には、開発対象の説明、プロジェクト構成、全体的なコーディング標準など、プロジェクトレベルの指針を記載します。
- `*.instructions.md` には、ファイルの種類 (ユニットテスト、Astro コンポーネント、データレイヤーなど) や特定のタスクに関する指針を記載します。
- 自然な言葉で、明確な指針とコードの例を示します。望ましい例と望ましくない例の両方が役立ちます。

インストラクションファイルの作成方法にも、AI の使い方にも唯一の正解はありません。実験を重ねて、プロジェクトに適した方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実したインストラクションファイル一式を用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、多様なタスク向けのファイルがあります。
>
> Copilot にインストラクションファイルを生成させることもできます。各環境で操作方法は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`)。該当する環境のレッスンで説明します。
>
> テンプレートや出発点を探す場合は、インストラクションファイル、カスタムエージェント、その他のリソースが集まった [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタムインストラクションファイルを確認する

スタータープロジェクトに含まれるインストラクションファイルを確認してから変更を加え、影響を比較します。

1. 前の演習で使った codespace に戻ります。
2. Codespaces エディター (ターミナルではありません) で `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。これらは、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関するインストラクションがあります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。glob でインストラクションの適用対象を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他のインストラクションファイルや既存のプロジェクトファイルへのリンクを確認します。大きな指示を小さな再利用可能なファイルに分け、例を Copilot に示せます。

## 変更用のブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## インストラクションを更新する**前に** Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な箇所で説明します。

カスタムインストラクションの影響を確認するため、まず現在のインストラクションファイルの状態でプロンプトを送信します。後でインストラクションを更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. Copilot がヘルパーの配置先を把握できるよう、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査し、コードを適用します。ヘルパーファイルとテストの両方に変更が及ぶ場合があります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取る型付き関数であり、publisher の型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案コードに TSDoc doc comment とファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。インストラクションを更新した後の一貫性向上が、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コーディング標準にルールを加えて、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを探します。ここにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。そのため、生成されたヘルパーには doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、インストラクションファイルはリポジトリ全体に適用する `.github/copilot-instructions.md` や、特定の言語、ファイルの種類、タスク向けの `*.instructions.md` として作成できます。追加した doc comment ルールのようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` をクリックし、Copilot に対象ファイルを認識させます。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で拡張する実際のコードに適用できました。

## インストラクションとブランチをコミットしてプッシュする

インストラクションファイルも、他のリポジトリアセットと同じようにソース管理します。変更をコミットしてブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、インストラクションとヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. インストラクションの変更と、ヘルパーの基盤作成で Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (テストなど) が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。手動でもできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼できます。issue を見つけ、issue を完了する PR を作成するようプロンプトを送ります。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクトテンプレートでは GitHub のツールがワークスペースに接続済みなので、セットアップは不要です。Copilot が初めてツールを使用するとき、VS Code から GitHub へのサインインを求められる場合があります。指示に従って許可してください。後の演習で、この接続の仕組みである Model Context Protocol (MCP) を学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. インストラクションファイル更新に関する issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR にはインストラクションの更新と、新しい publisher ヘルパーの基盤作成の両方を記載します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot が GitHub 操作の **Allow** を求めた場合は、コマンドを確認して適切に許可します。
6. PR の作成後、次のプロンプトで PR をマージし、ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作とシェルコマンドの **Allow** を求めた場合は、コマンドを確認して適切に許可します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot がインストラクションファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存インストラクションのもとでコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案コードに新しい標準が反映されることを確認しました。
- 次の演習で使えるよう、インストラクションとヘルパーの基盤を `main` にコミットしました。

次は、[agent mode][next-lesson]でインストラクションを活用し、Copilot にコードベース全体の新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使うインストラクションファイル][instruction-files]
- [カスタムインストラクションを作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタムインストラクションを作成する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタムインストラクション][personal-instructions]
- [Awesome Copilot — インストラクションファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
