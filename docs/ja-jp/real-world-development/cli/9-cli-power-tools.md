---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュコマンドが多数あります。高度な機能、内部情報、追加の設定オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドも確認しましょう。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で、利用可能なモデルを確認します。
- `/share` でセッションを書き出したり、共有したりする方法を学びます。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の中核的なワークフローを終えました。ここでは、コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大きなタスクや複雑なタスクに取り組むと、モデルの最大コンテキストウィンドウに近づくことがあります。必要に応じて Copilot CLI が会話を自動的に圧縮します。また、スラッシュコマンドを使って状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻ります。Copilot CLI がまだ起動していない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して、結果を比較します。会話が短い場合は、大きな違いが見られないことがあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。`/compact` を使うと、実行するタイミングを選べます。関連のない作業に切り替える場合は、`/clear` または `/new` で会話を新しくしてください。

## モデルを選ぶ

モデルにはそれぞれ異なる強みがあり、開発者の好みも異なります。Copilot CLI でモデルを一覧表示し、使用するモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、全員が AI ツールをより効果的に活用できます。`/share` コマンドで、セッションを Markdown または HTML ファイルに書き出したり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンに表示される `/share` のオプションを確認します。
2. セッションを共有する場合は、たとえばローカルの Markdown ファイルに書き出す `/share file` など、適切な出力先を選びます。
3. 書き出した内容を確認してから、送信または公開します。セッションの書き出しには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を拡張または委任する

ワークショップの中核部分は完了しました。Copilot CLI には、大きなタスクに対応するためのコマンドもあります。

- `/fleet` は、独立したサブタスクを複数のサブエージェントに分け、並行して実行します。
- `/worktree` は、別のタスク用に分離された Git worktree を作成します。
- `/delegate` は、タスクを Copilot cloud agent に送信します。cloud agent は非同期に作業し、プルリクエストを作成する場合があります。

これらのコマンドは追加の worktree やリモート作業を開始するため、任意としています。使用する前に、新しく範囲の明確なタスクを用意し、通常のワークフローで結果をレビューしてください。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部で起きていることを確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` で、利用可能なモデルを確認しました。
- `/share` でセッションを書き出したり、共有したりする方法を学びました。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

利用できるスラッシュコマンドはほかにもあります。最後に[学んだことと次のステップを振り返り][next-lesson]、Copilot CLI の学習を続ける方法を確認しましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
