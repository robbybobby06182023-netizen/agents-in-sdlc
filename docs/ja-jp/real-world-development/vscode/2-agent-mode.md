---
title: "演習 2 - Copilot agent mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な変更でも、複数のファイルの編集やテスト実行などが必要になることがあります。開発者は必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグして、変更漏れに気づき、さらに修正する、といった作業を行います。

このような作業に役立つのが Copilot Agent Mode です。

Copilot Agent Mode は IDE でより自律的に動作します。開発者と同様に、プロジェクト構成を調べ、必要なファイルを変更し、テストなどのタスクを実行して、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる体験を提供します。Agent mode は IDE 内で動作するため、すばやいフィードバックとやり取りが可能です。Cloud agent は Issue とプルリクエストを通じてチームメンバーのように非同期で作業します。

この演習では、次の内容を学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定して、複数の変更を調整する方法。
- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリーと発行元で絞り込めるようにします。前の演習では発行元ヘルパーを追加しました。ここでは、Copilot Agent Mode を使って残りのデータレイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

Web サイトは、開発者向けテーマのボードゲーム用クラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。サイトは単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな動作には標準 DOM API を使う小さな Astro `<script>` を使います。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら `http://localhost:4321` を選択します。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web サーバーをホストしている Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。

- **ホームページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリーなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを作成すると、Issue のバックログも自動的に作成されています。GitHub Copilot に一覧を表示させます。

1. Chat ビューのエージェント一覧で **Agent** を選択します。Agent は自律的に計画を立て、複数のファイルを変更し、ターミナルコマンドを実行して、ツールを呼び出します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨するものではありません。このワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高めることが目的ですが、LLM は確率的に動作するため、例とは異なる提案が返る場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは同様に完了できます。

3. 次のプロンプトを Copilot に送信して、バックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての Issue を一覧表示するコマンドを実行するため **Continue** を選択します。
5. 生成された Issue の一覧を確認します。

Copilot が、利用者にとって有用だと考えた順に項目を優先付けしたことを確認してください。

## 指示ファイルを確認する

コード生成を開始する前に、Copilot にコンテキストを与える指示ファイルを確認します。Web サイトへの機能追加方法に関する情報を含む [UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. アーキテクチャの概要、コンポーネント設計・テスト容易性・アクセシビリティの原則、Astro、Tailwind CSS、Drizzle などのファイル種類ごとの指示へのリンクを確認します。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、複雑な内容を小さく分け、Copilot に例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリング機能の完成には、少なくとも次の 3 つの変更が必要です。

- `src/lib/` に残りのフィルタリングロジックを追加または改善する。
- フィルタリング動作のテストを追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

コードベースにマージする前にテストを実行して合格する必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、UI 指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送ります。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを特定します。データレイヤーのヘルパー、UI、テストを見つけ、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など、複数の作業を行います。タスクやコードベースによって数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** が表示される場合があります。完了するとすべての変更に対する選択肢が表示されます。作業中に選ぶ必要はありません。

7. Copilot から求められたら、テストを実行するため **Continue** を選びます。

   ![Copilot Chat ペインにテスト実行の確認ダイアログが表示されたスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、エラーがないと判断するまで修正します。

   ![Copilot Agent Mode のチャットセッション完了画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys に新機能を追加する方法を学びました。

- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる機能を実装する。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定して変更を調整する方法を確認する。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 — ページ分割を実装する

ゲームの一覧が増えるとページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割の実装を依頼してみましょう。既存の指示ファイルなどのベストプラクティスに従い、利用者がページサイズを選べるようにするか固定値にするかを検討します。希望どおりの実装に必要なガイダンスをプロンプトに含めてください。Copilot に変更やコンテキストを伝えながら反復することもあります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
