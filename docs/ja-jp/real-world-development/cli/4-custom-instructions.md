---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメントの標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合、そのガイダンスを Copilot が利用できるようにします。[命令ファイル][instruction-files]では、必要なコードだけでなく、その構造も指定できます。フィルター機能を作成したので、Copilot が使った命令を確認し、ドキュメントの標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの命令とパスに応じた命令ファイルがエージェントに適用される方法を確認します。
- コーディング標準に従うよう命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントはコードの内容を繰り返すのではなく、意図や自明でない決定を説明する必要があります。
- `db/` と `src/lib/` にあるエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc が必要です。`db` 引数を注入する場合は、その引数についても記載します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連するコードの変更時にはコメントを最新の状態に保つ必要があります。
- 既存の書式設定と lint のガイダンスを維持する必要があります。

命令ファイルを使い、ここで取り上げた開発プラクティスに沿ってタスクを実行するための情報を Copilot に伝えます。

## 命令ファイル

カスタム命令を使うと、コンテキストや好みを Copilot に伝え、コーディング スタイルや要件を理解しやすくできます。これは、関連性の高い提案やコード スニペットを得るために役立ちます。好みのコーディング規則やライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体に適用する命令も、特定のファイルやタスクに適用する命令も作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一の命令ファイルです。大半のリクエストに関連するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定の言語、ファイルの種類、タスクに関するガイドラインを記載します。

> [!NOTE]
> ほかの形式の命令ファイルとそのサポート状況は、環境によって異なります。特定の形式を使う前に、[カスタム命令のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

作業を始めやすいよう、スターター プロジェクトには命令ファイルが含まれています。変更を加えたときの影響を確認する前に、既存のファイルを調べましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この命令は、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、命令を適用するファイルを glob で指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルや既存プロジェクト ファイルへのリンクを確認します。大きな命令セットを再利用可能な小さなファイルに分け、コード生成の例を Copilot に示すことができます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存の命令ファイルは役に立ちますが、まだ足りない部分があります。中核となる `copilot-instructions.md` ファイルを変更し、新しく生成される TypeScript に TSDoc コメントが追加されるようにします。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** という見出しのセクションを見つけます。35 行目付近にあります。
2. 次の項目を最後に追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話を始めるときにリポジトリの命令を読み込みます。更新した命令を保存したら、フィルター機能の会話を終了して再開します。これにより、機能のコンテキストを保ったまま、新しいガイダンスを読み込めます。

1. フィルター機能の会話が開いている場合は、`/exit` と入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチにあるリポジトリのルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は、フィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のように Copilot に依頼して、更新したガイダンスを適用します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` と入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの命令とパスに応じた命令ファイルがエージェントに適用される方法を確認しました。
- コーディング標準に従うよう命令ファイルを更新しました。
- 命令ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI のカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
