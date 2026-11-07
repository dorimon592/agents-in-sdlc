---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys のコピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、[codespace][codespaces] を起動します。次のレッスンでは、統合ターミナルを使って Copilot CLI をインストールし、実行します。

このレッスンでは、次の作業を行います。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。
- Codespace を作成し、プロジェクトを使う準備ができていることを確認します。

## ラボ用リポジトリを設定する

Tailspin Toys プロジェクトの自分用コピーを使います。[テンプレートリポジトリ][tailspin-template]から今すぐ作成してください。新しいリポジトリには、ラボに必要なファイルがすべて含まれています。

1. 新しいブラウザーウィンドウで、[Tailspin Toys のテンプレート][tailspin-template]に移動します。
2. **Use this template**、次に **Create a new repository** を選択して、自分用のリポジトリを作成します。
3. GitHub または Microsoft が主催するイベントの一環としてワークショップに参加している場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot を利用できる Organization に新しいリポジトリを作成します。
4. 作成したリポジトリのパス（`organization-or-user-name/repository-name`）を控えておきます。ワークショップの後半で参照します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップ全体を通じてこの Issue を使うため、自分で起票する必要はありません。

ワークショップのテンプレートから新しいコピーを作成してください。テンプレートには、リポジトリの指示、アプリケーションコード、テスト、`quality-checks` スキル、演習で使うバックログが含まれています。古いコピーを使用する場合は、必要なファイルが含まれていることをファシリテーターに確認してください。

## Codespace を作成する

次に、Codespace を使ってワークショップを進めます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードの作成、実行、デバッグができるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応したフル機能のエディターを利用できます。

1. 新しく作成したリポジトリに移動します。
2. **Code** を選択します。
3. **Codespaces** タブを選び、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートによって、プロジェクトの依存関係、Playwright Chromium、ローカルデータベースがインストールされます。
5. **Do you trust the authors of the files in this folder?** と尋ねられた場合は、**Trust Folder & Continue** を選択します。
6. リポジトリのルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択して、Tailspin Toys サイトが表示されることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発サーバーを停止します。

## まとめと次のステップ

準備ができました。このレッスンでは、次の作業を行いました。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成しました。
- Codespace を作成し、プロジェクトを使う準備ができていることを確認しました。

次は、Codespace に [GitHub Copilot CLI][next-lesson] をインストールし、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
