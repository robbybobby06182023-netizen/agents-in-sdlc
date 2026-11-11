---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューしてプルリクエストを作成しましょう。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルタリングと品質改善の作業を同じ会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次の内容を学びます。

- Agent Merge と、マージまでの作業を自動化する仕組みを理解する。
- 機能の全変更と検証の根拠を確認する。
- フィルタリングの PR を作成する。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルタリング機能の作業を通して、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの承認は開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするまでに必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題（失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要な場合の rebase など）に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せられますが、有効化する前に変更をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、変更をレビューして PR を作成し、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで PR を作成します。
5. `/pr agentmerge` コマンドで Agent Merge を有効にします。
6. Copilot が PR の CI を監視します。すべて成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は報告された理由を確認し、リポジトリで許可されている通常の手順でレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成やテスト、検証に加えて、プルリクエストのプロセスも自動化しました。このレッスンでは、次の内容を学びました。

- Agent Merge と、マージまでの作業を自動化する仕組みを理解する。
- 機能の全変更と検証の根拠を確認する。
- フィルタリングの PR を作成する。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認する。

次は、[Copilot CLI の便利なスラッシュコマンド][next-lesson]で、コンテキスト、モデル、共有、任意のクラウドへの委任を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
