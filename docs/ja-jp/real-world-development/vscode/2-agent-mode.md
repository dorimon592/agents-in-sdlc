---
title: "演習 2 - Copilot agent mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

最も簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は、必要なファイルを見つけ、変更し、テストを実行し、デバッグし、変更し忘れたファイルを見つけて、もう一度更新するという作業を繰り返します。

ここで役立つのが Copilot agent mode です。

Copilot agent mode は、IDE 内でより自律的に作業するよう設計されています。開発者と同様に、既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。agent mode を使ってサイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。agent mode は IDE 内で動作し、すばやいフィードバックとやりとりができます。cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じてやりとりするペアプログラマーとして設計されています。

この演習では、次のことを学びます。

- Copilot agent mode がプロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する方法
- GitHub Copilot agent mode で UI とデータレイヤーにわたる新機能を実装する方法
- コードベースにマージする前に、Copilot agent mode が生成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや発行元で絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。次は、Copilot agent mode を使って、データレイヤー、UI、テストの残りを実装します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームを一覧表示し、その詳細を表示できます。サイト全体は、ビルド時にページを静的 HTML として生成する単一の Astro アプリです。ページは、Drizzle ORM を通じてフロントマターからローカルの SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、必要な対話機能は標準 DOM API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを起動するには、単一の Astro 開発サーバーを起動する `npm run dev` タスクを使います。GitHub Codespace で次の手順を実行してください。

1. Codespace に戻ります。前の演習の後なので、`main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace で新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択し、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは、Web サーバーをホストしている Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトはすでに動作しますが、新しい機能で強化したいと考えています。まずはバックログを確認しましょう。テンプレートからプロジェクトを作成すると、Issue のバックログも自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** は、自律的に計画を立て、複数のファイルを変更し、ターミナルコマンドを実行して、ツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が、特定のモデルを推奨するものではありません。ワークショップの作成には Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードに一貫性を持たせ、快適に学べることを意図しています。ただし、LLM の動作は確率的なため、ワークショップの例とは異なる提案が表示されることがあります。これは通常の動作であり、想定されています。

> [!NOTE]
> LLM の動作は確率的なため、Copilot が異なる MCP コマンドを使う場合がありますが、タスクは同様に完了できます。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   GitHub リポジトリのバックログを表示してください。ユーザーに最も役立つものを基準に、優先順位を付けてください。
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が、ユーザーに最も役立つと考えた項目を優先順位付けしていることを確認してください。

## 指示ファイルを確認する

Copilot にコードを生成してもらう前に、作業のコンテキストを伝えるために使う指示ファイルを確認しましょう。Web サイトへの機能追加の進め方が記載された [ユーザーインターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加のガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則
   - Astro、Tailwind CSS、Drizzle など、ファイル形式ごとの指示ファイルへのリンク

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用しやすく小さな単位に分けたり、例やテンプレートを示したりできます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、アプリケーションに少なくとも 3 つの変更が必要です。

- データレイヤー (`src/lib/`) にフィルタリングのロジックを追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲームの一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前に、テストを実行してすべて成功することを確認する必要があります。Copilot agent mode にこれらを依頼しましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   バックログにある関連 GitHub Issue の要件に基づいて、発行元とカテゴリで絞り込めるようにサイトを更新してください。前の演習ですでに publishers ヘルパーを追加しているため、その作業を維持し、必要に応じて調整しながら、データレイヤー、UI、テストの残りを完成させてください。完了前に、すべてのテストが成功することを確認してください。サーバーはすでに実行中なので、起動する必要はありません。
   ```

6. Copilot がプロジェクトを調べ、対象となるファイルを見つけて作業を始める様子を確認します。データレイヤーのヘルパー、UI、テストを探した後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによっては、数分かかることがあります。その間、エディターに **Keep** と **Undo** ボタンが表示される場合があります。作業中に選択する必要はありません。完了後、すべての変更に対して選択できるようになります。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![テスト実行の確認を求める Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストの実行を繰り返し、タスクが完了してエラーが見つからなくなるまで進めます。

   ![Copilot agent mode のセッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作を確認し、コードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot agent mode を使って Tailspin Toys の Web サイトに機能を追加する方法を学びました。次のことを確認しました。

- GitHub Copilot agent mode で UI とデータレイヤーにわたる新機能を実装する方法
- Copilot agent mode がプロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する方法
- コードベースにマージする前に、Copilot agent mode が生成した変更とテストをレビューする方法

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### 追加の演習 — ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを追加するよう依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、値を固定するかなど、ページネーションの動作を決めます。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- GitHub Copilot にコンテキストを追加したり変更を依頼したりして、何度か反復する場合があります。Copilot を使うときには一般的な流れです。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
