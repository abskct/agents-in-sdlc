---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能の要望など、さまざまな課題があります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。これにより、手間のかかるタスクを任せて開発を加速し、開発者がより大きな課題に集中できるようになります。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。サイトの初期構築を担当した委託先は、ドキュメントを適切に整備していませんでした。実際、ドキュメントがまったくありません。まず、アプリケーションのすべてのエクスポートされた関数に TSDoc のドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを表示し、利用者が閲覧を続けられるようにします。まだ完成度の高い実装は必要ありません。UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度の高い issue がほかにあります。

どちらも、優先順位が下がりやすいタスクの例です。Copilot cloud agent に任せると、開発者がほかの作業に集中している間に非同期で処理できます。その後、戻って Copilot の作業をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。たとえば、[GitHub issue を Copilot に割り当て][assign-issue]ることができます。割り当てると、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境を設定してタスクに取り組み始めます。処理の途中でも完了後でも Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確に定めた命令の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな操作も、詳しく見ると複雑なことがあります。そのため、Copilot cloud agent に依頼するタスクの範囲を[慎重に定めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階を踏んで学び、試し、必要に応じて調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令と、Copilot への指示方法を確認しました。カスタム命令の演習をすでに完了していれば、`.github/copilot-instructions.md` が機能することを確認できています。まだの場合は、ここで簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる命令ファイルを読み取り専用で確認して、後でその影響を確認できるようにします。

リポジトリの GitHub Web UI、または起動済みの codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規則と TSDoc のドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この命令は `**/*.test.ts` ファイルに適用されます。

次のセクションで **Code lacks documentation** issue を cloud agent に割り当てたら、作成されるプルリクエストで TSDoc のドキュメント コメント、TypeScript の規則、ファイル コメント ヘッダーが使われているか確認してください。これらの命令ファイルの影響は、後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、多くの場合は特定の環境や、正常な状態にするためのセットアップ スクリプトが必要です。SWE と同様にタスクを実行する Copilot に作業を割り当てる場合も同じです。

Cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` ファイルに設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を使用できるようになります。このラボでは、学習をスムーズに進められるよう、あらかじめ設定してあります。このワークフローにより、単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシード済みの SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限のアクセス許可を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` アクセス許可が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
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

      # ビルドとテストでデータを利用できるように、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローと同じように見えますが、重要な点がいくつかあります。

- `copilot-setup-steps` という 1 つのジョブが含まれます。このジョブは、Copilot がプルリクエストの作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。これにより、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するのを待たずに、正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古いままか、そもそも存在しません。こうした技術的負債を放置すると、生産性が低下し、コードベースの保守や新しい開発者の受け入れが難しくなります。Copilot はドキュメントの作成を得意としているため、cloud agent に任せるのに適した issue です。バックグラウンドで必要なドキュメントを作成し、後の演習でその成果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てのダイアログを開きます。

  ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に新しい PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は前述のワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む方法を決めています。

13. todo リストと、実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールして、Copilot がどのように問題を解決しているか確認します。コードを調べて状態を把握する作業、計画を考えて決定するために一時停止する場面、コードを作成する様子を確認できます。

処理には数分かかることがあります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期に実行し、ほかの作業に集中できるようにすることです。この機能を活用するため、もう 1 つのタスクを Copilot cloud agent に割り当てた後、アプリケーションの機能を追加するコードの作成に取り組みます。

## ゲームの詳細ページに関連ゲームのセクションを追加する

前述のとおり、GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分担できることです。デザイン チーム向けに関連ゲームのセクションを追加する作業は時間がかからないかもしれませんが、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
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
8. 右側にある **Assign to Copilot** を選択して、割り当てのダイアログを開きます。

  ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Copilot が目の絵文字で作業中であることを示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent は SWE と同じように作業するため、常に監視する必要はありません。完了後にレビューできます。続いて、カスタム エージェントの作成と使用を確認しましょう。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に issue を割り当てると、非同期で作業を実行できます。技術的負債の解消、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などを Copilot に任せることができます。

次の概念を確認しました。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業する間、次はカスタム エージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。また、前のモジュールで確認したカスタム命令も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
