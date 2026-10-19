---
title: "レッスン 4 - カスタムインストラクションで Copilot を導く"
description: "リポジトリのインストラクションを確認し、ドキュメント標準を追加して、フィルタリングコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクに特定の実行方法がある場合、その指針を Copilot が利用できるようにします。[インストラクションファイル][instruction-files]では、必要なコードだけでなく、コードの構成方法も指定できます。フィルタリング機能を作成したので、Copilot が使用したインストラクションを確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリのインストラクションとパスを対象とするインストラクションファイルがエージェントに伝わる仕組みを確認します。
- コーディング標準に従うため、インストラクションファイルを更新します。
- インストラクションファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発の実践方法に関するガイドラインと要件があります。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数では、存在する場合は注入可能な `db` 引数も含め、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を記載し、関連コードの変更に合わせてコメントを最新の状態に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

インストラクションファイルを使い、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

## インストラクションファイル

カスタムインストラクションを使うと、コーディングスタイルや要件のコンテキストと好みを Copilot に伝えられます。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定し、より適切な提案やコードスニペットが得られるようにできます。リポジトリ全体、またはタスクに応じた特定の種類のファイルにインストラクションを作成できます。

インストラクションファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する **すべて** のリクエストで Copilot に送信される単一のインストラクションファイルです。大半のリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイダンスを提供します。

> [!NOTE]
> 他のインストラクション形式やそのサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタムインストラクションのサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタムインストラクションファイルを確認する

スタータープロジェクトにはインストラクションファイルが含まれています。変更を加える前に内容を確認し、変更による影響を見られるようにします。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。このリポジトリで Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどのインストラクションがあります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。glob でインストラクションの適用対象を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他のインストラクションファイルや既存プロジェクトファイルへのリンクを確認します。大きな指示を小さな再利用可能なファイルに分け、例を Copilot に示せます。

## チームのガイダンスに合わせてインストラクションファイルを更新する

既存ファイルは良い出発点ですが、まだ不足があります。コアの `copilot-instructions.md` を変更し、新たに生成する TypeScript に TSDoc コメントが追加されるようにします。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。35行目付近にあります。
2. 最後の箇条書きとして次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリのインストラクションを読み込みます。機能のコンテキストを保ちながら更新後のガイダンスを使うため、インストラクションを保存した後にフィルタリングの会話を終了して再開します。

1. フィルタリングの会話が開いている場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリルートの `game-filters-cli` ブランチから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたらフィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトで、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードの内容を正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI がインストラクションファイルからコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリのインストラクションとパスを対象とするインストラクションファイルがエージェントに伝わる仕組みを確認しました。
- コーディング標準に従うため、インストラクションファイルを更新しました。
- インストラクションファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタムインストラクションを追加する][instruction-files]
- [カスタムインストラクションのサポート][custom-instructions-support]
- [カスタムインストラクションを作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — インストラクションファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
