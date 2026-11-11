---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz を公開しましょう。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのは自分の役割です。

このレッスンでは、次の内容を学びます。

- Copilot に GitHub Pages のデプロイを設定させる。
- マージする前にワークフローとプルリクエストをレビューする。
- デプロイの進行状況を確認し、公開されたサイトを検証する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、手軽に試せる方法が必要です。GitHub Pages を使って単一ファイルのサイトを公開すると、サーバーやランタイム依存関係を追加せずに、公開 URL からアクセスできるようになります。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Space Quiz のプルリクエストのマージ、Build website、Upload artifact、Deploy to Pages の各ステップを含む GitHub Actions の Deploy website 実行成功、yourname.github.io/space-quiz/ で公開された Space Quiz の 3 段階を示す図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトでは、エージェントにプルリクエストの作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。続行する前に、差分、デプロイ権限、GitHub の承認要求を確認してください。

## ワークフローをレビューし、デプロイを確認する

1. README は、リポジトリに存在しない場合にのみ追加されていることを確認します。
2. ワークフローのビルド手順とデプロイ手順を含め、プルリクエストの変更をレビューします。
3. 必須のプルリクエストチェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行が成功するまで確認します。
5. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを実行できない場合は、具体的な指示に従ってください。デプロイが成功し、公開サイトを開けるまでは、作業が完了したと判断しないでください。

## まとめと次のステップ

Space Quiz がサイトとして公開され、エージェントの報告を信頼するだけでなく、自分でデプロイを確認できました。[レッスン 13: レビューと次のステップ][next-lesson]に進んでください。

[next-lesson]: ../13-review/
