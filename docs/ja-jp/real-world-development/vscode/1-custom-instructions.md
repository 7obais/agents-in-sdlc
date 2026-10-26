---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot にコード生成の指針を伝える方法の 1 つが[命令ファイル][instruction-files-overview]です。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパス指定の命令ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わるしくみを確認します。
- 現在の命令でフィルター機能のデータ スライスの最初の部分 (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成されたコードに新しい標準が反映されることを確認します。
- 命令の更新とフィルター機能のスライスを `main` にコミットし、次の演習で Copilot が更新後のガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードが設定した標準から外れることがあります。Copilot の出力は確定的ではありません。この演習の目的は、命令を更新した後に動作がどのように変化するかを確認することです。出力を一字一句同じにすることではありません。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ レイヤーには常にユニット テストが必要です。
- UI はダーク モードにし、現代的な見た目にします。
- コード内ドキュメントとして TSDoc コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使って、Copilot がこれらのプラクティスに沿って作業するために必要な情報を伝えます。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件のコンテキストを Copilot に伝え、関連性の高い提案やコード スニペットを得られます。好みのコーディング規約、ライブラリ、追加したいコメントの種類などを指定できます。リポジトリ全体、または特定の種類のファイルに対して命令を作成できます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の命令ファイルです。Copilot Chat や CLI での多くのリクエストに関係する、プロジェクト全体の情報を記載します。たとえば、技術スタック、開発中の内容、ベスト プラクティス、全般的なガイダンスなどです。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に関するガイドラインを記載します。TypeScript や Astro などの言語のほか、UI コンポーネントやユニット テストの作成などのタスクを指定できます。

> [!NOTE]
> IDE で作業する場合、命令ファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集の提案には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令ファイルと、`applyTo` フロントマターがある `*.instructions.md` ファイルの両方をコード生成に使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの規格の命令ファイルにも対応しています。[カスタム命令のサポート][custom-instructions-support]をご覧ください。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成方法のすべてをこのワークショップで説明することはできませんが、サンプル プロジェクトには一般的な方法が示されています。基本的には、次の点に留意してください。

- `copilot-instructions.md` には、開発中の内容、プロジェクト構成、全体的なコーディング標準など、プロジェクト全体に適用するガイダンスを記載します。
- `*.instructions.md` は、ユニット テスト、Astro コンポーネント、データ レイヤーなど、ファイルの種類やタスクに固有の命令に使います。
- 自然な言葉で明確なガイダンスを記載します。コードの良い例と悪い例を示すのも効果的です。

AI の使い方に決まった方法がないように、命令ファイルの作り方にも唯一の正解はありません。プロジェクトで試しながら、効果的な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した命令ファイルを用意することをお勧めします。このプロジェクトには、[UI の命令][ui-instructions]や [Astro の命令][astro-instructions]など、さまざまなタスクに対応したファイルがあります。
>
> Copilot に命令ファイルの作成を依頼することもできます。環境によって手順は異なります。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。各環境のレッスンで、関連する手順を説明します。
>
> テンプレートや出発点を探している場合は、命令ファイル、カスタム エージェント、その他のリソースが集まった [awesome-copilot][awesome-copilot] をご覧ください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[instruction-files-overview]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてみましょう。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスにも注目してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle のデータ レイヤー、テストなどに関する命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、命令を適用するファイルを指定する glob です。ここでは、`**/*.test.ts` などに一致する TypeScript のテスト ファイルが対象になります。
5. このプロジェクトでユニット テストを作成するための命令を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの命令ファイルや、既存のプロジェクト ファイルへのリンクを確認します。これにより、大きな命令セットを再利用可能な小さいファイルに分け、Copilot に参考例を示せます。ここにあるパスはリポジトリのルートではなく、命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で、TSDoc コメントとファイル先頭のコメントを追加するルールを設定します。

## 変更用のブランチを作成する

ここでは、ベスト プラクティスに従って変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

カスタム命令の効果を確認するために、まず現在の命令ファイルを使ってプロンプトを送ります。その後、命令を更新して同じプロンプトを再送し、違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを読み取るようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選び、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新します。多くの場合、ヘルパー ファイルとテストの両方が変更されます。
6. 提案されたヘルパーは、最初の引数として `db` クライアントを受け取り、型付きの publishers 配列を返す関数です。これは `.github/instructions/drizzle.instructions.md` (`src/lib/*.ts` に適用) のデータ レイヤー規約によるものです。
7. 提案されたコードには TSDoc コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示しなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。命令を更新した後に一貫性が高まることが、この演習のポイントです。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` はプロジェクト全体の情報を Copilot に伝えるためのファイルです。コード提案を改善するため、リポジトリのコーディング標準を追加します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。そのため、生成されたヘルパーにはドキュメント コメントがありませんでした。
3. 既存の標準のすぐ下に、ファイル先頭のコメントと TSDoc コメントを追加する次の Markdown を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、命令ファイルはリポジトリ レベルの `.github/copilot-instructions.md` に作成して全体に適用することも、言語、ファイルの種類、タスクに固有の `*.instructions.md` として作成することもできます。ドキュメント コメントのルールなど、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## プロンプトをもう一度実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されたことを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を守らせ、次の演習で使う実際のコードに適用できました。

## 命令をコミットしてブランチをプッシュする

命令ファイルもほかのアセットと同様に、ソース管理で管理します。変更をコミットし、ブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、命令の更新とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. ターミナルで命令の更新とヘルパーの基盤となる変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連ファイル (テストなど) が表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、プルリクエストを作成し、バックログにあるドキュメント標準の Issue と関連付けます。手動で操作することもできますが、GitHub Copilot に任せてみましょう。Copilot に Issue を見つけさせ、それを解決する PR を作成して、マージします。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは、これが初めてです。プロジェクト テンプレートには GitHub のツールがすでに接続されています。そのため、設定は必要ありません。Copilot が初めてツールを使うとき、VS Code から GitHub へのサインインを求められる場合があります。指示に従って許可してください。後の演習では、この接続がどのように機能するか (Model Context Protocol) を学びます。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選ばれていることを確認します。
3. 命令ファイルの更新に関連する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR には命令の更新と publishers ヘルパーの基盤の両方を記載します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を検索して PR を作成します。
5. Copilot が GitHub の操作を許可するよう求めたら、コマンドを確認し、適切な場合に **Allow** を選択します。
6. PR が作成されたら、PR をマージしてブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub の操作とシェル コマンドを許可するよう求めたら、コマンドを確認し、適切な場合に **Allow** を選択します。

GitHub Copilot の助けを借りて、プルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot が命令ファイルからコンテキストを取得するしくみを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の命令を使ってコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が適用されることを確認しました。
- 命令の更新とヘルパーの基盤を `main` にコミットし、次の演習で利用できるようにしました。

次は[エージェント モード][next-lesson]で、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
