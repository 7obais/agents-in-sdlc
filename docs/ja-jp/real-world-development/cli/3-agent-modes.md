---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で実装方針を決め、Autopilot で Issue に基づくフィルタリング機能を実装し、結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能を追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作するため、適切な機能を適切な方法で構築できます。このレッスンから、Issue を使って機能を追加し、コードの妥当性と動作を確かめ、プロジェクトにマージするまでの、エージェントを活用した開発プロセスを学びます。

> [!NOTE]
> この機能の作業を続ける間は、同じ会話とブランチを使います。通常はファイルの種類ごとに別のブランチや PR を使いますが、ここでは中心的な概念に集中するため、手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の新しい会話を始めます。
- Plan モードで要件を決めます。
- Autopilot モードで新しい機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能開発を続けながら、リポジトリの命令を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログは拡大しており、訪問者がカテゴリと発行者でゲームを絞り込めるようにする必要があります。バックログの Issue には機能が記載されていますが、カテゴリの組み合わせなど、コーディング前に合意が必要な詳細があります。Plan モードで決定してから、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、その重要性は高まります。多くの開発者は、次のような流れで作業します。

1. 実装内容を説明した Issue から始めます。
2. 構築する内容の計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くの場合は上記の流れに沿って作業します。

この標準的な進め方を守ることで、AI が生成したコードが要件を満たし、手書きのコードと同じ検証プロセスを経るようにできます。

## 会話モード

**会話モード**では、エージェントにどの程度の自律性を与えるかを指定します。<kbd>Shift</kbd>+<kbd>Tab</kbd> でモードを切り替えます。

- **Interactive**: エージェントが変更を提案し、次に進む前にあなたの入力を待ちます。
- **Plan**: エージェントが計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントがコードを作成し、テストを実行して、タスクが完了するまで自律的に反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から開始する

フィルタリング機能に取りかかる前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認します。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、実装を依頼する前に機能の実装方法や作業内容を決められます。複雑なタスクでは、事前に時間をかけて計画することをお勧めします。Plan モードに切り替え、Copilot に計画の作成を依頼しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 先ほど追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な方法を判断して計画を作成します。

3. フィルタリング機能の動作について Copilot から質問されたら、好みに応じて回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画をスクロールして確認します。
6. 決定と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画が作成され、レビューも完了したので、実装に進みましょう。Autopilot を使って Copilot に作業を任せます。

Autopilot では、Copilot が完了したと判断するまで、課題に対する作業を反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストールしたバージョンで同様の名前の選択肢を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを作成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まることがあります。先に計画を確認してください。依存関係がない、またはポートが競合していると Copilot が報告した場合は、チェックの完了とみなす前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様、マージ前にレビューする必要があります。コードをレビューし、サイトを起動して問題がないことを確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリング機能とテストを確認します。
3. 結果を Issue と計画時の決定に照らし合わせます。
4. コードを確認したら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードを確認できました。次はアプリを起動して、Codespaces から転送したポートを使ってサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces がポート `4321` を使用可能と通知したら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリ フィルター、発行者フィルター、およびそれらの組み合わせを試します。
4. リセット時と結果がない場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

会話モードを切り替えて、機能を作成しレビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot の新しい会話を始めました。
- Plan モードで要件を決めました。
- Autopilot モードで新しい機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は[カスタム命令を使って][next-lesson]、コード生成が文書化されたプラクティスに従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
