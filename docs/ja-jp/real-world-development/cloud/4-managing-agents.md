---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の数回の演習で、Copilot cloud agent に UX の改善や機能追加など、3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールはいくつかあります。そのひとつが GitHub.com の[エージェントページ][agents-page]です。ここはミッションコントロールとして、開いているプルリクエスト (PR) があるすべてのエージェントタスクを表示します。エージェントの操作を確認し、進行中のセッションに追加の指示を出すこともできます。

このレッスンでは、次の内容を学びます。

- エージェントページを使って cloud agent のタスクを監視する。
- 進行中のセッションに追加機能を依頼する。

## シナリオ

ハイコントラストモードの作成を依頼した後、チームはライトモードも追加するよい機会だと考えました。サイトのスタイルと切り替え機能を更新しているため、同じ作業に含めるのが合理的です。エージェントに指示を出し、ハイコントラストに加えてライトモードも実装させます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方に、タスクの一覧が表示されます。コードのドキュメント更新、ゲームを変更する API の生成、Web サイトのハイコントラストモード追加などのタスクが含まれています。
3. 実行中のタスクを選択し、Copilot が実行した作業（リポジトリのチェックアウト、作業環境の作成、MCP サーバーの設定、タスクを完了するための各手順など）を確認します。

> [!NOTE]
> 表示される手順は、作業の状況や Copilot が選択した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認して、作業を監視できます。

## Cloud agent に指示を出す

実行中のタスクを確認したので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状態によって異なります。

    ![mission control に表示されたアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents ページの cloud agent タスクで、作業中のセッションに指示を出すダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、作業手順に追加したことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい依頼をワークフローに取り込みます。

処理には数分かかります。その間に、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページは、GitHub Copilot cloud agent のタスクを監視して指示を出すための中心的な場所です。ミッションコントロールから、進行中または完了したタスクを追跡し、作業内容を確認して、必要に応じて進行中のタスクに指示を出せます。このレッスンでは、次の内容を学びました。

- mission control と agents ページを使って cloud agent のタスクを監視する。
- 進行中のセッションに追加機能を依頼する。

次は、Copilot が作成したプルリクエストを[反復して改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
