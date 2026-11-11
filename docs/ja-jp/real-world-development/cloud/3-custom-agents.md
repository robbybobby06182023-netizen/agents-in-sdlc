---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、焦点を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を記述すると、Copilot がその種類の作業をより効果的に実行できます。チームの知識を再利用可能なエージェントにできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング手法に従うセキュリティエージェント、一貫したテストパターンを保つテストエージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、または全体に適用される `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を指示します。最も分かりやすく区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、スキル（場合によっては別のエージェントも）を調整して作業できます。この演習では、アクセシビリティのカスタムエージェントがガイドラインに沿ってサイトをレビュー・更新します。その作業の一部として、プルリクエストのワークフロー用スキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの活用方法と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、次の内容を学びます。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスク割り当て。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての人がクラウドファンディングプラットフォームを利用できるようにしたいと考えています。最近のフィードバックによると、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいと感じる利用者がいます。このアクセシビリティ上の課題に対応するため、デザインチームは利用者が切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。カスタムエージェントを使って機能を作成します。

## アクセシビリティのカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントは、すでに作成されています。内容を確認し、Copilot に提供されるガイダンスを把握しましょう。

Codespace に戻り、アクセシビリティのカスタムエージェントファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、アクセシブルな Web サイトを作成する際の主な責務、ベストプラクティス、HTML/CSS/JavaScript のコード例、よくある間違いを確認します。

## Issue を作成して割り当てる

Mission control は、環境内のエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当てたり、進行中の作業を監視したり、方針を変えたり、追加のガイダンスを提供したりできます。まず、ハイコントラストモードの作成を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue**、次に **Blank issue** を選択します。
4. **Title** に `Add high contrast mode to website` と入力します。
5. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

6. **Create** を選択します。
7. 右側で **Assign to Copilot** を選択します。
8. 一覧から **Accessibility agent** を選択します。

    ![cloud agent の割り当て画面で、カスタムエージェントと Accessibility が選択されているスクリーンショット](../../../_images/ex5-select-custom-agent.png)

9. **Assign** を選択します。
10. Copilot がバックグラウンドでタスクを開始します。

## まとめと次のステップ

特定のタスクや分野に特化した GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、Copilot の作業を導けます。

この演習では、次の内容を学びました。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスク割り当て。次は mission control から[エージェントセッションを監視して指示を出し][next-lesson]ます。

## リソース

- [カスタムエージェントについて][custom-agents]
- [Organization でカスタムエージェントを使うための準備][org-custom-agents]
- [Enterprise でカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
