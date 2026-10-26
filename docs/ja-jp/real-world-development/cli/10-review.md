---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、今後の学習リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
next: false
---

GitHub Copilot CLI を使って、Tailspin Toys の一連の開発ワークフローを進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、登録済みのフィルタリング Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認し、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング Issue を起点に、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリング動作を確認しました。
- 品質保証 (QA) 用カスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの根拠を評価しました。
- 機能全体の変更をレビューし、フィルタリングの PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、オプションのクラウド委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

このワークショップには、更新済みの `main` ブランチから作成する2つの PR の節目があります。

1. **星評価**: ゲームカードに既存の `starRating` と、未評価の場合の明示的な表示を追加します。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして QA プロファイルを作成し、関連するテストを追加します。

フィルタリングの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡略化するため、すべての作業を1つの PR にまとめました。

## さまざまな方法での検証

自動テスト、自分でのブラウザー確認、MCP を使った Copilot によるブラウザー確認など、複数の方法でコードをチェックしました。`quality-checks` スキルは、ユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は PR の前に、要件とテスト範囲のレビューにその結果を組み合わせました。

追加のテストで実際の不足を解消しましょう。QA の実行時にテストを追加する必要がない場合もあります。マージを許可する前にコードと根拠を確認し、変更後は影響を受けた検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。会話をまたいで再利用し、チームの必要に合わせて調整してください。命令は標準を定め、スキルは繰り返し実行するタスクをまとめ、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った **モードとモデル** を選びます。実装前に方針を検討するには **Plan**、範囲の限られた変更に取り組むには **Interactive**、範囲を明確にしたタスクを任せるには **Autopilot** を使います。定型的な編集には応答の速いモデルを、複雑な作業には高性能なモデルを選びます。

基盤と同様に、コンテキストも重要です。何を、なぜ、どのように構築するかを明確に伝えると、結果が大きく変わります。

## さらに学ぶ

中心的なワークフローを学びました。Copilot CLI の機能をさらに確認するには、次のコマンドを試してください。

- `/review` でコードレビュー エージェントに変更の分析を依頼します。
- `/rubber-duck` で問題について対話し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して実行します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールを使い続けることが、上達への近道です。本番コード、趣味のコード、作りたいと思いながら何年も手を付けていなかった小さなアプリなどで試してみましょう。学んだことをチームで共有し、ほかの人の経験からも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境については、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]をご覧ください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
