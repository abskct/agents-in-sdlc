---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完全なアプリケーション、共有バックログを通して、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。計画、コード作成、動作テスト、プルリクエストのレビュー、エージェントを使った作業管理を繰り返しながら進めます。

## シナリオ

開発者をテーマにしたボードゲームのクラウドファンディングを提供する架空の会社、Tailspin Toys に新しい開発者として参加します。チームのバックログには、すぐに取り組める GitHub issue が登録されています。フィルタリングやページネーションなどの機能開発に加えて、アクセシビリティやコーディング規約など、品質を向上させる作業も含まれます。

## 環境を選ぶ

GitHub Copilot は、どの環境でも利用できます。自分の開発スタイルに合った環境を選んでください。各ワークショップでは、それぞれに必要なセットアップを行い、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

デスクトップアプリで複数のエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業し、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業を案内し、進行状況を監視して、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
