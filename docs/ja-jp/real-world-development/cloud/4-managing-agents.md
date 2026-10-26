---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの 2 つの演習で、Copilot cloud agent にユーザー体験の改善や機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作するよう設計されていますが、これらのタスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するためのツールがいくつかあります。その 1 つが GitHub.com の[エージェント ページ][agents-page]です。この mission control では、オープンなプルリクエスト (PR) を持つエージェント タスクをすべて確認できます。実行された操作を調べたり、進行中のセッションに指示して作業を導いたりできます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに指示し、機能の追加を依頼します。

## シナリオ

エージェントに高コントラスト モードの作成を依頼した後、チームはライト モードも追加することにしました。サイトのスタイルを変更し、切り替え機能を追加しているため、同時に実装するのが合理的です。高コントラストに加えてライト モードも追加されるよう、エージェントの作業に指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てられたタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインにあるタスクの一覧を確認します。割り当てた次のタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更するための API を生成する。
   - Web サイトに高コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次の作業が含まれる場合があります。
   - リポジトリのコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーを設定する。
   - 割り当てられたタスクを完了するための各種手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実行方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここから PR と変更ファイルを確認し、作業を監視できます。

## cloud agent に指示する

実行中のタスクを確認できたので、エージェントが高コントラスト モードを実装している間に、ライト モードのトグルも含めるよう依頼しましょう。

1. 高コントラスト モードを追加するセッションを選択します。Copilot が付けたタイトルや作業の状態によって、正確なタイトルは異なります。

   ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セッションを数分監視し、セットアップが完了して作業を開始したことを確認します。次のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェント ページで、作業中のセッションへの指示ダイアログを示す画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に追加したことを確認します。

## Copilot に作業を続けてもらう

前回と同じように、Copilot が更新されたタスクに取り組みます。メッセージを送った時点で進めていた手順が完了すると、新しい依頼を作業に組み込みます。

この処理にも数分かかります。ここまでに学んだ内容を振り返ってみましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して導く中心的な場所である Copilot agents ページについて学びました。この mission control で、進行中および完了済みのすべてのタスクを追跡し、実行中の作業をレビューし、作業範囲の調整や追加のガイダンスを行うために進行中のタスクに指示できます。

次の概念について学びました。

- mission control とエージェント ページで cloud agent のタスクを監視する。
- 進行中のセッションに指示し、機能の追加を依頼する。

Copilot がアクセシビリティ機能の作業を完了したら、Copilot が作成した[プルリクエストを反復して改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
