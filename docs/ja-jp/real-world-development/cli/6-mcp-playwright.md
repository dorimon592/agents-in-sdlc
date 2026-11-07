---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、必要な作業はあります。データや外部サービスを扱い、Copilot が利用できる自動化機能を追加する必要もあります。そこで役立つのが MCP サーバーです。MCP サーバーを使うと、Copilot CLI に組み込まれていないツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI でどのように使うかを確認します。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認します。

## シナリオ

ユニットテストとエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。ユーザーと同じように Copilot に Web サイトを使わせ、変更が期待どおりに動くという確信を高めましょう。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信する方法を提供します。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムにやりとりできます。最新情報にアクセスしたり、ユーザーに代わって操作を行ったりできます。

ツールやリソースには MCP サーバーを通じてアクセスします。MCP サーバーは AI エージェントと外部のツールやサービスの間をつなぎます。各 MCP サーバーは、AI エージェントが使えるツールやリソースのセットを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも、さまざまなツールやリソースを使える MCP サーバーがあります。GitHub は、エコシステムの発見性とコントリビューションを高めるために [MCP registry][mcp-registry] を公開しています。

> [!CAUTION]
> MCP サーバーはプロジェクト内のほかの依存関係と同じように扱ってください。使用する前に、ソースコードを確認し、公開元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP が設定されている場合は、セットアップ手順を省略して、次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd>、次に <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って機能を試す

先ほどは、機能が期待どおりに動くことを手動で確認しました。今度は、Copilot に利用可能な Playwright MCP サーバーを使って同じ確認をしてもらいましょう。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   アプリを起動し、Playwright MCP を使って、Issue と計画に照らしながらフィルタリング機能を確認してください。変更は加えず、正常に動作する点とそうでない点を教えてください。確認が終わったら、起動したサーバーを停止してください。
   ```

> [!NOTE]
> 通常は MCP サーバーの使用を Copilot に明示しなくても、Copilot が自動的に判断します。ただし、使用するツールがわかっている場合は、明示してもかまいません。適切な方向に導くことで、より一貫した結果を得られ、トークンも節約できます。

2. ブラウザーで機能を確認するため、Copilot がどのような手順を実行するかを見ます。
3. レポートを読み、すべて期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

おめでとうございます。Playwright MCP サーバーを使い、Copilot CLI から実際のブラウザーで機能を確認しました。具体的には、次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI でどのように使うかを確認しました。
- まだ利用できない場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認しました。

次は、スキルとブラウザーツールを専門的な役割としてまとめる [QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜいま注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
