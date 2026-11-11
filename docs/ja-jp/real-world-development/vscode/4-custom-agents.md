---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、焦点を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を記述すると、Copilot がその種類の作業を効果的に進められます。チームの知識を再利用可能なエージェントにできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング手法に従うセキュリティエージェント、一貫したテストパターンを保つテストエージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、または全体に適用される `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を指示します。最も分かりやすく区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、スキル（場合によっては別のエージェントも）を調整して作業できます。この演習では、アクセシビリティのカスタムエージェントがガイドラインに沿ってサイトをレビュー・更新します。その作業の一部として、プルリクエストのワークフロー用スキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの活用方法と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての人がクラウドファンディングプラットフォームを利用できるようにしたいと考えています。最近のフィードバックによると、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいと感じる利用者がいます。このアクセシビリティ上の課題に対応するため、デザインチームは利用者が切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。カスタムエージェントを使って機能を作成します。

この演習では、既存のアクセシビリティ用カスタムエージェントを確認し、Copilot Chat で使ってハイコントラストモードを実装します。

## アクセシビリティのカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントは、すでに作成されています。内容を確認し、Copilot に提供されるガイダンスを把握しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用の新しいブランチを `main` から作成します（演習 3 のフィルタリング PR とは分けて作業します）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、アクセシブルな Web サイトを作成する際の主な責務、ベストプラクティス、HTML/CSS/JavaScript のコード例、よくある間違いを確認します。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビューの下部にあるエージェント一覧には、`.github/agents` で定義されたすべてのカスタムエージェントが表示されます。エージェントを選択すると、その指示とツールに合わせてチャットセッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェント一覧で **Agent** が選択されていない場合は選択します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント一覧を開きます。ここには現在のエージェントが表示されます。既定のエージェントは **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選びます。
4. アクセシビリティエージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat はコードベースを調べ、変更を提案して、プロジェクトファイルに適用します。各編集はファイルパスと差分を含めてチャットに表示されます。
6. エージェントが変更したファイルに応じて **Files changed** の表示が更新され、作業対象を確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなどの既存ファイルを実際に変更します。

次の演習では、進行中の作業をレビューして指示を出します。

## まとめと次のステップ

特定のタスクや分野に特化した GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、Copilot の作業を導けます。このレッスンでは、次の内容を学びました。

- カスタムエージェントの定義方法。
- Copilot Chat の agent mode でカスタムエージェントを使う方法。

次は[エージェントの作業を監視して指示を出し][next-lesson]、変更を確認しながら同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
