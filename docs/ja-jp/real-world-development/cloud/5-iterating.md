---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能の追加に関するいくつかのタスクで、GitHub Copilot と作業してきました。コードへのドキュメント追加、デザインチームが反復できる関連ゲーム機能の作成、ハイコントラストモードとライトモードを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要であれば Copilot にフィードバックを送って作業を改善しましょう。その後、チーム全体のコードレビューを有効にします。

### シナリオ

これまでに何度も説明したように、生成 AI を利用してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業する必要があります。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関して GitHub Copilot が提案した内容を確認しましょう。

## GitHub Copilot cloud agent とセキュリティ

Copilot cloud agent は非同期に、監督されずにタスクを実行するため、安全性を保つための制約が設けられています。次のような制約があります。

- Copilot はリポジトリに対して読み取り権限を持ち、書き込みできるのは作業用のブランチ**のみ**です。
- cloud agent は GitHub Actions 内で動作します。作業用に個別の一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも制限の対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールによってブロックされているという警告が表示される場合があります。Copilot は、外部 MCP サーバーの呼び出しなど、既定で外部リソースへのアクセスが制限されているため、これは**想定される動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** の表示があります。その場合は、Copilot の作業が完了するまで待ってください。数分かかることがあります。その間に休憩したり、ここまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TSDoc のドキュメントコメントや、その他のドキュメントを含むコードを確認します。正確な変更内容は状況によって異なります。

    変更を確認するときは、演習 2 の最初に確認したカスタム指示ファイルによる、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了するまで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するとき、一方向に作業を進めるだけではありません。チームのほかのメンバーと同じように、プルリクエストやコードの行単位のコメントで Copilot にメンションできます。Copilot はコメントを確認し、新しいセッションを開始して対応します。結果は決定論的ではないため、必ず期待どおりの結果になる具体的な文面はありません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例や、コードベースに追加したいドキュメントについての別の提案を使い、**@copilot** にメンションして生成されたドキュメントへの変更を依頼するコメントを追加します。
2. **View Session** を選択して Copilot の作業を確認します。更新のために新しいセッションが始まります。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが追加されます。
5. **Files changed** タブを選択して変更をレビューします。

問題がなくなるまで、何度か反復してもかまいません。作業が完了したら、PR をドラフトからレビュー可能な状態に変更して、`main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了するまで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストすることもできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます。例: **copilot/fix-8**）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に追加の変更を依頼して、作業を反復できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc のドキュメントコメントを追加したりできます（カスタム指示を更新する**前**に割り当てたタスクであることに注意してください）。前の PR と同様に、**Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで追加を依頼したライトモードが含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** の表示があります。その場合は、Copilot の作業が完了するまで待ってください。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点に注目してください。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保存する local storage の使用
   - ハイコントラストモードとライトモードに関する CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了するまで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードを切り替えてください。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用のカスタムエージェントが、アクセシビリティのベストプラクティスに沿った機能の実装を Copilot に促したことがわかります。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 作業の委任を続ける

cloud agent を活用するには、バックログの実際の Issue を割り当て、その間にほかの作業に取り組むと効果的です。練習として、リポジトリに Issue を追加して Copilot に割り当ててみてください。次のような内容が考えられます。

- ゲームの詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。レッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の出力にどのように影響するかを理解しました。**
- **Issue を Copilot cloud agent に割り当て、非同期に環境を準備し、計画して、作業する様子を確認しました。**
- **カスタムエージェントを作成して使用し、ハイコントラストモードとライトモードを追加しました。**
- **Copilot エージェントページをミッションコントロールとして使用し、アクセシビリティのセッションを監視して途中で指示を追加しました。**
- **cloud agent が作成したプルリクエストをレビューして反復し、`@copilot` に変更を依頼してワークフローを承認しました。**

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで今回の内容を発展させられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat の agent mode と MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` ブリッジを使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリで、次のことも試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
