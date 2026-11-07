---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、基本を確認して、用意されているフィルタリングの Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディングアシスタントです。コマンドラインからコードベースを調べ、コードを生成し、コマンドを実行し、外部ツールと連携できます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。まずはツールをインストールしましょう。使い慣れたツールでインストールできます。

このレッスンでは、次の作業を行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使って、フィルタリングの Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに対応するため、AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでその機能を利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、デバイスコードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。認証を求められずに Copilot CLI が起動した場合は、そのまま進めてください。

## 基本を確認する

通常のシェルプロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語の入力がエージェントに送信され、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押します。続けて、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みのバージョンで使用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 動作確認のため、Copilot に簡単な質問をします。

   ```plaintext
   このプロジェクトの主なファイルを教えてください。
   ```

4. 回答を読み、Copilot が回答前にリポジトリを調べていることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. Copilot にフィルタリングの Issue を探してもらいます。

   ```plaintext
   GitHub MCP を使って、このリポジトリで "Allow users to filter games by category and publisher" というタイトルの Issue を探してください。URL と簡単な概要を教えてください。何も変更しないでください。
   ```

7. URL を開いて Issue を読みます。最初の小さな変更を終えた後、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使います。自動的に worktree は作成されません。変更ごとに機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、許可済みの範囲外でツールを使う前に確認を求めます。このワークショップでは、Codespace 内で承認プロンプトを省いて演習に集中できるよう、ユーザーが承認したショートカット `--yolo` を付けて再起動します。

> [!CAUTION]
> `--yolo` は、すべての自動権限（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）を有効にします。Codespace や VM などの隔離された環境でのみ使用し、日常の開発で既定のエイリアスにしないでください。詳細は[ツールの使用を許可および拒否する][allow-all-warning]を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` によって、後のレッスンで Issue や pull request を操作するための読み取り・書き込み用 GitHub MCP ツールを有効にします。Codespace によってローカルコンピューターへのアクセスは制限されますが、認証済みの GitHub リソースには引き続きアクセスできます。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をして会話できることを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンで指示を変更したりエージェントを追加したりした後、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次の作業を行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使って、フィルタリングの Issue を見つけました。

次は、ゲームカードに星評価を表示する最初の変更を Copilot CLI に依頼します。[最初の対象を絞った変更][next-lesson]に進みます。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
