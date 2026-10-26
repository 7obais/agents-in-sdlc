---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債を抱えていない組織は、ほとんどありません。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなどがその例です。GitHub Copilot の cloud agent は、コードの更新や機能の追加といった作業を自律的に行います。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。これにより、面倒な作業を任せて開発プロセスを速め、開発者はより大きな課題に集中できます。

GitHub Copilot cloud agent を使って、次のことを行います。

- コード生成環境をカスタマイズする。
- 操作が安全に実行されるようにする。
- 明確に範囲を定めた Issue の重要性を確認する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。サイトの最初のバージョンを作成した開発者は、ドキュメントを十分に整備しませんでした。実際、ドキュメントがほとんどありません。まず、アプリケーションのすべてのエクスポートされた関数に TSDoc ドキュメント コメントを追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに関連するゲーム (同じカテゴリのほかのタイトル) を表示し、ユーザーがさらに閲覧できるようにしたいとのことです。洗練された実装はまだ必要ありません。UX の受け入れテストに使えるものを求めています。これは現在ブロッカーですが、優先度がより高い Issue がほかにあります。

どちらも優先順位が下がりやすいタスクであり、Copilot cloud agent に割り当てるのに適しています。Copilot cloud agent は非同期に作業するため、開発者はほかのタスクに集中できます。その後、Copilot の作業をレビューして、期待どおりかを確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。人間の開発者と作業する場合と同様に、[GitHub Issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で作業を依頼できます。割り当てると、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境を設定してタスクに取りかかります。セッションは作業中でも完了後でも確認できます。提案された解決策のレビュー準備ができると、Copilot がプルリクエストで通知します。

## 範囲を明確に定めた命令の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく調べると複雑な場合があります。そのため、Copilot cloud agent に依頼するタスクの範囲を[慎重に定めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階を踏んで学び、試し、必要に応じて調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令と、それが Copilot を導く方法を学びました。カスタム命令を実際に試している場合は、`.github/copilot-instructions.md` の動作を確認したことがあるでしょう。まだの場合は、ここで簡単に確認します。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる命令ファイルを読み取り専用で確認し、後からその効果を見つけられるようにします。

リポジトリの GitHub Web UI、または実行中の codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメント コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターで、命令が `**/*.test.ts` ファイルに適用されることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てると、生成されたプルリクエストに TSDoc ドキュメント コメント、TypeScript 規約、コメント ヘッダーが反映されます。これらは命令ファイルで定義されています。後の演習で PR をレビューするときに、もう一度確認します。

## Copilot cloud agent の開発環境を設定する

誰が作業する場合でも、コードを作成するには通常、適切な環境と、正しく動作するために実行するセットアップ スクリプトが必要です。Copilot にタスクを割り当てる場合も同じです。Copilot はソフトウェア エンジニアのように作業します。

Cloud agent は、作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に環境をカスタマイズできます。必要な開発ツールや依存関係を利用できるようになります。このラボの進行と学習機会のために、あらかじめ構成されています。Node.js、プロジェクトの依存関係、エンドツーエンド テスト用ブラウザー バイナリ、単一の Astro アプリ用に移行およびシードされた SQLite データベースを Copilot が使えるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできるようにします
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要最小限の権限を設定します。Copilot は操作用の専用トークンを受け取ります。
    permissions:
      # セットアップ手順の一部としてリポジトリをクローンする場合 (依存関係のインストールなど)、`contents: read` 権限が必要です。クローンしない場合、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 単一の Astro アプリのビルドやテストで使用するローカル SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフロー ファイルと同じように見えますが、いくつかの重要な点があります。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは、Copilot がプルリクエストで作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーに注目してください。リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot の実行を待たずに、ワークフローが正常に動作するかをテストする際に便利です。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古くなっていたり、ドキュメントが不足していたりするプロジェクトは多くあります。このような技術的負債は、開発の生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成を得意とするため、Copilot cloud agent に依頼するのに適した Issue です。バックグラウンドで必要なドキュメントを作成してもらいます。後の演習で、その作業をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` を設定します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue を Copilot に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。一覧に新しい PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が作業リストを作成します。

> [!NOTE]
> Copilot の作業リストが PR に表示されるまで数分かかる場合があります。Copilot は環境を作成し (先ほどのワークフローを実行)、プロジェクトを分析して、タスクに取り組む方法を決めています。

13. 作業リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するために、ウィンドウの更新が必要な場合があります。

16. ライブ セッションをスクロールし、Copilot が問題を解決する過程を確認します。コードを調査して状況を把握する様子、考えて適切な計画を決める様子、コードを作成する様子などを確認できます。

この処理には数分かかる場合があります。Copilot cloud agent の主な目標の 1 つは、非同期にタスクを実行し、ほかの作業に集中できるようにすることです。この機能を利用して、もう 1 つのタスクを Copilot cloud agent に割り当てた後、アプリケーションへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

前述のように、GitHub Copilot cloud agent の大きな利点の 1 つは作業を分担できることです。デザイン チーム向けに関連ゲームのセクションを追加する作業は時間がかからないかもしれませんが、その時間を別のタスクに使うこともできます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` を設定します。
6. **Description** に次の内容を設定します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue を Copilot に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が Issue に取り組んでいることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組み始めました。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はありません。作業が完了したらレビューできます。ここでは、カスタム エージェントの作成と利用に進みます。

## まとめと次のステップ

このレッスンでは[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当てると、非同期で作業してもらえます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに Copilot を活用できます。

次の概念を学びました。

- コード生成環境をカスタマイズする。
- 操作が安全に実行されるようにする。
- 明確に範囲を定めた Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

Copilot cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と利用に取り組みます。[Copilot cloud agent は MCP サーバーも使用できます][cloud-agent-mcp]。また、前のモジュールで学んだカスタム命令も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
