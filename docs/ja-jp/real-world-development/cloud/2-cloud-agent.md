---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残っている機能リクエストなどが考えられます。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行します。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。これにより、手間のかかるタスクを委任して開発を加速し、開発者はより大きな課題に集中できます。

このレッスンでは、Copilot cloud agent で次のことを確認します。

- コード生成環境をカスタマイズする方法
- 処理を安全に行う方法
- 範囲を明確に定めた Issue の重要性
- Copilot に Issue を割り当てる方法

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを作成した委託先がドキュメントを整備しておらず、実際にはほとんどありません。まず、アプリケーション内のエクスポートされたすべての関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのタイトルを「関連ゲーム」として表示したいとのことです。まだ洗練された実装は必要なく、UX の受け入れテストに使えるものを求めています。現在はブロッカーですが、優先度がより高い Issue がほかにあります。

どちらも、優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てれば、非同期で作業を進めてもらえます。その間、別の作業に集中し、あとから Copilot の変更をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で作業を依頼できます。割り当てると、Copilot は進行状況を追跡するドラフトプルリクエストを作成し、環境をセットアップしてタスクに取りかかります。実行中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備が整うと、プルリクエストであなたに通知します。

## 適切な範囲の指示が重要な理由

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単な作業でも、詳しく調べると複雑な場合があります。Copilot cloud agent に依頼する作業の範囲は、[慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、調整しましょう。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示とその働きを確認しました。カスタム指示のハンズオンを完了していれば、`.github/copilot-instructions.md` が機能することを確認できています。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを確認してください。後でその効果を確かめられます。

リポジトリの GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターに注目してください。この指示は `**/*.test.ts` に適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、プルリクエストで TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーを確認してください。これらの指示ファイルに由来する内容です。後の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

誰が作業する場合でも、コードの作成には通常、環境の準備と、良好な状態で作業するためのスクリプト実行が必要です。Copilot にタスクを割り当てる場合も同様です。Copilot は SWE と同じように作業を進めます。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定した[セットアップワークフロー][setup-workflow]を作成し、作業開始前に実行することで、必要な開発ツールや依存関係を利用できるようにできます。このラボでは、学習に必要な環境をあらかじめ設定しています。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一の Astro アプリ用にマイグレーションとシードを済ませた SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローと似ていますが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できるため、Copilot の作業を待たずに正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントが重要だと理解していても、情報が古かったり、まったくなかったりするプロジェクトは少なくありません。こうした技術的負債は放置されがちで、生産性を低下させ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を得意としているため、cloud agent に依頼するのに適したタスクです。バックグラウンドで必要なドキュメントを作成します。次の演習では、その作業をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空の Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    関数やメソッドでは、役立つ場合に TSDoc のドキュメントコメントを含めることが、組織の要件です。残念ながら、最近の更新ではこの標準が守られていません。動作を明確にするコメントが適切に含まれるよう、既存のコードを更新する必要があります。
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前です。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が ToDo リストを作成したことを確認できます。

> [!NOTE]
> Copilot の ToDo リストが PR に表示されるまで、数分かかる場合があります。Copilot は前述のワークフローを実行して環境を作成し、プロジェクトを分析して、問題を解決する方法を検討しています。

13. リストと、Copilot が実行しようとしているタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が出るまで、ウィンドウの再読み込みが必要な場合があります。

16. セッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状態を把握し、計画を考えて適切な方法を決め、コードを作成します。

この作業には数分かかる場合があります。Copilot cloud agent の大きな利点は、タスクを非同期で実行し、その間にほかの作業に取り組めることです。この利点を活かすため、もう 1 つのタスクを cloud agent に割り当ててから、アプリケーションに機能を追加するコードを書きます。

## ゲームの詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザインチームから依頼された関連ゲームの表示は、それほど時間のかからない作業かもしれませんが、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    ゲームの詳細ページに関連ゲームを表示し、ユーザーがさらにゲームを見つけやすくしたいと考えています。デザインチームは UX を検討し、受け入れテストを行いたいと考えています。要件は次のとおりです。

   - `src/lib/` にデータアクセスヘルパーを追加し、指定したゲームを除く同じカテゴリのほかのゲームを返す
   - ヘルパーを使って、ゲームの詳細ページに「関連ゲーム」セクションを表示する
   - 関連ゲームがない場合にも対応する
   - 新しいヘルパーのユニットテストを作成する
   - PR を作成する前に、すべてのテストが成功することを確認する
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が Issue の最初のコメントに表示されます。

![Issue に Copilot が取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent は SWE と同じように動作するため、常に監視する必要はありません。完了後にレビューできます。続けて、カスタムエージェントの作成と使用を見ていきましょう。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当て、非同期で作業を任せることができます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに利用できます。

このレッスンでは、次のことを確認しました。

- コード生成環境をカスタマイズする方法
- 処理を安全に行う方法
- 範囲を明確に定めた Issue の重要性
- Copilot に Issue を割り当てる方法

cloud agent がバックグラウンドで作業する間、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。また、前のモジュールで扱ったカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
