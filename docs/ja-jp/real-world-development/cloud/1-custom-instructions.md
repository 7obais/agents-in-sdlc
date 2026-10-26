---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、Copilot にタスクの内容だけでなく、実行方法も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパス指定の命令ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加します。

> [!NOTE]
> VS Code と CLI のハーネスとは異なり、ここでは命令を更新する前後に同じプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期に作業するため、影響をリアルタイムに比較するのは難しいからです。このハーネスの後半で cloud agent が作成するプルリクエストをレビューするときに、命令ファイルの効果を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには常にユニットテストが必要です。
- UI はダーク モードにし、現代的な見た目にします。
- コードのドキュメントとして TSDoc コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使って、Copilot がこの方針に沿って作業するために必要な情報を与えます。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件のコンテキストを Copilot に与え、提案を改善できます。コーディング規約、ライブラリ、含めたいコメントの種類などを指定できます。リポジトリ全体、または特定の種類のファイルに命令を作成できます。

命令ファイルには、次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の命令ファイルです。Copilot Chat や CLI での多くのリクエストに関係する、プロジェクト全体の情報を記載します。たとえば、技術スタック、開発中の内容、ベスト プラクティス、全般的なガイダンスなどです。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に関するガイドラインを記載します。TypeScript や Astro などの言語や、UI コンポーネントやユニットテストの作成などのタスクを指定できます。

> [!NOTE]
> IDE で作業する場合、命令ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集の提案には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令ファイルと、`applyTo` フロントマターがある `*.instructions.md` ファイルの両方をコード生成に使います。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの規格の命令ファイルにも対応しています。[カスタム命令のサポート][custom-instructions-support]をご覧ください。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成方法のすべてをこのワークショップで説明することはできませんが、サンプル プロジェクトには一般的な方法が示されています。基本的には、次の点に留意してください。

- `copilot-instructions.md` には、開発中の内容、プロジェクト構成、全体的なコーディング標準など、プロジェクト全体に適用するガイダンスを記載します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データレイヤーなど、ファイルの種類やタスクに固有の命令に使います。
- 自然な言葉で、明確なガイダンスを記載します。コードの良い例と悪い例を示すのも効果的です。

AI の使い方に決まった方法がないように、命令ファイルの作り方にも唯一の正解はありません。プロジェクトで試しながら、効果的な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した命令ファイルを用意することをお勧めします。このプロジェクトには、[UI の命令][ui-instructions]や [Astro の命令][astro-instructions]など、さまざまなタスクに対応したファイルがあります。
>
> Copilot に命令ファイルの作成を依頼することもできます。各環境で手順は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。該当するレッスンで説明します。
>
> テンプレートや出発点を探している場合は、命令ファイル、カスタム エージェント、その他のリソースが集まった [awesome-copilot][awesome-copilot] をご覧ください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。Codespace または GitHub Web UI で開いてみましょう。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の中にある **GitHub Actions Workflows** のガイダンスにも注目してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle のデータレイヤー、テストなどに関する命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、命令を適用するファイルを指定する glob です。ここでは、`**/*.test.ts` などに一致する TypeScript のテスト ファイルが対象になります。
5. このプロジェクトでユニットテストを作成するための命令を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの命令ファイルや、既存のプロジェクト ファイルへのリンクを確認します。これにより、大きな命令セットを再利用可能な小さいファイルに分割し、Copilot に参考例を示せます。ここにあるパスはリポジトリのルートではなく、命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で、TSDoc コメントとファイル先頭のコメントを追加するルールを設定します。

## プロジェクト全体の標準を追加する

ここで行う編集は、コード内ドキュメントとして TSDoc コメントとファイル先頭のコメントを追加する、プロジェクト全体のルールです。次の演習で Issue を割り当てたときに、cloud agent がこのルールを使います。

編集する前にブランチを準備します。cloud agent は Issue の対象ブランチにある命令を読み込みますが、ここでは変更を独立してコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

先ほど説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を伝えるファイルです。コーディング標準を記載し、コード提案を改善しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを探します。コーディング標準が記載されていますが、コード内ドキュメントのルールはありません。先ほど生成されたヘルパーにドキュメント コメントがなかったのは、そのためです。
3. 既存の標準のすぐ下に、ファイル先頭のコメントと TSDoc コメントを追加する次の Markdown を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、命令ファイルはリポジトリ レベルの `.github/copilot-instructions.md` に作成して全体に適用することも、言語、ファイルの種類、タスクに固有の `*.instructions.md` として作成することもできます。コード コメントのルールなど、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## 命令の更新をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある命令ファイルを読み込みます。次の演習で `main` の Issue を Copilot に割り当てるため、cloud agent がこの命令を使えるように、変更を `main` にマージする必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で Issue を割り当てるときに、cloud agent はこの命令を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業しても構いません。その場合はブランチの手順を省略し、`main` に直接コミットします。ここでブランチを使うのは、実際のプロジェクトでの作業方法を再現するためです。

## まとめと次のステップ

Copilot が命令ファイルからコンテキストを取得するしくみを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスで cloud agent が作成するプルリクエストを確認すると、その標準がどのように使われたかを確認できます。

次は[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
