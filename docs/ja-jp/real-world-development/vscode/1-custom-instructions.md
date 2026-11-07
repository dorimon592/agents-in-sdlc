---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: agent mode →][next-lesson]

生成 AI を使う際、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを共有する方法はいくつかあります。その中心となるのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成の指針として、何を実装するかだけでなく、どのように実装するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- *現在の*指示で、フィルタリング用の最初のデータスライス（publishers ヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 次の演習で Copilot が更新済みのガイダンスを使えるよう、指示の更新とフィルタリングの最初のスライスをコミットして `main` に反映します。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は確率的です。この演習の目的は、指示を更新した後の動作の*傾向*を確認することであり、出力を 1 文字ずつ一致させることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。具体的には次のとおりです。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードのドキュメントには、TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発慣行に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、コードに含めたいコメントなどを指定でき、より適切な提案やコードスニペットを得られます。リポジトリ全体、または特定のファイルを対象にした指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発する内容の概要、ベストプラクティスなど、Copilot とのほとんどのチャットや CLI のやりとりに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類を対象に作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加などに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用されます。コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使います。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。[対応する指示ファイルの形式][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法の詳しい説明は、このワークショップの範囲外です。ただし、サンプルプロジェクトでは一般的な管理方法を確認できます。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、開発するものの説明、プロジェクトの構成、全体的なコーディング標準など、プロジェクト全体に関する指針を記載します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル形式や、特定のタスクに関する指針に使用します。
- 自然な言葉で、明確なガイダンスを記載します。コードの良い例と悪い例を示します。

指示ファイルの作成方法は 1 つではありません。AI の使い方と同じように、試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトの指示ファイルを見ると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあることがわかります。
>
> Copilot に指示ファイルを作成してもらうこともできます。手順は環境によって異なります。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。詳しくは、各環境のレッスンで該当する箇所を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェントなどのリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。VS Code のエディターまたは GitHub Web UI で開き、内容を確認しましょう。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針にも注目してください。これらの指示は、Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob（リポジトリルートからの相対パス）を設定します。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示セットを小さな再利用可能ファイルに分け、コード生成の参考例を Copilot に示せます。ここに記載されたパスは、リポジトリルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーコメントのルールを追加します。

## 変更用ブランチを作成する

ここでは、ベストプラクティスに従って作業用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前に* Copilot Chat を使う

カスタム指示の影響を確認するため、まず現在の指示ファイルでプロンプトを実行します。その後、指示を更新して、同じプロンプトをもう一度送信します。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

1. 前の演習から開いているエディタータブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に伝えます。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   `src/lib/publishers.ts` に、各 publisher の名前と ID を含む一覧を返すデータアクセスヘルパーを作成または更新してください。ファイルに変更を適用してください。
   ```

5. Copilot はプロジェクトを調べ、ヘルパーファイルとテストを含むコードの変更を適用します。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取る型付き関数であり、型付きの publisher 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確率的なため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合でも問題ありません。重要なのは、指示の更新後に一貫性が向上することです。

## リポジトリ全体に適用する標準を追加する

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を伝えるためのファイルです。コードの提案を改善するために、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加して、ファイルヘッダーコメントと TSDoc のドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - エクスポートされたすべての関数に、目的、パラメーター、戻り値を説明する TSDoc コメントを追加します。
   - import 文やコードより前に、ファイルの目的を説明するコメントブロックを追加します。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル（`.github/copilot-instructions.md`）に置いて全体に適用することも、特定の言語、ファイル形式、タスク向けの `*.instructions.md` ファイルとして作成することもできます。先ほど追加したドキュメントコメントのルールのように、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を再度開き、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   `src/lib/publishers.ts` に、各 publisher の名前と ID を含む一覧を返すデータアクセスヘルパーを作成または更新してください。ファイルに変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys のクラウドファンディングプラットフォーム向けの publisher データアクセスヘルパーです。
    * データベースから publisher の情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * ID と名前を含むすべての publisher の一覧を返します。
    *
    * @param db - Drizzle データベースクライアントです。
    * @returns publisher オブジェクトの配列に解決される Promise です。
    */
   ```

Copilot に新しいプロジェクト標準を適用させ、次の演習につながる実際のコードに反映できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルも、ほかのリポジトリアセットと同じようにソース管理します。更新した指示とヘルパーの変更をコミットして、リポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、指示とヘルパーの変更があることを確認します。
3. 指示の更新と、ヘルパーの基礎となるファイルをステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けたプルリクエストを作成します。手動で作成する代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に依頼できます。Issue を検索し、その Issue を完了する PR を作成してから、マージを依頼しましょう。

> [!NOTE]
> Copilot が GitHub 上の作業を行うのは、この演習が初めてです。プロジェクトのテンプレートでは、GitHub ツールがすでにワークスペースに接続されています。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従って認証してください。後の演習では、この接続の仕組みである Model Context Protocol について説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と publishers ヘルパーの土台の両方を説明に含めます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を検索し、PR を作成します。
5. Copilot が GitHub の操作を許可するよう求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、PR をマージしてブランチを `main` に戻すよう、次のプロンプトで依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub の操作やシェルコマンドの実行を許可するよう求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の助けを借りて、プルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- コード生成プロンプトを送信し、*既存の*指示で Copilot が何を提案するかを確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 次の演習で引き続き作業できるよう、指示の更新とヘルパーの土台を `main` にコミットしました。

次は、Copilot がコードベース全体に新機能を追加する[agent mode][next-lesson]で、指示を実際に活用します。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
