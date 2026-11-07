---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。具体的には、次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルタリング Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から始め、Plan モードで方法を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを案内し、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) のカスタムエージェントを作成して選択し、要件、テストカバレッジ、スキルの結果、ブラウザーでの検証結果を確認しました。
- フィルタリングの変更全体をレビューし、Agent Merge を許可して PR を作成しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## 提出した変更

このワークショップでは、更新済みの `main` からそれぞれ作成したブランチで、2 つの PR を完了します。

1. **星評価:** ゲームカードに既存の `starRating` と、評価がない場合の状態を表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用します。`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連テストを追加します。

フィルタリングを計画して PR を開くまで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作など、いくつかの方法でコードを確認しました。`quality-checks` スキルでユニットテスト、lint、型チェックを実行し、新しい形式で報告しました。QA は、その結果を要件とテストカバレッジのレビューにまとめてから PR を作成しました。

追加するテストは、実際の不足を解決する必要があります。QA の実行で新しいテストが不要と判断されることもあります。マージを許可する前に、コードと検証の根拠を確認してください。変更を加えた場合は、影響を受ける検証結果を更新します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。作成前に方法を検討するときは **Plan**、作業に参加しながら進めるときは **Interactive**、範囲の明確なタスクを任せるときは **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を作るのか、なぜ必要なのか、どのように進めるのかを明確に説明すると、結果が大きく変わります。

## さらに試す

基本的なワークフローは完了しました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビューエージェントに変更を分析してもらう。
- `/rubber-duck` で問題を説明し、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送信する。

## 次のステップ

どのツールでも、上達する一番の方法は使い続けることです。本番コード、趣味のコード、何年も構想していた小さなアプリなどで活用してください。チームと学びを共有し、チームからも学びましょう。そして、いつものようにドキュメントを確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
