---
title: "レッスン 2 - 星評価を追加する: 手早い改善"
description: "GitHub Copilot app で最初のエージェント セッションを開始し、ゲーム カードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットを試したので、**エージェント セッション**を開始して、プロジェクトに最初の変更を加えます。小さな変更から始めます。ゲーム データにはすでに星評価がありますが、ホーム ページのゲーム カードには表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- エージェント セッションを開始し、セッションの構成を確認します。
- エージェントにプロジェクトの小さな変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホーム ページのゲーム カードにはタイトル、カテゴリ、パブリッシャー、説明しか表示されません。練習として、各カードに既存の評価を表示するようエージェントに依頼します。小規模で独立した変更なので、最初のセッションに適しています。

## セッションの構成

**セッション**は、専用の分離されたワークスペースでエージェントと会話する場所です。各セッションには**専用の git worktree とブランチ**が割り当てられます。そのため、一方では機能を追加し、もう一方ではバグを修正するなど、変更を競合させずに複数のセッションを同時に実行できます。セッションはサイドバーのリポジトリごとにグループ化され、選択すると切り替えられます。

セッション内には、エージェントとの**会話**、ファイルを調べて編集するエージェントの**ツール操作**、差分付きの**変更済みファイル**一覧が表示されます。

## セッションを開始して変更を依頼する

新しいセッションを開始してプロジェクトを確認し、機能を実装します。[アプリのセットアップ][prior-lesson]では、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用の新しいセッションを作成し、変更を依頼します。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバーの上部にある **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選択し、次のプロンプトを使って変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は、まずプロジェクトの分離されたコピーである新しい worktree を作成します。次にプロジェクトを調べ、新機能に必要な更新対象ファイルを見つけてコードを作成します。Copilot app を使って新機能を追加できました。

## 差分をレビューする

小さな変更であっても、AI が生成した変更はすべてマージ前にレビューします。Copilot app で変更を確認しましょう。

1. アプリの右上隅にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更されたファイルとその差分を確認します。

    ![Create PR ボタンの右側にある Toggle review panel ボタンを矢印で示した、GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲーム詳細の表示に使う主要ファイル `GameCard.astro` にコードが追加されていることを確認します。評価がある場合は表示し、`starRating` が `null` の場合は "No rating yet" に切り替える、次のような小さなブロックが追加されているはずです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> すべての生成 AI ツールと同様に、Copilot の動作は決定論的ではなく確率的です。実際のコードは上記と異なる場合がありますが、比較的よく似たものになります。

## 変更を確認する

ブラウザーを開く前に、エージェントが自動実行したチェックの結果をレビューします。数値の `starRating` と、`null` の場合の代替表示の両方をテストで確認していることを確かめます。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストールの要求は内容を確認してから承認します。

コードを読むだけで動作すると判断するのではなく、Copilot に Web サイトを起動させ、更新された UI を確認します。次のように依頼して、ブラウザーキャンバスで Web サイトを開きます。

> [!TIP]
> キャンバスは、Copilot app 内で使えるインタラクティブなウィジェットです。後のレッスンではカスタム キャンバスを確認して自分でも作成しますが、ここでは組み込みのブラウザーキャンバスを使います。

1. 次のプロンプトを使い、アプリを起動してブラウザーキャンバスでページを開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザー ウィンドウが開きます。
3. 評価済みのゲーム カードに 5 段階評価が表示されることを確認します。
4. 確認が終わったら、次のプロンプトを使い、このセッションで起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。新しいコードを既存のコードベースにマージするプルリクエスト (PR) を作成します。

1. 右上隅にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. リポジトリのポリシーにより、PR を作成する前に関連する issue が必要だと Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューペインでプルリクエストを開きます。ここで PR をレビューできます。
6. 準備ができたら **Ready to merge** を選択します。
7. 表示された新しいダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

GitHub Copilot app を使って、最初の変更を公開できました。具体的には、次のことを行いました。

- エージェント セッションを開始し、セッションの構成を確認しました。
- エージェントにゲーム カードの小さな変更を依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして明示的にマージしました。

次は[フィルター機能の issue から始め、Plan と Autopilot モードを使って][next-lesson]より大きな機能を作成します。

## リソース

- [GitHub Copilot app でエージェント セッションを使う][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
