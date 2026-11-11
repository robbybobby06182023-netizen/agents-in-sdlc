---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode でフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで正しく動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーを操作させ、機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の内容を学びます。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを理解する。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot にサーバーを使わせて、ブラウザーでフィルタリング機能を手動テストする。
- ブランチを公開し、フィルタリング作業の PR を作成する。

## Model Context Protocol (MCP) とは

Agent mode は、エディターの外部にアクセスできるとさらに便利になります。Copilot が外部ツールを使うための標準的な方法が Model Context Protocol (MCP) です。

![agent mode の内部構成と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部ツールやサービスと通信できます。MCP サーバーは、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりするための橋渡しをします。

MCP サーバーは、AI エージェントと外部ツールやサービスの間の通信を管理します。たとえば既存の API や、NPM パッケージなどのローカルツールに接続します。それぞれの MCP サーバーは、エージェントが利用できるツールやリソースを提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスし、新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを行います。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページの移動、フォームの入力、ボタンの選択などを行えます。

ほかにもさまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、見つけやすさとエコシステムへの貢献を促すために [MCP レジストリ](https://github.com/mcp)を提供しています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかの依存関係と同様に扱ってください。利用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセス許可には注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースが利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれているため、前の演習で Copilot が Issue のバックログを確認できました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]により Copilot がブラウザーを操作できるため、機能のテストに適しています。

1. `.vscode/mcp.json` で `github` の隣に `playwright` エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` はローカルサーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面に表示しないブラウザーで動作するよう指定します。Codespace には表示用デスクトップ環境がないため必要です。

> [!NOTE]
> Tailspin Toys では E2E テストに Playwright を使うため、通常はブラウザーもインストール済みです。ブラウザーがないと Copilot が報告した場合は、`npx playwright install chromium` を実行して再試行します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて使うときにサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に表示される **Start** を選択し、サーバーを起動して構成を確認します。
2. VS Code からサーバーを信頼するか尋ねられた場合は、構成を確認して信頼することを選びます。

手動での起動は省略できます。その場合、次の手順で Copilot が初めてツールを必要としたときに VS Code が起動し、信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でサイトを操作する代わりに、エージェントがページを開き、フィルターを設定し、結果を読み取って要約します。会話を離れずに機能が期待どおりに動作するか確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。エージェントは支援技術と同じように、ボタン、リンク、リスト項目などの構造化されたラベル付き要素を判断するため、簡単な機能確認がアクセシビリティの簡易チェックにもなります。

サーバーを接続し、アプリを起動した状態で、作成したフィルタリング機能を Copilot にテストさせます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーからブラウザーを起動し、手順を実行して結果を報告します。Issue の受け入れ基準とレポートを照らし合わせます。想定と異なる場合は質問を続けるか、PR を作成する前にコードの修正を依頼してください。

> [!NOTE]
> テストするにはアプリが `http://localhost:4321` で起動している必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動します。Playwright MCP サーバーを初めて使うときは、ブラウザーのダウンロードが必要な場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行するよう依頼してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームでレビューするためのプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が変更したファイルを確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。

   ![変更されたファイルを表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、PR を作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼できます。Copilot は関連 Issue を探し、関連付けて PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるよう、エージェント一覧で **Agent** が選択されていることを確認します。
3. 次のプロンプトを送り、PR を作成するよう依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報の取得や操作を Copilot に許可するため、必要に応じて **Continue** を選択します。GitHub ツールの初回使用時に GitHub 認証を求められることがあります。画面の指示に従ってください。
5. Copilot が Issue を検索し、適切なものを見つけて PR を作成します。
6. Copilot が生成したリンクを選択して PR を確認します。ただし、まだマージしないでください。

## まとめと次のステップ

おめでとうございます。この演習では、次の内容を学びました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを理解する。
- Playwright MCP サーバーをワークスペースに追加する。
- Playwright MCP サーバーを使って、ブラウザーでフィルタリング機能を手動テストする。
- ブランチを公開し、フィルタリング作業の PR を作成する。

MCP サーバーで機能を**テスト**しましたが、Copilot を拡張する方法は MCP だけではありません。次は[カスタムエージェントを作成し][next-lesson]、アクセシビリティレビューなど、対象を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
