---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。作業を特定の方法で進める必要がある場合や、Copilot に背景情報を伝えたい場合は、その情報を参照できるようにします。Copilot に固有のコンテキストを伝える方法はいくつかあります。主な方法のひとつは、コード生成のガイダンスを提供する[指示ファイル][instruction-files]です。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパスに応じた指示ファイルから、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認する。
- 現在の指示を使って、フィルタリング用の最初のデータ部分（発行元ヘルパー）を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。
- 同じプロンプトを再実行し、新しい標準が生成コードに反映される様子を確認する。
- 次の演習で更新したガイダンスを使えるよう、指示の変更とフィルタリングの基盤を `main` にコミットする。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、出力を一字一句一致させるのではなく、指示を更新した後の動作の「傾向」を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインがあります。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
- コードには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に理解させるためのコンテキストや好みを伝えられます。規約、ライブラリ、コメントの書き方などを指定して、より適切な提案やコードスニペットを得ることができます。リポジトリ全体、または特定の種類のファイルに指示を作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関する**すべての**リクエストで Copilot に渡される単一の指示ファイルです。技術スタック、開発中のものの概要、ベストプラクティス、全般的なガイダンスなど、プロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類を対象にできます。たとえば、TypeScript などの言語、UI コンポーネントの作成、ユニットテストの作成などです。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使います。
>
> また Copilot は AGENTS.md や CLAUDE.md など、ほかの規格を使う指示ファイルにも対応しています。[対応する指示ファイル][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、この演習の範囲外です。サンプルプロジェクトに代表的な方法を示しています。基本的には、次の点を意識してください。

- `copilot-instructions.md` は、構築中のもの、プロジェクト構成、全体的なコーディング標準など、プロジェクトレベルのガイダンスに絞ります。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種類や特定のタスク向けに使います。
- 自然な言葉で、明確なガイダンスと例を記載します。

指示ファイルの作成にも AI の利用にも唯一の正解はありません。試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを作成させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各環境で手順が異なります。該当するレッスンで説明します。
>
> テンプレートや出発点を探していますか？指示ファイルやカスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。中心となる `copilot-instructions.md` と、タスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針にも注目してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルの glob を指定します。ここでは、`**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや既存ファイルへのリンクがあります。大きな指示を小さな再利用可能なファイルに分け、Copilot に例を示せます。ここに記載されたパスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはまだありません。次の手順で TSDoc コメントとファイルコメントヘッダーのルールを追加します。

## 変更用ブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送ります。後で指示を更新し、同じプロンプトを再送して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを参照できるようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの作成場所を Copilot に知らせます。
3. Chat ビューのエージェント一覧から **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを変更します。ヘルパーファイルとテストなど、複数のファイルが変更されることがあります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、発行元の型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加することがあります。その場合も問題ありません。指示を更新した後に動作が一貫することが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案の質を高めるため、コーディング標準を記載します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクション（27 行目付近）を探します。既存のコーディング標準には、コード内ドキュメントのルールがありません。
3. 既存の標準の直下に、ファイルヘッダーと TSDoc コメントを追加する次の Markdown を記述します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルは、`.github/copilot-instructions.md` のようなリポジトリレベルのファイルに作成して全体に適用することも、特定の言語、ファイル種類、タスク向けの `*.instructions.md` として作成することもできます。今回のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選んで会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使うコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもほかのファイルと同じように、ソース管理で管理します。変更をコミットしてリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の変更と、ヘルパーの基盤として Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けたプルリクエストを作成します。手動で作成する方法もありますが、GitHub ツールがプロジェクトに接続されているため、Copilot に依頼できます。Issue を検索し、Issue を解決する PR を作成して、マージを依頼します。

> [!NOTE]
> Copilot が GitHub 操作を実行するのは、この演習が初めてです。プロジェクトのテンプレートには GitHub のツールがあらかじめ接続されています。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められる場合があります。画面の指示に従ってください。この接続の仕組み (Model Context Protocol) は後の演習で学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** を選びます。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成して、指示の更新と発行元ヘルパーの基盤の両方を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub 操作を許可するよう求めた場合は、コマンドを確認して、適切な場合に **Allow** を選びます。
5. PR が作成されたら、次のプロンプトでマージを依頼し、ローカルブランチを `main` に戻します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub 操作やシェルコマンドの実行を許可するよう求められた場合は、内容を確認して **Allow** を選びます。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを得る方法を確認し、VS Code の Copilot Chat で次の作業を行いました。既存の指示を使ったコード生成プロンプトを実行し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加して、同じプロンプトを再送信し、提案されたコードに標準が反映されることを確認しました。指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は [agent mode][next-lesson]を使い、Copilot がコードベース全体に新機能を追加する際に指示を適用します。

## リソース

- [GitHub Copilot をカスタマイズする指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
