---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、命令、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリング機能、命令の更新、スキルの更新、品質保証 (QA) プロファイル、テストが、1つのブランチに保存されています。まとめてレビューし、プルリクエストを開きましょう。星評価のプルリクエスト (PR) は手動でマージしましたが、今回は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は、機能、命令の更新、スキルの更新、QA エージェントをいくつかの PR に分けます。このワークショップでは手順を簡略化するため、フィルタリングと品質のワークフロー全体を1つの会話とブランチで進め、作業をこの PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を確認します。
- 機能全体の変更と検証の根拠を確認します。
- フィルタリング機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効化し、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能の開発を通じて、Copilot で機能を計画、実装、検証してきました。Tailspin Toys では、マージを許可する権限を開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗への対処、レビュー コメントへの対応、必要に応じたリベースなど、PR を妨げる問題に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその処理を任せることもできます。ただし、有効にする前に、作業をレビューして問題がないことを確認してください。

## Agent Merge で PR を管理する

コードがそろったので、まとめてレビューして PR を作成し、残りの手順を Agent Merge に任せます。

1. Codespace に戻ります。
2. Copilot CLI に `/agent` と入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべて成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合は、レビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の作業を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を確認しました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルタリング機能の PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は[便利な Copilot CLI のスラッシュ コマンド][next-lesson]で、コンテキスト、モデル、共有、オプションのクラウド委任を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
