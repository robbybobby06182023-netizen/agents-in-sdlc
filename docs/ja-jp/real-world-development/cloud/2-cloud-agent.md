---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残っている機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加を自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。時間のかかるタスクを任せることで、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成環境のカスタマイズ。
- 操作を安全に行う方法。
- 範囲が明確な Issue の重要性。
- Issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを開発した委託先がドキュメントを十分に作成しなかったため、まず、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーのほかのゲームを表示し、利用者が閲覧を続けられるようにします。まず受け入れテストで UX を確認できる程度の実装を希望しています。この作業は現在ブロッカーですが、ほかに優先度の高い Issue があります。

どちらも、優先度が下がりやすいタスクの例です。Copilot cloud agent に割り当てれば、開発者がほかの作業を進めている間に非同期で処理し、完了後に結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業します。[GitHub Issue を Copilot に割り当てる][assign-issue]などの方法で作業を開始できます。割り当てると、Copilot は進行状況を追跡するドラフト PR を作成し、環境を準備して、タスクに取り組みます。作業中または完了後にセッションを確認でき、解決案をレビューする準備ができると、プルリクエストで通知されます。

## 範囲が明確な指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、適応させます。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot のガイダンスへの適用方法を確認しました。カスタム指示を使ったことがあれば `.github/copilot-instructions.md` が機能する様子を見ています。まだ確認していない場合は、Copilot に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認してください。後で結果にどのように影響したかを確認できます。

実行中の Codespace、またはリポジトリの GitHub Web UI で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript 規約と TSDoc ドキュメントコメントに関する項目を確認します。
- `.github/instructions/unit-tests.instructions.md`: `**/*.test.ts` ファイルに適用される `applyTo` フロントマターを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成された PR に TSDoc コメント、TypeScript 規約、コメントヘッダーがあるかを確認します。これらの内容は指示ファイルに基づいています。後の演習でも、PR のレビュー時に確認します。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、通常は環境の準備とセットアップスクリプトが必要です。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てる場合も同じです。

Cloud agent は [GitHub Actions][github-actions] 環境で動作します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を準備できます。この演習では、学習を進めやすくするため、あらかじめ設定されています。単一の Astro アプリ用に Node.js、プロジェクト依存関係、E2E テスト用ブラウザー、移行・シード済み SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は `contents: read` 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
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

      # ビルドとテストでデータを使えるよう、ローカル SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルですが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブがあり、Copilot が PR 作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、Copilot に実行させるのを待たずに、リポジトリの **Actions** タブからワークフローを手動実行してテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古い、または不足しているプロジェクトは少なくありません。こうした技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成が得意なので、cloud agent に依頼するのに適した作業です。バックグラウンドで必要なドキュメントを作成します。後の演習で結果を確認します。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。まだ一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> Copilot が PR に TODO リストを表示するまで数分かかる場合があります。前述のワークフローを実行して環境を作り、プロジェクトを分析して、作業手順を検討しているためです。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ウィンドウの再読み込みが必要になる場合があります。

16. ライブセッションを確認します。Copilot はコードを調べて状態を理解し、方針を考えて、コードを作成します。

処理には数分かかる場合があります。cloud agent の主な目的は、ほかの作業に集中できるよう非同期で処理することです。別のタスクも割り当ててから、アプリケーションの機能を実装しましょう。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent の利点のひとつは、作業を分けられることです。デザインチームのために関連ゲームの表示を追加する作業は、時間がかからないとしても、その時間をほかのタスクに使えます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue**、次に **Blank issue** を選択します。
4. **Title** に `Show related games on the game details page` と入力します。
5. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

6. **Create** を選択して Issue を作成します。
7. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。
8. **Assign** を選択します。

Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことが分かります。

![Copilot が Issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組みます。cloud agent はソフトウェアエンジニアと同じように動作するため、作業を常に監視する必要はありません。完了後にレビューできます。次はカスタムエージェントを作成して使います。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents] を使って、Issue を Copilot に割り当て、非同期で作業させる方法を学びました。技術的負債の解消、新機能の作成、別のフレームワークからのコード移行などに活用できます。

- コード生成環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- 範囲が明確な Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[cloud agent では MCP サーバー][cloud-agent-mcp]と、前のモジュールで扱ったカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
