---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリングの検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、必要な作業はあります。これまでは、コードが動作することを手動で検証し、指示ファイルを使って標準に沿うようにしてきました。テストや lint、継続的インテグレーション (CI) のそのほかの作業はどうでしょうか。

このような作業には**エージェントスキル**が適しています。スキルは、テストなどの処理を適切に実行する方法を Copilot に伝えます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使います。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルには、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースが含まれます。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` で構成されます。Markdown ファイルには、スキルの名前と説明を定義するフロントマター、概要、呼び出すタイミングに関する指針を記載します。必要に応じて、スキルが実行するスクリプトや利用するリソースをサブフォルダーに含めることができます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルはプロジェクトの既存コマンドを使うため、`SKILL.md` のみで構成されています。

スキルはプロジェクトの `.github/skills` フォルダーに配置すると、チームで共有して再利用できます。また、ユーザーのスキルフォルダー `~/.copilot/skills` に配置することもできます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces のエディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明により、Copilot はいつスキルを呼び出すかを判断できます。
3. 指示を読み、テストと lint の手順を Copilot にどう伝えているかを確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接、または自然言語を使ってスキルを呼び出せます。Copilot に 3 つのチェックを実行してもらいましょう。

1. Interactive モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   `quality-checks` スキルを実行して、ユニットテスト、lint、型チェックを行ってください。
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが報告した内容を確認できる、より良いレポートが必要です。レポートを作成するようスキルを更新しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## 結果レポートの形式

   完了したら、実行した各コマンドと、その結果が成功、失敗、またはブロックのいずれかを報告してください。テスト数、所要時間、エラー、警告、その他の指標は、ツールが報告した場合にのみ含めてください。失敗やブロックがある場合は、次の対応を明記してください。スキップまたは未完了のチェックを成功と説明しないでください。
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、スキルを実行してみましょう。Copilot CLI は、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを入力します。

   ```plaintext
   `quality-checks` スキルを実行して、ユニットテスト、lint、型チェックを行ってください。
   ```

3. 最後のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェック向けの `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、機能の PR に含めます。次は [Playwright MCP サーバー][next-lesson]を通じて、Copilot がサイトと直接やりとりできるようにします。

## スキルの例

次のコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート用スクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
