---
title: "演習 1 - カスタムインストラクション (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクの実行方法に決まりがある場合や、Copilot に背景情報を伝える必要がある場合は、その情報にアクセスできるようにします。[インストラクションファイル][instruction-files]を使って指針を伝えれば、Copilot は何をするかだけでなく、どのように実行するかも理解できます。

この演習では、次のことを行います。

- リポジトリのカスタムインストラクションとパスを対象とするインストラクションファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの「変更前/変更後」を実行しません。Copilot cloud agent は GitHub issue を使って非同期に動作するため、その影響をリアルタイムで並べて示すのは困難です。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、インストラクションファイルの影響を確認します。

## インストラクションファイル

### シナリオ

Tailspin Toys には、開発の実践方法に関するガイドラインと要件があります。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードで、現代的なデザインにします。
- TSDoc doc comment の形式でコードにドキュメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

インストラクションファイルを使い、Copilot がこれらの実践方法に沿ってタスクを実行できるようにします。

### カスタムインストラクション

カスタムインストラクションを使うと、コーディングスタイルや要件のコンテキストと好みを Copilot に伝えられます。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定し、より適切な提案やコードスニペットが得られるようにできます。リポジトリ全体、またはタスクに応じた特定の種類のファイルにインストラクションを作成できます。

インストラクションファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する **すべて** のリクエストで Copilot に送信される単一のインストラクションファイルです。Copilot に送るチャットや CLI のリクエストの大半に関係する、プロジェクトレベルの情報を記述します。技術スタック、開発対象の概要、ベストプラクティス、全体に適用する指針などが該当します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に対するインストラクションです。TypeScript や Astro などの言語、UI コンポーネントやユニットテストの作成などのタスクについてガイドできます。

> [!NOTE]
> 統合開発環境 (IDE) で作業する場合、インストラクションファイルがコード生成に使われるのは Copilot Chat のみです。コード補完や次の編集の提案には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルのインストラクションと、`applyTo` フロントマターを含む `*.instructions.md` ファイルの両方をコード生成に使います。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、[他の標準形式のインストラクションファイル][custom-instructions-support]もサポートしています。

### インストラクションファイルを管理するためのベストプラクティス

インストラクションファイル作成の全容はこのワークショップの範囲外ですが、サンプルプロジェクトには一般的なアプローチの例があります。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、開発対象の説明、プロジェクト構成、全体的なコーディング標準など、プロジェクトレベルの指針を記載します。
- `*.instructions.md` には、ファイルの種類 (ユニットテスト、Astro コンポーネント、データレイヤーなど) や特定のタスクに関する指針を記載します。
- 自然な言葉で、明確な指針とコードの例を示します。望ましい例と望ましくない例の両方が役立ちます。

インストラクションファイルの作成方法にも、AI の使い方にも唯一の正解はありません。実験を重ねて、プロジェクトに適した方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実したインストラクションファイル一式を用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、多様なタスク向けのファイルがあります。
>
> Copilot にインストラクションファイルを生成させることもできます。各環境で操作方法は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`)。該当する環境のレッスンで説明します。
>
> テンプレートや出発点を探す場合は、インストラクションファイル、カスタムエージェント、その他のリソースが集まったリポジトリ [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタムインストラクションファイルを確認する

このリポジトリに含まれるインストラクションファイルを確認します。コアとなる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスにも注目してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データレイヤー、テストなどのインストラクションがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、インストラクションの適用対象をリポジトリルートからの glob で指定します。ここでは、`**/*.test.ts` などに一致する TypeScript テストファイルが対象です。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別のインストラクションファイルや、プロジェクト内の既存ファイルへのリンクを確認してください。大きなインストラクションを小さな再利用可能なファイルに分けたり、コード生成時の例を Copilot に示したりできます (ここでのパスはリポジトリルートではなく、インストラクションファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で、TSDoc doc comment とファイルコメントヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は、コード内に TSDoc doc comment とファイルレベルのコメントヘッダーを記載するというプロジェクト全体のルールを追加することです。後続の演習で issue を割り当てるときに、cloud agent がこのルールを参照します。

編集前に作業ブランチを作成します (cloud agent は issue の対象ブランチから指示を読み取りますが、ここでは変更をきちんとコミットします)。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。リポジトリのコーディング標準にルールを加え、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはまだありません。そのため、生成されたヘルパーに doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加して、ファイルコメントヘッダーと TSDoc doc comment の追加を Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、インストラクションファイルはリポジトリ全体に適用する `.github/copilot-instructions.md` や、特定の言語、ファイルの種類、タスク向けの `*.instructions.md` として作成できます。追加した doc comment ルールのようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## インストラクションの変更をコミット、プッシュ、マージする

Cloud agent は issue の対象ブランチにあるインストラクションファイルを読み取ります。次の演習で Copilot に issue を割り当てるとき、`main` からブランチが作成されます。そのため、cloud agent が変更後の指示を使えるように、変更を `main` に反映しておきます。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で issue に割り当てたとき、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチ作成を省略して `main` にコミットしてもかまいません。ここでは実際のプロジェクトでの進め方に合わせてブランチを使います。

## まとめと次のステップ

このプロジェクトで Copilot がインストラクションファイルからコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半で、cloud agent が作成するプルリクエストに標準が反映される様子を確認します。

次は、[Copilot cloud agent に最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使うインストラクションファイル][instruction-files]
- [カスタムインストラクションを作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタムインストラクションを作成する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — インストラクションファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
