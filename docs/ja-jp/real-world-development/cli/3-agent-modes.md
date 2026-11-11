---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で実装方針を決め、Autopilot で Issue に基づくフィルタリングを実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。大きな変更には、より確実なプロセスが必要です。GitHub Copilot CLI は組織の既存のワークフローに沿って動作し、適切な方法で必要な機能を実装できます。このレッスンから数回にわたり、Issue から機能を作成し、コードの妥当性と動作を確認して、プロジェクトにマージするまでの一般的なエージェント主導の開発プロセスを進めます。

> [!NOTE]
> 機能のワークフローでは、同じ会話とブランチを使い続けます。通常はファイルの種類ごとにブランチや PR を分けますが、ここでは中心的な概念に集中するため手順を簡略化します。

このレッスンでは、次の内容を学びます。

- GitHub Issue から Copilot との新しい会話を始める。
- Plan モードで要件を定義する。
- Autopilot モードで新しい機能を実装する。
- コードをレビューする。
- 転送したブラウザーで手動検証する。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、利用者はカテゴリーや発行元でゲームを絞り込む必要があります。バックログの Issue には機能が記載されていますが、カテゴリーを組み合わせる方法など、実装前に合意が必要です。Plan モードで仕様を決めてから、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発に取り入れても、基本は変わりません。むしろ、基本がより重要になります。多くの開発者は、次のような手順で作業します。

1. 必要な作業を記載した Issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、一般的にはこの手順を基本にしています。

この標準的な手順に従うことで、AI が生成したコードが要件を満たし、手書きのコードと同じ検証プロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントの自律性の度合いを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次の操作に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行して、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルタリングに着手する前に、Codespace とターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、作業用ブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue やプルリクエストをコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、Copilot に作業を依頼する前に、機能の実装方法やタスクの進め方を決められます。複雑な作業では、計画に時間をかけることをお勧めします。Plan モードに切り替え、Copilot に計画を作成させましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot がプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. フィルタリングの動作について質問された場合は、希望に応じて回答します。正解はひとつではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を確認します。
6. 決定した内容と異なる箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を確認したので、実装に進みます。Autopilot を使って Copilot に作業を任せます。Autopilot では、Copilot がタスクを完了したと判断するまで作業を反復します。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンで同様の名前の選択肢を選びます。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があります。先に計画を確認してください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と判断する前にセットアップの問題を解決してください。

## 実装をレビューして検証する

ほかのコードと同様に、生成されたコードもマージ前にレビューします。コードを確認し、サイトを起動して正しく動作することを確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を Issue と計画時の決定事項に照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を確認する

コードがよさそうでも、実際に動くか確認しましょう。前と同じようにアプリを起動し、Codespaces のポート転送を通じてサイトを開きます。

1. Copilot に次のプロンプトを送り、アプリを起動します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリー、発行元、およびその組み合わせでフィルタリングします。
4. リセットと該当結果がない場合の動作が、Issue と決定事項に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能を作成しレビューしました。このレッスンでは、次の作業を行いました。

- GitHub Issue から Copilot との新しい会話を始める。
- Plan モードで要件を定義する。
- Autopilot モードで機能を実装する。
- コードをレビューする。
- 転送したブラウザーで機能を手動検証する。

次は、[カスタム指示を使って][next-lesson]、コードが文書化された開発手法に沿うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
