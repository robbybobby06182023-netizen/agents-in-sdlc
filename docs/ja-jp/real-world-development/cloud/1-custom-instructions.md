---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報にアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」してほしいかだけでなく、「どのように」実行してほしいかも伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの実行前後を比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで影響を並べて示すのが難しいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの効果を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
- コードには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。規約、ライブラリ、好みのコメントの書き方などを指定できるため、より適切な提案やコードスニペットを得られます。リポジトリ全体、または特定の種類のファイルに対して指示を作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関する**すべての**リクエストで Copilot に渡される単一の指示ファイルです。技術スタック、開発中の内容の概要、ベストプラクティス、全般的なガイダンスなど、Copilot Chat や CLI の多くのリクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に使う指示ファイルです。TypeScript などの言語や、UI コンポーネントの作成、ユニットテストの作成などのタスクを対象にできます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使用します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの規格を使った指示ファイルにも対応しています。[対応する指示ファイル][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成に関する詳細はこの演習の範囲外ですが、サンプルプロジェクトには代表的な構成が含まれています。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、構築中の内容、プロジェクト構成、全体的なコーディング標準など、プロジェクトレベルの情報に絞って記載します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種類や、特定のタスクに関する指示に使います。
- 自然な言葉で、明確なガイダンスや例を示します。

指示ファイルの作成や AI の使い方に唯一の正解はありません。実際に試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot を使って指示ファイルを作成することもできます。各環境で手順は異なります（たとえば、VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など）。該当するレッスンで手順を説明します。
>
> テンプレートや出発点を探していますか？指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** の指針にも注目してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示を適用するファイルの glob を指定しています。ここでは、`**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。パスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## リポジトリの標準を追加する

ここで行う編集は 1 か所だけです。コード内のドキュメントを TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーで記述する、プロジェクト全体のルールを追加します。後でこのハーネスの演習で Issue を割り当てると、cloud agent がこのルールを使います。

編集の前に、作業用のブランチを作成します（cloud agent は Issue の対象ブランチにある指示を読み取ります。ここでは変更をきちんとコミットします）。

1. Codespace のターミナルで、次のコマンドを実行して新しいブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案の質を高めるため、コーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはまだありません。そのため、生成されたヘルパー関数にドキュメントコメントがありませんでした。
3. 既存の標準の直下に、次の Markdown を追加します。これにより、Copilot にファイルコメントヘッダーと TSDoc のドキュメントコメントを追加するよう指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベルの `.github/copilot-instructions.md` に作成して全体に適用することも、特定の言語、ファイル種類、タスク向けの `*.instructions.md` として作成することもできます。今回追加するようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

Cloud agent は、Issue が対象とするブランチの指示ファイルを読み取ります。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を読み取れるよう、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で Issue に割り当てるとき、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチを作成せずに `main` にコミットしてもかまいません。実際のプロジェクトでの手順に沿うため、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを得る方法を確認し、リポジトリ全体に適用する標準を追加しました。このハーネスで cloud agent が作成するプルリクエストを確認するときに、この標準が適用されているかを確認できます。

次は、[最初の Issue を Copilot cloud agent に割り当て][next-lesson]ます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
