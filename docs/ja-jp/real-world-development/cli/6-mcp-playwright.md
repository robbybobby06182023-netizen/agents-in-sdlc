---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が使える自動化機能を追加する必要もあります。MCP サーバーを使うと、Copilot CLI に組み込まれている機能を超えて、ツールやサービスを利用できます。

このレッスンでは、次の内容を学びます。

- Model Context Protocol (MCP) と Copilot CLI での使い方を理解する。
- Playwright MCP サーバーがまだ利用できない場合に追加する。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認する。

## シナリオ

ユニットテストや E2E テストは重要ですが、UI の変更を検証するには実際に UI を操作する必要があります。Copilot が利用者と同じように Web サイトを使えるようにし、期待どおりの変更をより確実に実現します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、エージェントは最新の情報を取得し、ユーザーに代わって操作を実行できます。

ツールやリソースには MCP サーバーを介してアクセスします。MCP サーバーはエージェントと外部ツール／サービスの橋渡しをし、それぞれが利用できるツールやリソースを提供します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにもさまざまな MCP サーバーがあります。GitHub は、見つけやすさとエコシステムへの貢献を促すために [MCP レジストリ][mcp-registry]を提供しています。

> [!CAUTION]
> MCP サーバーは、ほかの依存関係と同様に扱ってください。利用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot が利用者と同じように Web サイトを操作できるように追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、この手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類として **STDIO** を確認し、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のコマンドを貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を調べるよう Copilot に依頼する

以前は手動で機能を確認しました。今度は Copilot から利用できる Playwright MCP サーバーを使います。

1. 次のプロンプトを送り、Playwright MCP サーバーを使って機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーを指定しなくても、Copilot が自動的に判断します。ここでは使用するサーバーが分かっているため、指定すると結果の一貫性が高まり、トークンの節約にもなります。

2. Copilot がブラウザーで機能を確認する手順を見ます。
3. レポートを読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作し、サーバーを停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。このレッスンでは、次の内容を学びました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を理解する。
- Playwright MCP サーバーがまだ利用できない場合に追加する。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認する。

次は、[QA カスタムエージェントを作成し][next-lesson]、スキルとブラウザーツールを専門的な役割として組み合わせます。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
