---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリングを確認しました。ここでは、これらの機能を明確な QA プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、次の内容を学びます。

- カスタムエージェントが指示、スキル、MCP ツールと連携する仕組みを確認する。
- 再利用可能な品質保証プロファイルを作成して確認する。
- QA エージェントを選び、フィルタリング Issue に照らして結果をレビューする。

## シナリオ

Tailspin Toys では、PR を作成する前に要件、コード品質、自動チェック、テストカバレッジ、ブラウザー上の動作を一貫してレビューしたいと考えています。カスタムエージェントを使うと、この QA プロセスを調整し、再利用できるレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する、Copilot の特化したバージョンです。目的、指示、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでのカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を示し、`quality-checks` スキルは繰り返し実行するチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルは要件を評価して結果を報告するために、これらの機能をどう使うかを Copilot に指示します。既存の機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルの作成を Copilot に依頼します。確認項目と、実行時に従う制約をプロファイルに定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のプロファイルを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA の手順と制約を反映したことを確認します。検証だけを頼んだのに、機能を変更してしまうような不備を防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は省略できますが、設定するとエージェントの表示名が明確になります。
3. 指示を読み、要件の確認、リポジトリ指示の順守、`quality-checks` スキルの実行、Playwright MCP の利用が含まれていることを確認します。
4. 根拠を示してレポートすること、実装コードの変更前に確認を求めること、変更をコミットしたり PR を開いたりしないことを確認します。
5. 必要な役割や制約が欠けている場合は、続行する前に既定のエージェントに修正を依頼します。

## Issue を QA エージェントで確認する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイル作成後にフィルタリングの会話を再開し、QA を選択すると、Issue と計画の内容を引き継げます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントが選択肢に表示されない場合がありますが、上記のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能をレビューさせます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業を確認します。
4. 完了したらレポートを読みます。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに加え、結果をレビューしました。このレッスンでは、次の内容を学びました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する仕組みを確認する。
- 再利用可能な品質保証プロファイルを作成して確認する。
- QA エージェントを選び、フィルタリング Issue に照らして結果をレビューする。

実装、スキルの更新、QA プロファイル、テスト、検証レポートをまとめ、[機能 PR を作成して Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
