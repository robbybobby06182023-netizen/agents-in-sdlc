---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使ううえで、コンテキストは重要です。作業を特定の方法で進める必要がある場合は、Copilot が参照できる場所にガイダンスを用意します。[指示ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。フィルタリング機能を実装したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の内容を学びます。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される仕組みを確認する。
- コーディング標準に従うよう指示ファイルを更新する。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインがあります。

- コメントにはコードを言い換えるのではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を TSDoc/JSDoc で記載します。該当する場合は、注入可能な `db` 引数も説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変更されたときはコメントを最新の状態に保ちます。
- 既存の書式と lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発手法に沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に理解させるためのコンテキストや好みを提供できます。規約、ライブラリ、コメントの書き方などを指定し、より適切な提案やコードスニペットを得られます。リポジトリ全体、または特定の種類のファイルに指示を作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関する**すべての**リクエストで Copilot に渡される単一の指示ファイルです。多くのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種類、タスク向けのガイドラインを記載します。

> [!NOTE]
> その他の指示形式やサポート状況は環境によって異なります。特定の形式を使用する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、すぐ使えるよう指示ファイルが含まれています。変更前に内容を確認して、指示の効果を比較できるようにします。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの説明とコーディングガイダンスを確認します。これらの指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどに関する指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドが対象ファイルを示す glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` のリンクを確認します。ほかの指示ファイルや既存ファイルを参照することで、大きな指示を再利用可能な小さなファイルに分け、Copilot に例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存の指示はよい出発点ですが、追加のルールが必要です。`copilot-instructions.md` に TypeScript の TSDoc コメントを追加する標準を記述します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクション（35 行目付近）を探します。
2. 末尾に次の項目を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルタリングのコンテキストを保ったまま新しい指示を読み込めるよう、変更を保存した後に会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチに切り替え、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   求められた場合はフィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを得る仕組みを確認し、新しい標準を機能に適用しました。具体的には、次の作業を行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される仕組みを確認する。
- コーディング標準に従うよう指示ファイルを更新する。
- 指示ファイルがコードに与える影響を確認する。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
