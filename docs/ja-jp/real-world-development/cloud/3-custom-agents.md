---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせて、専用の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置いてエージェントを定義し、重点的な指示、ベストプラクティス、コーディングパターン、特定分野の知識を指定します。これにより、Copilot がその種類の作業をより効果的に行えるようになります。チームの専門知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティエージェント、安全なコーディングを確認するセキュリティエージェント、一貫したテストパターンを守るテストエージェントなどです。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に作業の方法を指示します。最もわかりやすい違いは、**カスタムエージェントは作業者、スキルはツール**だということです。

カスタムエージェントには専用のコンテキストウィンドウがあり、作業の一環としてスキルや、ほかのエージェントを調整して実行します。このラボのアクセシビリティ用カスタムエージェントは、アクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プルリクエストワークフローやテストの実行と管理を行うスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの機能と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

このレッスンでは、次のことを確認します。

- カスタムエージェントの定義方法
- カスタムエージェントへのタスクの割り当て方法

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての人がクラウドファンディングプラットフォームを使えるようにしたいと考えています。最近のユーザーフィードバックで、テキストと背景色のコントラストが不十分なため、ダークテーマが読みにくいという意見がありました。このアクセシビリティの問題に対処するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を作成しましょう。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、すでに作成されています。内容を確認し、Copilot に与える指針を理解しましょう。

Codespace に戻り、アクセシビリティ用のカスタムエージェントファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必須です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成に関する次の内容が記載されています。
   - 主な役割
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と避けるべきこと

## Issue を作成して割り当てる

ミッションコントロールは、環境内のエージェントと作業するための中心的な場所です。Copilot cloud agent にタスクを割り当てたり、タスクを監視したり、進行中のセッションに追加の指示を与えたりできます。まず、ハイコントラストモードの作成を Copilot に依頼しましょう。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Web サイトにハイコントラストモードが必要です。ユーザーが切り替えられるトグルを追加し、設定をブラウザーの local storage に保存してください。
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![cloud agent の割り当て画面で、カスタムエージェントと Accessibility が選択されている](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

[GitHub Copilot のカスタムエージェント][custom-agents]について学びました。特定のタスクや分野に特化した AI アシスタントです。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、特定の種類の作業をより効果的に行えるよう Copilot を案内できます。

このレッスンでは、次のことを確認しました。

- カスタムエージェントの定義方法
- カスタムエージェントへのタスクの割り当て方法

Copilot がハイコントラストモードを実装する間に、[ミッションコントロールからエージェントセッションを監視して案内する][next-lesson]方法を確認しましょう。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備をする][org-custom-agents]
- [エンタープライズでカスタムエージェントを使う準備をする][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
