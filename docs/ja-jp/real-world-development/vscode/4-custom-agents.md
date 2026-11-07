---
title: "演習 4 - カスタムエージェント"
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

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての人がクラウドファンディングプラットフォームを使えるようにしたいと考えています。最近のユーザーフィードバックで、テキストと背景色のコントラストが不十分なため、ダークテーマが読みにくいという意見がありました。このアクセシビリティの問題に対処するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を作成しましょう。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat のアクセシビリティエージェントで、ハイコントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、すでに作成されています。内容を確認し、Copilot に与える指針を理解しましょう。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します（フィルタリング PR とは分けておきます）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必須です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成に関する次の内容が記載されています。
   - 主な役割
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と避けるべきこと

## Copilot Chat でカスタムエージェントを使う

VS Code のエージェントドロップダウンには、`.github/agents` に定義したすべてのカスタムエージェントが表示されます。カスタムエージェントを選択すると、エージェントの指示とツールに基づいてチャットセッションを進められます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

1. Chat ビューのエージェントドロップダウンから、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェントドロップダウンを選択します。現在のエージェント名が表示されます。既定では **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

    ```plaintext
    サイトにハイコントラストモードを追加してください。ユーザーが切り替えられるトグルを設け、設定をブラウザーの local storage に保存して、ページを再読み込みしても保持してください。
    ```

5. Copilot Chat はコードベースを調べ、変更を提案して、プロジェクトのファイルを編集します。編集内容は、ファイルパスとレビューできる差分とともにチャットに表示されます。
6. エージェントが変更したファイルの一覧は、**Files changed** の表示に反映されます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなど、実際にリポジトリのファイルを変更します。

次の演習では、進行中の作業をレビューし、必要な指示を追加します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。特定のタスクや分野に特化した AI アシスタントです。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、特定の種類の作業をより効果的に行えるよう Copilot を案内できます。

このレッスンでは、次のことを確認しました。

- カスタムエージェントの定義方法
- Copilot Chat の agent mode でカスタムエージェントを使う方法

次は、エージェントが作業している間に変更をレビューし、同じセッションにライトモードの切り替え機能を追加するよう依頼して、[作業を監視して案内します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
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
