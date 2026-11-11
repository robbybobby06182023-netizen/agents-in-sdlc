---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。具体的には、次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されているフィルタリング Issue を見つけました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- リポジトリの指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーの根拠を確認しました。
- フィルタリングの変更全体をレビューし、Agent Merge を許可して PR をマージしました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## リリースした内容

このワークショップには、更新済みの `main` からそれぞれブランチを作成して進める、2 つの PR があります。

1. **星評価**: 既存の `starRating` と、評価がない場合の状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポート形式をカスタマイズし、QA プロファイルと関連テストを追加します。

フィルタリングの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を使った Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、設定した形式で結果を報告しました。QA はこれらの結果と要件、テストカバレッジのレビューをまとめてから PR に進みました。

追加するテストは実際の不足を補うものである必要があります。QA を実行してもテストを追加する必要がない、という結果が正しい場合もあります。マージを許可する前にコードと根拠を確認し、変更があった場合は影響を受ける根拠を更新してください。

## ベストプラクティス

Copilot に提供するコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームの要望に合わせて調整してください。指示は標準を定め、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に方針を検討するとき、**Interactive** は変更に関与し続けたいとき、**Autopilot** は範囲が明確なタスクに適しています。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように実装するのかを明確に伝えると、結果は大きく変わります。

## さらに試す

中心的なワークフローを完了しました。ほかにも試せる CLI 機能があります。

- `/review` でコードレビューエージェントに変更を分析させます。
- `/rubber-duck` で問題を相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールを上達させるには、使い続けるのが一番です。製品コード、趣味のコード、長年作ってみたいと思っていた小さなアプリなどで活用してください。チームで学びを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張するには、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
