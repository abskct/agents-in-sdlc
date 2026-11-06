---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションに簡単な更新を加える場合でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は、必要なファイルをすべて見つけ、変更を加え、テストを実行し、デバッグし、変更し忘れたファイルを探して、さらに更新するという作業を繰り返します。作業は次々と増えていきます。

このようなときに役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業するよう設計されています。まず既存のプロジェクト構造を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使って、サイトに新しい機能を追加する方法を確認しましょう。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを見つけ、複数のファイルにわたって連携した変更を行う方法。
- GitHub Copilot Agent Mode が UI とデータ層にまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途向けに設計されています。エージェント モードは IDE 内でタスクを実行するため、すばやいフィードバックとやり取りができます。Cloud agent はチーム メンバーのように非同期で作業し、issue やプルリクエストを通じて連携します。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。残りのデータ層、UI、テストの作業を Copilot Agent Mode と一緒に行います。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを調べ、現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示できます。Web サイトは単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページはフロントマターから Drizzle ORM を通じてローカル SQLite データベースを直接クエリします。個別のバックエンド API やクライアント側の UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能は標準の DOM API を使う小規模で限定的な Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習で `main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナル ウィンドウを開きます。
3. フィルター機能用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら Astro サーバーのアドレス `http://localhost:4321` を選択して、Web サイトを開きます。

> [!NOTE]
> codespace で localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは codespace に接続するプライベート トンネルであり、Web サーバーは codespace 上で実行されています。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明を含むボード ゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなど、詳しい情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。演習ごとにモードとモデルの選択が異なります。必要な選択は各手順で説明します。

Web サイトの初期実装は機能しますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトを設定すると、issue のバックログが自動的に作成されています。GitHub Copilot に issue の一覧を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントは、自律的に変更を計画して複数のファイルを実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップ作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。安定した学習体験のため、比較的一貫したコードの提案が得られることを期待しています。ただし、LLM の動作は確率的であるため、ワークショップの例とは異なる提案が表示されることがあります。これは通常の想定された動作です。

> [!NOTE]
> LLM の動作は確率的なため、Copilot が別の MCP コマンドを使用することがありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が利用者にとって役立つと思われる順に、項目を優先順位付けしたことを確認しましょう。

## 命令ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを与える命令ファイルを確認します。Web サイトへの機能追加の方法に関する情報を含む [ユーザー インターフェイス (UI) ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の方法に関する全般的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則。
   - ファイルの種類ごとの命令ファイルへのリンク。たとえば、次のとおりです。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 命令ファイルから別の命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルからの相対パスです。命令を再利用し、複雑な内容を管理しやすい小さなファイルに分け、例やテンプレートを示すことができます。

## フィルター機能を実装する

フィルター機能を完成させるには、アプリケーションに少なくとも次の 3 つの更新が必要です。

- データ層 (`src/lib/`) に残りのフィルター ロジックを追加または調整します。
- フィルター動作のテストを追加または更新します。
- ゲーム一覧ページを更新して、フィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行し、成功することを確認する必要があります。Copilot Agent Mode にこれらのタスクを任せましょう。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、命令ファイルを追加します。

   ![UI 命令ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. 次のプロンプトを使い、バックログの関連 issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを見つける様子を確認します。データ層のヘルパー、UI、テストを見つけた後、ファイルの変更とテストの実行を始めます。

   ![Copilot がプロジェクト ファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot がプロジェクトの調査、ファイルの変更、テストの実行などを行うことがわかります。タスクやコードベースの複雑さによって、数分かかることがあります。その間、コード エディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業が完了すると、すべての変更に対して **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot のプロンプトに従い、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテストの実行を確認するダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot は、タスクを完了してエラーを検出しなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新しい機能を確認します。
11. すべて動作し、コードのレビューも完了したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode が UI とデータ層にまたがる機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを見つけ、複数のファイルにわたって連携した変更を行う方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 — ページングを実装する

ゲームの一覧が増えると、ページングが必要になります。この演習で学んだスキルを使い、ページングを実装するよう Copilot に依頼してください。コードを作成する際は、次の点を検討します。

- 既存の命令ファイルを使うなど、既存のベスト プラクティスに従います。
- ページ サイズを利用者が選択できるようにするか、値を固定するかなど、ページングの方法を検討します。
- 希望どおりの実装に必要なガイダンスをプロンプトに含めます。
- 変更を依頼してコンテキストを追加しながら、GitHub Copilot とのやり取りを繰り返す必要がある場合もあります。これは Copilot と作業するときの通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 各モードの機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
