---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加に関するいくつかのタスクに GitHub Copilot と取り組みました。コードへのドキュメント追加、デザイン チームが改善を重ねるための関連ゲーム機能の作成、高コントラストとライト モードの切り替えなどのアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善しましょう。

### シナリオ

何度も述べたように、生成 AI を加えてもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業する必要があります。チームのほかのメンバーにレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は監視なしで非同期にタスクを実行するため、安全性を保つためのセキュリティ制約が設けられています。

- Copilot がリポジトリに対して持つ権限は読み取り専用で、書き込みができるのはコードのために使うブランチ**のみ**です。
- Cloud agent は GitHub Actions 内で実行され、作業用に独立した一時的な環境を作成します。
- GitHub Actions のワークフローを実行するには、人間の承認が必要です。
- 既定では、MCP サーバーなどの[外部リソースへのアクセスが制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールによってブロックされているという警告が表示される場合があります。既定で MCP サーバーへのアクセスなどの外部リソースへのアクセスが制限されているため、これは**想定された動作**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が表示されます。その場合は、作業が完了するまで待ちます。数分かかることがあるので、休憩したり、ここまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed** タブを選択して変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 更新されたコードを確認します。追加された TSDoc ドキュメント コメントや、その他のドキュメントなどが含まれます。実際の変更内容は異なる場合があります。

   変更を確認するときは、TSDoc ドキュメント コメント、TypeScript の規約、コメント ヘッダーに注目します。これらは演習 2 の冒頭で確認したカスタム命令ファイルに由来します。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を確認します。
8. 承認待ちのワークフローがある場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック セクションにワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックに合格します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との作業は一方通行ではありません。チームのメンバーと同様に、プルリクエストのコメントやコードへのインライン コメントで Copilot にタグ付けできます。Copilot はコメントを受け取り、それに対応する新しいセッションを開始します。出力は確定的ではないため、決まったプロンプトは示せません。たとえば、次のような更新を依頼できます。

- それぞれのコード ファイルの先頭に、ファイルの役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc ドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** を一般のユーザーと同じようにタグ付けし、生成されたドキュメントへの変更をコメントで依頼します。上記の案や、コードベースに追加したいドキュメントに関する別の提案を使います。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

   ![プルリクエストに戻る画面](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更を確認します。

満足できるまで、引き続き改善を依頼できます。問題がなければ、PR をドラフトからレビュー可能な状態に変更し、`main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するために Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. 承認待ちのワークフローがあることを示す表示を確認します。
7. 承認待ちのワークフローがある場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック セクションにワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックに合格します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えます)。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。先ほどと同様に、Copilot cloud agent に更新を依頼して反復的に作業できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーや TSDoc ドキュメント コメントを追加したりできます (この作業はカスタム命令を更新する**前**に割り当てたことを思い出してください)。これまでと同様に、**Conversation** タブで新しいコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能をレビューします。この PR には演習 3 で依頼した高コントラスト モードと、演習 4 の mission control で追加を依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が表示されます。その場合は、作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替えるトグル UI コンポーネント。
   - ユーザー設定を永続化するローカル ストレージの使用。
   - 高コントラスト モードとライト モードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード ナビゲーションなど)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を確認します。
8. 承認待ちのワークフローがある場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック セクションにワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックに合格します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーで高コントラストとライト モードのトグルをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに沿ってこれらの機能を実装するよう Copilot を導いたことを確認できます。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — さらにタスクを委任する

バックログの実際の項目を cloud agent に任せ、その間にほかの作業に取り組むと効果的です。習慣にするために、リポジトリでさらにいくつかの Issue を作成し、Copilot に割り当ててみましょう。たとえば、次のような項目があります。

- ゲーム詳細ページに支援に関心のある人向けのフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム命令ファイルを確認し**、cloud agent の出力に反映されることを確認しました。
- **Issue を Copilot cloud agent に割り当て**、環境の設定、計画、非同期の作業を確認しました。
- **アクセシビリティ用のカスタム エージェントを作成して使い**、高コントラストとライト モードのトグルを追加しました。
- **Copilot agents ページを mission control として使い**、アクセシビリティ セッションを監視して途中で指示しました。
- **cloud agent のプルリクエストをレビューして改善し**、`@copilot` に変更を依頼してワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。ほかのハーネスでは、ここで学んだ内容に関連するシナリオを別の環境で試せます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から直接 Copilot Chat のエージェント モードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、省略可能な cloud agent への `/delegate` ブリッジを使い、ターミナルからレビュー済みの変更を 2 件作成します。

自分のリポジトリで、次のアイデアを試してみましょう。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- ほかの分野向けのカスタム エージェントを作成する。
- 別の技術スタック用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
