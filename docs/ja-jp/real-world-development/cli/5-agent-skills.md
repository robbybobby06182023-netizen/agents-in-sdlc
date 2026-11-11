---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能の検証に使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、開発には多くの作業があります。これまでは、コードが正しく動作することを手動で検証し、指示ファイルを使って標準に従うようにしてきました。テスト、lint、その他の継続的インテグレーション (CI) の作業はどうでしょうか。

このような作業には **agent skill** が適しています。スキルは、これらの操作を正しく実行する方法を Copilot が理解するのに役立ちます。

このレッスンでは、次の内容を学習します。

- 既存の `quality-checks` スキルを確認する。
- 結果の形式をカスタマイズする。
- スキルを再読み込みして実行する。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使用します。チームは結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

Agent skill は、再利用可能なタスク指示、実行可能なスクリプト、サポート用のリソースをまとめたもので、エージェントが必要に応じて読み込みます。基本的には、スキル名をフォルダー名とし、その中に `SKILL.md` という Markdown ファイルを置きます。Markdown には、スキルの内容を定義する `name` と `description` の frontmatter、概要、呼び出すタイミングのガイダンスが含まれます。フォルダーには、呼び出されたときにスキルが使用するスクリプトやその他のリソースを含むサブフォルダーを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルには `SKILL.md` のみがあり、プロジェクト既存のコマンドを使用します。

スキルはプロジェクトの `.github/skills` フォルダーに配置すると、チームで共有して再利用できるリポジトリアセットになります。または、ユーザーのスキルフォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭の `name` と `description` を読みます。description は Copilot がスキルを呼び出すタイミングを判断するのに役立ちます。
3. 指示を読み、テストと lint の手順をどのように案内しているか確認します。
4. スキルには **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出すことも、自然言語で指示することもできます。Copilot に3つのチェックを実行するよう依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを使用します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが報告した内容を伝える、よりよいレポートにしましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルを実際に使ってみましょう。Copilot CLI は会話を再起動せずに、編集されたスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを使用します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されたレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使用しました。このレッスンでは、次の作業を行いました。

- 単体テスト、lint、型チェック用の `quality-checks` スキルを確認した。
- 結果の形式をカスタマイズした。
- スキルを再読み込みして実行した。

この変更は、フィルター機能とともに機能 PR に含まれます。次は、[Playwright MCP サーバーを使って][next-lesson] Copilot がサイトと直接やり取りできるようにします。

## スキルのその他の例

コミュニティによる次の例は参考資料であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とバンドルされたエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
