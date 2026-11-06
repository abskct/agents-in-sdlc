---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、ユーザー エクスペリエンスの改善と機能追加を目的に、Copilot cloud agent に 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業するよう設計されていますが、タスクの進行状況を監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。たとえば、GitHub.com の[エージェント ページ][agents-page]を使います。このミッション コントロールでは、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行された操作を調べたり、進行中のセッションに追加の指示を与えたりすることもできます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼し、作業の方向を調整します。

## シナリオ

ハイ コントラスト モードの作成をエージェントに依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイルを更新し、切り替え機能を追加しているため、一緒に実装するのが適切です。エージェントに追加の指示を与え、ハイ コントラストに加えてライト モードも追加するようにします。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左側のペインにあるタスク一覧を確認します。Copilot に割り当てた次のようなタスクが表示されます。
    - コードベースのドキュメントの更新。
    - ゲームを変更する API の生成。
    - Web サイトへのハイ コントラスト モードの追加。
3. 実行中のタスクを 1 つ選択し、Copilot が実行したタスクを確認します。次のような操作が含まれます。
    - リポジトリからコードをチェックアウトする。
    - Copilot が作業する環境を作成する。
    - MCP サーバーを設定する。
    - 割り当てられたタスクを完了するためのさまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認してください。PR と変更されたファイルを確認し、作業を監視できます。

## Cloud agent に追加の指示を与える

実行中のタスクを確認したので、ハイ コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードの追加に関連するセッションを選択します。正確なタイトルは、Copilot が付けた名前や作業状況によって異なります。

    ![ミッション コントロールに表示されたアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Cloud agent のタスク画面で、作業中のセッションに指示するダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押して、プロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映することを確認します。

## Copilot の作業を待つ

前と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で進めていた手順が終わると、新しい依頼が作業に反映されます。

今回も完了まで数分かかります。その間に休憩して、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して方向を調整するための中心的な場所である、Copilot agents ページを確認しました。このミッション コントロールを使うと、実行中および完了したタスクを追跡し、作業内容をレビューして、進行中のタスクに追加の指示を与えられます。

次の概念を確認しました。

- ミッション コントロールとエージェント ページで cloud agent のタスクを監視する方法。
- 進行中のセッションに追加機能を依頼する方法。

Copilot がアクセシビリティ機能を実装する間に、次は Copilot が作成したプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
