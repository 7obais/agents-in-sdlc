---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント標準を追加して、フィルタリング機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使ううえで、コンテキストは重要です。特定の方法でタスクを実行する必要がある場合は、そのガイダンスを Copilot が利用できるようにします。[命令ファイル][instruction-files]では、求めるコードの内容だけでなく、構成方法も指定できます。フィルタリング機能を構築したので、Copilot が使用した命令を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの命令とパス指定の命令ファイルがエージェントに適用されるしくみを確認します。
- コーディング標準に従うよう命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントでは、コードをそのまま説明するのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、`db` を注入可能な引数として含む場合はそれも含めて、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。
- 再利用可能な Astro コンポーネントでは `Props` の仕様を記述し、関連コードが変更されたらコメントも更新します。
- 既存のフォーマットと lint のガイダンスを維持します。

命令ファイルを使って、Copilot がこの方針に沿って作業できるように必要な情報を与えます。

## 命令ファイル

カスタム命令を使うと、コーディング スタイルや要件のコンテキストを Copilot に与え、提案を改善できます。希望する規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルに命令を作成できます。

命令ファイルには、次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の命令ファイルです。多くの Copilot Chat や CLI のリクエストで役立つ、プロジェクト全体に関する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを記載します。

> [!NOTE]
> ほかの命令形式とそのサポート状況は環境によって異なります。形式を利用する前に、[カスタム命令のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには命令ファイルが含まれています。変更の効果を確認する前に、既存のファイルを見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この命令は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle のデータレイヤー、テストなどに関する命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭にある `applyTo` フィールドは、命令を適用するファイルを指定する glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルや既存のプロジェクト ファイルへのリンクを確認します。これにより、大きな命令セットを再利用可能な小さいファイルに分割し、参考例を Copilot に示せます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存のファイルは出発点として適切ですが、改善の余地があります。`copilot-instructions.md` を更新して、新しく生成する TypeScript に TSDoc コメントを追加するようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35行目付近にあります。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。機能のコンテキストを維持したまま新しいガイダンスを読み込むため、命令を保存した後、フィルタリングの会話を終了して再開します。

1. フィルタリングの会話を開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルからコンテキストを取得するしくみを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの命令とパス指定の命令ファイルがエージェントに適用されるしくみを確認しました。
- コーディング標準に従うよう命令ファイルを更新しました。
- 命令ファイルがコードに与える影響を確認しました。

次は[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI のカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [カスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
