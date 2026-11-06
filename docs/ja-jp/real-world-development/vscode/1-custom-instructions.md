---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。その中でも重要なのが[命令ファイル][instruction-files]です。コード生成の指針を伝えるために使います。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパスに応じた命令ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメントの標準を Copilot に伝える方法を確認します。
- 現在の命令を使って、フィルター機能の最初のデータ部分 (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトを再実行し、生成されたコードが新しい標準に従うことを確認します。
- 次の演習で Copilot が最新のガイダンスを使えるよう、命令の変更とフィルター機能の一部を `main` にコミットします。

> [!CAUTION]
> 生成されたコードが、指定した標準の一部に従わないことがあります。Copilot の動作は確率的です。この演習の目的は、命令を更新した後に動作がどう変わるか、その**傾向**を確認することです。出力が一字一句同じになることではありません。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。たとえば、次のとおりです。

- データ層には常に単体テストが必要です。
- UI はダーク モードで、現代的な見た目にする必要があります。
- コードには TSDoc のドキュメント コメントを追加する必要があります。
- 各ファイルの先頭には、そのファイルの役割を説明するコメント ブロックを追加する必要があります。

命令ファイルを使い、ここで取り上げた開発プラクティスに沿ってタスクを実行するための情報を Copilot に伝えます。

### カスタム命令

カスタム命令を使うと、コンテキストや好みを Copilot に伝え、コーディング スタイルや要件を理解しやすくできます。これは、関連性の高い提案やコード スニペットを得るために役立ちます。好みのコーディング規則やライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体に適用する命令も、特定のファイルやタスクに適用する命令も作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一の命令ファイルです。Copilot に送るチャットまたは CLI リクエストの大半に関連するプロジェクトレベルの情報を記載します。たとえば、技術スタック、開発中の内容の概要、ベスト プラクティス、全般的なガイダンスなどです。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に関するガイドラインを記載します。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの作成といったタスクに使用できます。

> [!NOTE]
> IDE で作業する場合、命令ファイルがコード生成に使われるのは Copilot Chat のみです。コード補完や次の編集の提案には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの命令ファイルと `applyTo` フロントマターがある `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、別の標準に基づく命令ファイルも[サポートしています][custom-instructions-support]。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成について詳しく扱うことは、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法を示す例が含まれています。基本的には、次のようにします。

- `copilot-instructions.md` には、開発中の内容、プロジェクトの構造、全般的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- 単体テスト、Astro コンポーネント、データ層など、特定のファイルの種類やタスクに関する命令には `*.instructions.md` ファイルを使います。
- 自然な言葉で、明確なガイダンスを書きます。コードの良い例と悪い例を示します。

命令ファイルの作成に唯一の正解はありません。AI の使い方と同様に、プロジェクトで試行錯誤しながら最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した命令ファイルを用意することをお勧めします。このプロジェクトの命令ファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあることがわかります。
>
> Copilot に命令ファイルを作成させることもできます。各画面で方法は異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。利用中の画面に関係するレッスンで、該当する方法を説明します。
>
> テンプレートや出発点を探していますか。[awesome-copilot][awesome-copilot]では、命令ファイルやカスタム エージェントなど、さまざまなリソースを確認できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリに含まれる命令ファイルを確認します。中核となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドを確認してください。このフィールドは、命令が適用されるファイルをリポジトリのルートからの glob で指定します。たとえば `**/*.test.ts` に一致するすべての TypeScript テスト ファイルが対象です。
5. このプロジェクトで単体テストを作成する際に適用される命令を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの命令ファイルやプロジェクト内の既存ファイルへのリンクがあります。大きな命令セットを再利用可能な小さなファイルに分け、コード生成の例を Copilot に示すことができます。ここにあるパスはリポジトリのルートではなく、命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーの規則を追加します。

## 変更用ブランチを作成する

ベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。演習ごとにモードとモデルの選択が異なります。必要な選択は各手順で説明します。

カスタム命令の効果を確認するため、まず現在の命令ファイルを使ってプロンプトを送信します。その後、命令を更新して同じプロンプトを再送信し、違いを確認します。

1. Copilot が必要なコンテキストだけを使うよう、前の演習で開いたエディター タブを閉じます。
2. Copilot にヘルパーの場所を知らせるため、`src/lib/publishers.ts` を開きます。
3. Copilot がファイルを変更できるよう、Chat ビューのエージェント ドロップダウンから **Agent** を選択します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、コードを更新します。多くの場合、ヘルパー ファイルとテストの両方が変更されます。
6. 提案されたヘルパーは、最初の引数として `db` クライアントを受け取り、型付きの publishers 配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規則に基づいています。
7. 提案されたコードには、TSDoc のドキュメント コメントとファイルレベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確率的なため、指示がなくてもドキュメント コメントが追加されることがあります。その場合も問題ありません。命令を更新した後、一貫性が向上するかを確認することが目的です。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供するファイルです。コードの提案を改善するために、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する規則はありません。そのため、生成されたヘルパーにはドキュメント コメントがありませんでした。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc のドキュメント コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリレベルの `.github/copilot-instructions.md` は全般的なガイダンスに、`*.instructions.md` ファイルは特定の言語、ファイルの種類、タスクに関するガイダンスに使用できます。今回追加したドキュメント コメントの規則のような、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. Copilot が対象ファイルに集中するよう、`src/lib/publishers.ts` を選択します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を伝え、次の演習で使う実際のコードに適用できました。

## 命令をコミットしてブランチをプッシュする

命令ファイルもリポジトリのほかのファイルと同様に、ソース管理で管理します。命令の変更をコミットし、ブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナル ウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、命令とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで命令の変更と、ヘルパーの基盤のために Copilot が変更したすべてのファイルをステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (たとえばテスト) が表示される場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。手動で作成することもできますが、接続済みの GitHub ツールを使って Copilot に依頼できます。issue を検索して、issue をクローズする PR を作成し、マージするよう指示します。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートには GitHub のツールがあらかじめ接続されています。そのため、セットアップは不要です。Copilot が初めてツールを使用するとき、VS Code で GitHub へのサインインが求められることがあります。画面の指示に従ってください。次の演習では、この接続の仕組み (Model Context Protocol) を学びます。

1. codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. 命令ファイルの更新に関係する issue を検索し、現在のブランチから PR を作成するよう Copilot に依頼します。PR には命令の変更と新しい publishers ヘルパーの基盤の両方を記載します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を検索して PR を作成します。
5. GitHub の操作を実行するために Copilot が **Allow** を求めたら、コマンドを確認して適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトを使って PR をマージし、ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作とシェル コマンドの実行に対して **Allow** を求めたら、コマンドを確認して適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が命令ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- コード生成のプロンプトを送り、*既存の*命令を使って Copilot が生成する内容を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認しました。
- 次の演習で作業を引き継げるよう、命令の変更とヘルパーの基盤を `main` にコミットしました。

次は、Copilot がコードベース全体に新機能を追加するときに、[エージェント モードで命令を活用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム命令をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
