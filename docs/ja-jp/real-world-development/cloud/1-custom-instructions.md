---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、実行してほしいことだけでなく、その方法も Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパスに応じた命令ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメントの標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue に対して非同期に動作するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、命令ファイルの影響を確認します。

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

## リポジトリ全体に適用する標準を追加する

ここで編集するのは、コード内に TSDoc のドキュメント コメントとファイルレベルのコメント ヘッダーを追加する、プロジェクト全体の規則です。cloud agent がこのハーネスの後半で割り当てられた issue に取り組むとき、この規則を参照します。

編集する前に、作業用のブランチを作成します。cloud agent は issue の対象ブランチから命令を読み込みますが、変更を整理してコミットするためにもブランチを使います。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 命令の変更をコミット、プッシュ、マージする

Cloud agent は、issue の対象ブランチにある命令ファイルを読み込みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。命令を cloud agent に適用するには、変更を `main` に反映する必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で issue を割り当てたとき、cloud agent がこの命令を読み込めるようになります。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチの作成を省略して `main` に直接コミットできます。ブランチを使うのは、実際のプロジェクトでの作業方法を再現するためです。

## まとめと次のステップ

このプロジェクトで Copilot が命令ファイルからコンテキストを取得する方法を確認し、リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で cloud agent が作成するプルリクエストに、この標準が反映されます。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム命令をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
