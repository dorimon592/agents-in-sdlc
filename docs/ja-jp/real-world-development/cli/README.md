---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエストの自動化を学びながら、Tailspin Toys に 2 つの変更を実装、検証、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型コーディングアシスタントとして利用できます。グラフィカルエディターに切り替えることなく、コマンドラインからコードベースを調べ、コードを生成し、コマンドを実行し、外部ツールに接続できます。

このワークショップでは、Tailspin Toys の一連のワークフローに沿って作業します。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、プロジェクトの概要を把握します。
2. 星評価の小さな変更を加え、ブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージします。
3. フィルタリングの Issue から始め、Plan モードで方法を決め、Autopilot モードで作成し、Interactive モードでレビューします。
4. リポジトリの指示を更新し、フィルタリングの作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルタリングを確認します。
7. 品質保証 (QA) のカスタムエージェントを作成して使い、要件、カバレッジ、検証結果をレビューします。
8. フィルタリングの変更全体をレビューし、Agent Merge を使って PR をマージします。
9. コンテキスト、モデル、共有、cloud agent への任意の委任に使う便利なスラッシュコマンドを確認します。

ワークショップを簡潔にするため、星評価の変更と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルタリングの変更の、2 つの PR を作成します。フィルタリングと品質確認のワークフローでは、1 つの会話とブランチを使って、各ツールの使い方を学びながら作業を続けます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. Copilot CLI をインストールする][ex1] | インストール | Copilot CLI をインストールして認証し、使い始めます |
| [2. 星評価を追加する: 小さな改善から始める][ex2] | 最初の変更 | 既存の評価と評価がない場合の状態を表示し、最初の PR をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で作成して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot を案内する][ex4] | コンテキスト | 指示を確認して更新し、フィルタリングに適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行できるチェック | 既存のスキルを確認し、レポート形式を更新して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーによる確認 | CLI で MCP を設定し、フィルタリングの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門的なプロファイルを作成して選択し、最終的な検証結果を集めます |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使います |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認します |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、追加リソースを確認します |
| [任意: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイして Web サイトに接続します |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] リポジトリと Codespace を作成できる権限
- [ ] ターミナルまたはコマンドライン操作の基本的な知識

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## 開始する

**[前提条件から開始する →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
