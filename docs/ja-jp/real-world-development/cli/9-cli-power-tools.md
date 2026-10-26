---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデル選択、セッション共有、クラウド委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様、GitHub Copilot CLI には操作に使えるスラッシュ コマンドが多数あります。これらのコマンドで、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ほかの便利なコマンドも見てみましょう。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使えるコマンドを確認します。

## シナリオ

CLI の基本的なワークフローを終えました。次はコンテキストの管理、モデルの切り替え、セッションの共有といった機能を確認し、必要に応じて [Copilot cloud agent][about-cloud-agent] に作業を委任します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルのコンテキスト ウィンドウの上限に近づくことがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドを使ってコンテキストを確認したり、自分で圧縮したりできます。

1. Codespace に戻り、まだ開いていない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き領域ごとのコンテキストの配分を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、目立った変化がないこともあります。

> [!NOTE]
> コンテキスト ウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。圧縮するタイミングを選びたい場合は `/compact` を使います。関係のない別のタスクを始める場合は、`/clear` または `/new` で会話を新しくします。

## モデルを選ぶ

モデルごとに強みが異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルの一覧表示と選択ができます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用できるモデルと使用状況に関する情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで共同作業し、学んだことを共有すると、AI ツールの活用を広げられます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストールしたバージョンで `/share` が使えることを確認します。
2. セッションを共有する場合は、ローカルの Markdown ファイルにエクスポートする `/share file` など、目的に合う方法を選びます。
3. エクスポートした内容を、送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## オプション: 作業を拡張または委任する

ワークショップの中心的な内容はここまでです。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期で作業してプルリクエストを作成することもある Copilot cloud agent にタスクを送信できます。

これらのコマンドは新しい worktree やリモートの作業を作成する可能性があるため、使用は任意です。試す場合は、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく学ぶには、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定を変更したり、セッションを共有したり、内部の動作を確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使えるコマンドを確認しました。

Copilot CLI にはほかにも多くのスラッシュ コマンドがあります。最後に[学んだことと次のステップを振り返り][next-lesson]、今後の学習に役立つリソースを確認します。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
