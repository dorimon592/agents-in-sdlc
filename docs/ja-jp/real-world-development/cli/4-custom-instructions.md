---
title: "レッスン 4 - カスタム指示で Copilot を案内する"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際、コンテキストが重要です。特定の方法でタスクを実行したい場合は、Copilot がその指針を参照できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、コードの構成方法も記載します。フィルタリング機能を作成したので、Copilot が使用した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス指定の指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準が守られるように指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。具体的には次のとおりです。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数では、`db` の注入可能な引数がある場合はそれも含めて、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連するコードが変わったらコメントも更新します。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿ってタスクを行えるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot が理解できるよう、コンテキストや好みを伝えられます。コーディング規約、ライブラリ、コードに含めたいコメントを指定して、より適切な提案やコードスニペットを得ることができます。リポジトリ全体、または特定のファイルを対象にした指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスクのガイドラインを記載するファイルです。

> [!NOTE]
> ほかの指示形式やサポート状況は、環境によって異なります。特定の形式を使う前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

変更の影響を確認できるよう、スタータープロジェクトにはいくつかの指示ファイルが含まれています。変更を加える前に、内容を確認しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob を設定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。指示を小さな再利用可能ファイルに分け、Copilot に参考例を示す方法がわかります。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは良い出発点ですが、まだ不足があります。中心となる `copilot-instructions.md` ファイルを更新し、新しく生成される TypeScript に TSDoc コメントが追加されるようにします。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. 次の行を末尾の箇条書きとして追加します。

   ```markdown
   - 新しく作成する TypeScript には、ドキュメントとして TSDoc コメントを含めてください。
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新を保存したら、フィルタリングの会話を終了して再開します。機能のコンテキストを保ったまま、新しい指示を読み込めます。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   指示された場合はフィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   指示とコードのガイダンスを更新しました。生成したコードを新しいガイダンスに合わせて更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、機能に新しい標準を適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパス指定の指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準が守られるように指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な quality-checks スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
