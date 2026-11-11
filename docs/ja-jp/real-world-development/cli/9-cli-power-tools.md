---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、クラウドへの任意の委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI にもさまざまなスラッシュコマンドがあります。高度な機能、内部情報、追加の構成オプションを利用できます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ほかの便利なコマンドも確認しましょう。

このレッスンでは、次の内容を学びます。

- `/context` と `/compact` で Copilot の会話コンテキストの管理方法を確認する。
- `/model` で利用可能なモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認する。

## シナリオ

CLI の中心的なワークフローを終えたので、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを調べる

大規模または複雑なタスクでは、モデルのコンテキストウィンドウの上限に近づく場合があります。Copilot CLI は必要に応じて会話を自動的に compact します。また、スラッシュコマンドで状態を確認したり、自分で compact したりできます。

1. Codespace に戻ります。起動していない場合は、リポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように配分されているかを確認します。
4. 会話を compact します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな変化がないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的に compact します。タイミングを選びたい場合は `/compact` を使います。関係のないタスクに切り替えて新しい会話を始めるには、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも違います。Copilot CLI では、モデルを一覧表示して使用するモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用量に関する情報を確認します。
3. 現在のモデルを使う、別のモデルを選ぶ、または <kbd>Esc</kbd> を押して一覧を閉じる、のいずれかを選びます。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールの活用方法を互いに高められます。`/share` は、セッションの Markdown/HTML ファイルへのエクスポート、共有リンクの作成、GitHub gist の公開に使えます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` オプションを確認します。
2. 共有する場合は、ローカルの Markdown へのエクスポートに使う `/share file` など、目的に合った方法を選びます。
3. エクスポートした内容を送信または公開する前に確認します。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を予定していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業の並列化または委任

ワークショップの中心的な内容は完了しました。Copilot CLI には、大きなタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに割り当て、並行実行できます。
- `/worktree` は、別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は、非同期で動作しプルリクエストを作成することがある Copilot cloud agent にタスクを送信できます。

これらのコマンドは追加の worktree やリモート作業を作成する可能性があるため、任意です。試す場合は、範囲の明確な新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく学ぶには、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

スラッシュコマンドを使うと、Copilot CLI を構成したり、セッションを共有したり、内部の動作を確認したりできます。このレッスンでは、次の内容を学びました。

- `/context` と `/compact` で Copilot の会話コンテキストの管理方法を確認する。
- `/model` で利用可能なモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認する。

Copilot CLI には、さらに多くのスラッシュコマンドがあります。最後に、[学んだ内容と次のステップを確認][next-lesson]しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
