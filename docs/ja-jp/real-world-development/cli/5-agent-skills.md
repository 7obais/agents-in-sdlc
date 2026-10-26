---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、手動での動作確認や命令ファイルによる標準の適用が必要です。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

こうした作業には、**エージェント スキル**が適しています。スキルを使うと、Copilot にこれらの操作を適切に実行する方法を伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 命令、スクリプト、リソース

エージェント スキルは、再利用可能なタスクの命令、実行可能なスクリプト、補助リソースをまとめ、エージェントが必要に応じて読み込めるようにします。基本的な構成は、スキル名のフォルダーと、その中の `SKILL.md` です。Markdown ファイルには、スキルの名前と説明を定義するフロントマター、概要、呼び出すタイミングのガイダンスを記載します。必要に応じて、スクリプトやほかのリソースを含むサブフォルダーも追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、チームで共有して再利用するプロジェクト内の `.github/skills` フォルダー、またはユーザーのスキル フォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys のチームがユニットテスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの先頭にある `name` と `description` を読みます。説明は、Copilot がスキルを呼び出すタイミングを判断するのに役立ちます。
3. 命令を読み、テストと lint の手順をどのように案内しているかを確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接スキルを呼び出すことも、自然言語で依頼することもできます。スキルに3つのチェックを実行させましょう。

1. Interactive モードのフィルタリング会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行したチェック、その成否、ツールが実際に報告した内容が分かるレポートに改善しましょう。スキルを更新して、そのようなレポートを作成するようにします。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、動作を確認しましょう。Copilot CLI では、会話を再起動せずに編集したスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを読み、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして実行しました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルタリング機能の PR に含めます。次は[Playwright MCP サーバーを使って][next-lesson]、Copilot がサイトを直接操作できるようにします。

## スキルの例

次のコミュニティの例は参考資料であり、追加の課題ではありません。

- [Agent Skills の仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
