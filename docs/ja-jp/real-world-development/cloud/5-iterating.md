---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、UX の改善や機能追加に関する複数のタスクで GitHub Copilot と作業しました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能の実装、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能を依頼しました。ここではコードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善します。

### シナリオ

生成 AI を加えても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを有効化する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に対する GitHub Copilot の提案を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期で、監視されずにタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコード用ブランチに限られます。
- Cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions のワークフローは、人間の承認がなければ実行されません。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加する GitHub Copilot cloud agent の最初のプルリクエスト (PR) を、GitHub.com の標準的な PR インターフェイスで確認します。

> [!NOTE]
> PR を確認するとき、Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定された動作**です。必要に応じて、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択します。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が付いています。作業が完了するまで待ってください。数分かかることがあります。その間に休憩したり、学んだことを振り返ったりできます。

4. PR の準備ができたら **Files changed** タブを選び、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しい TSDoc のドキュメントコメントやその他のドキュメントなど、更新されたコードを確認します。実際の変更は異なる場合があります。レビュー時には、演習 2 の初めに確認したカスタム指示ファイルから反映される TSDoc、TypeScript 規約、コメントヘッダーに注目してください。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。単一 Astro アプリのプロジェクトチェックが成功することを確認します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。ほかのチームメンバーと同様に、PR やコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確率的に変わるため、必ず同じ結果になる具体的な文章は示せません。変更例は次のとおりです。

- 各コードファイルの先頭に、役割を説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを使い、コードベースに必要な変更をコメントで依頼します。ほかの利用者と同様に **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が変更する様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選ぶと PR に戻れます。

    ![プルリクエストに戻るボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、新しいコミットが PR に追加されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復できます。満足したらドラフト PR をレビュー可能な状態にし、main ブランチにマージします。

![PR をレビュー可能な状態にする画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次に、ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選び、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄に追加されたワークフローを確認します。単一 Astro アプリのプロジェクトチェックが成功することを確認してください。数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。Codespace のターミナルで、Copilot が作成したブランチ名（例: **copilot/fix-8**）を `<branch-name>` に指定して次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

関連ゲーム機能が実装されました。ほかの PR と同様に、cloud agent にコメントして変更を依頼できます。たとえば表示する関連ゲームの数を調整したり、この作業をカスタム指示の更新前に依頼したことを踏まえてコメントヘッダーや TSDoc を追加したりできます。**Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティのカスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で mission control から追加依頼したライトモードが含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選びます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。完了するまで待ってください。数分かかる場合があります。

4. **Files changed** タブで生成されたコードを確認します。
5. 実装を確認し、次の点に注目します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保持する local storage。
   - 高コントラストとライトモードの CSS やスタイル変更。
   - ARIA ラベルやキーボード操作などのアクセシビリティ属性。
   - モード切り替えを管理する JavaScript/TypeScript コード。
6. 問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄に追加されたワークフローが成功することを確認します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えてアクセシビリティ機能を手動テストできます。ターミナルで Copilot が作成したブランチ名を `<branch-name>` に指定して、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後アプリケーションを起動し、ブラウザーでハイコントラストとライトモードを切り替えます。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティのカスタムエージェントが、ベストプラクティスに沿って Copilot の実装を導いたことを確認しましょう。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent に実際のバックログ項目を渡して別の作業に取り組むと、効果的な使い方を身につけられます。リポジトリにいくつか Issue を作成し、Copilot に割り当ててみてください。たとえば、ゲーム詳細ページに支援者向けフォームを作る、ゲーム一覧にページネーションを実装する、データアクセスヘルパーに入力検証とエラー処理を追加する、などです。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンで、次の作業を行いました。

- リポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の出力への影響を確認しました。
- Issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ用カスタムエージェントを作成して使い、ハイコントラストとライトモードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、作業中のアクセシビリティセッションを監視して指示を出しました。
- `@copilot` に変更を依頼し、ワークフローを承認しながら cloud agent のプルリクエストをレビューして改善しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学ぶ場合は、ほかのハーネスで今回の内容を発展させられます。

- 🖥️ [VS Code ハーネス](../../vscode/): IDE から離れずに Copilot Chat の agent mode と MCP 統合を試します。
- 💻 [CLI ハーネス](../../cli/): Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリで、次のことも試してください。

- リファクタリングまたはテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 異なるスタック用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
