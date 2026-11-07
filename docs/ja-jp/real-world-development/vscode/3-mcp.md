---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode でフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を学びます。
- ワークスペースに Playwright MCP サーバーを追加します。
- ブラウザーでフィルタリング機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントがエディターの外にあるツールを使えると、agent mode の能力が大きく向上します。Model Context Protocol (MCP) は、Copilot が外部ツールやサービスとやりとりするための標準です。

![agent mode の内部動作と、コンテキスト、LLM、ツール（MCP サーバーと VS Code 拡張機能が提供するツールを含む）とのやりとりを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信する手段を提供します。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムにやりとりできます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりできます。

これらのツールやリソースには、MCP サーバーを通じてアクセスします。MCP サーバーは AI エージェントと外部のツールやサービスをつなぎます。MCP サーバーは、既存の API や NPM パッケージのようなローカルツールなど、AI エージェントと外部ツールの通信を管理します。各 MCP サーバーは、AI エージェントが使えるツールやリソースのセットを提供します。

すでに広く使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントは、リポジトリの作成と更新、Issue やプルリクエストの管理などを行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンの選択などを行えます。

ほかにも、さまざまなツールやリソースを利用できる MCP サーバーがあります。GitHub は、エコシステムの発見性とコントリビューションを高めるために [MCP registry](https://github.com/mcp) を公開しています。

> [!CAUTION]
> セキュリティの観点では、MCP サーバーはプロジェクト内のほかの依存関係と同じように扱ってください。使用する前に、ソースコードを確認し、公開元を検証して、セキュリティへの影響を検討してください。信頼できる MCP サーバーのみを使用し、機密性の高いリソースや操作へのアクセスを許可する際は慎重に判断してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクトのテンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールするものはありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

ここで 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できるブラウザーを提供するため、機能のテストに適しています。

1. `.vscode/mcp.json` に `github` と並べて `playwright` の設定を追加します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグにより、Playwright は表示ウィンドウなしでブラウザーを実行します。デスクトップ表示がない Codespace 内では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトは、エンドツーエンドテストですでに Playwright を使用しているため、必要なブラウザーも通常はインストール済みです。後で Copilot からブラウザーがないと報告された場合は、`npx playwright install chromium` を実行してから再試行します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて使うときに VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` の設定の上に **Start** が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. サーバーを信頼するか確認されたら、設定を確認し、サーバーを起動できるよう信頼を選択します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを初めて使うときに VS Code がサーバーを起動し、信頼の確認を表示します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。自分でアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用して、その結果を確認して報告します。会話を離れずに機能を確認できるため、期待どおりの動作を確かめる最も速い方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使って動作します。ボタン、リンク、リスト項目など、構造化されラベルの付いた要素を支援技術と同じように扱います。そのため、簡単な機能確認がアクセシビリティの簡易チェックも兼ねます。

サーバーを接続し、アプリを起動したら、次のプロンプトを Copilot に送り、フィルタリング機能を確認します。

```text
Playwright MCP サーバーを使って、実行中のアプリを http://localhost:4321 でブラウザーに開き、新しいゲームのフィルタリング機能を確認してください。

1. ゲームページに移動し、表示されているゲームの数を確認します。
2. カテゴリフィルターを適用し、そのカテゴリのゲームだけが一覧に表示されることを確認します。
3. フィルターを解除してから発行元フィルターを適用し、その発行元のゲームだけが一覧に表示されることを確認します。
4. カテゴリフィルターと発行元フィルターを組み合わせ、両方の条件が結果に反映されることを確認します。

各手順で確認した内容を報告し、期待どおりでない動作があれば指摘してください。
```

Copilot は Playwright MCP サーバーからブラウザーを起動し、各手順を実行して確認内容を報告します。その要約を Issue の受け入れ条件と照らし合わせます。想定どおりでない場合は、プルリクエストを開く前に質問したり、コードの修正を依頼したりします。

> [!NOTE]
> このテストでは、アプリを `http://localhost:4321` で実行している必要があります。開発サーバーを停止していた場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行してから再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動くことを確認できたので、チームでレビューするためのプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が加えた変更を確認します。
2. **+** アイコンを選択して変更をステージングします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストは github.com や GitHub コマンドラインインターフェイス (CLI) などで作成できます。ここでは、GitHub Copilot に作成してもらいましょう。関連する Issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントドロップダウンで **Agent** が選択され、Copilot が GitHub ツールを使えることを確認します。
3. Copilot に次のように依頼して、PR を作成します。

   ```plaintext
   カテゴリと発行元でのフィルタリングに関するリポジトリの Issue を探してください。現在のブランチから新しいプルリクエストを作成し、該当する Issue に関連付けてください。
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報を収集して処理を実行できるようにします。GitHub ツールを初めて使うときに認証を求められた場合は、画面の指示に従います。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が表示したリンクを選択して PR を確認します。**まだマージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を学びました。
- ワークスペースに Playwright MCP サーバーを追加しました。
- 出荷前に、ブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

MCP サーバーで機能を*テスト*しましたが、MCP は Copilot を拡張する方法の 1 つです。次は[カスタムエージェントを作成し][next-lesson]、アクセシビリティレビューなど、対象を絞った作業を効率化します。

## リソース

- [MCP とは何か、なぜいま注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
