---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は通常、必要なファイルを探して変更し、テストを実行し、問題をデバッグし、変更漏れがないか調べ、さらに修正します。この作業は続いていきます。

ここで役立つのが Copilot エージェント モードです。

Copilot エージェント モードは IDE 内でより自律的に作業します。開発者のように、まず既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。エージェント モードを使ってサイトに新機能を追加する方法を見ていきましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる体験を提供します。エージェント モードは IDE 内でタスクを実行し、迅速なフィードバックと対話を可能にします。cloud agent はチーム メンバーのように非同期で作業し、Issue やプルリクエストを通じてやり取りするペア プログラマーとして設計されています。

この演習では、次の方法を学びます。

- Copilot エージェント モードでプロジェクトを調べ、関連ファイルを特定し、複数の変更を連携して行う方法。
- GitHub Copilot エージェント モードで UI とデータ レイヤー全体に新機能を実装する方法。
- Copilot エージェント モードが生成した変更やテストを、コードベースにマージする前にレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリやパブリッシャーで絞り込めるようにします。前の演習で publishers ヘルパーを追加しました。ここでは Copilot エージェント モードを使い、残りのデータ レイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを見て、現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ユーザーはゲームの一覧や詳細を表示できます。単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページは Drizzle ORM を使ってフロントマターからローカル SQLite データベースに直接クエリを実行します。個別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能は標準の DOM API を使った範囲限定の Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習を終えていれば、`main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` をブラウザー ウィンドウで開くかどうかを尋ねるダイアログが表示されたら、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して Web サイトを開きます。

> [!NOTE]
> codespace を使っている場合、Codespace ターミナルから localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは Web サーバーを実行している codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホーム ページ**: ボード ゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は機能していますが、新しい機能を追加して改善したいと考えています。まず、バックログを確認しましょう。テンプレートからプロジェクトを設定すると、Issue のバックログが自動的に作成されています。GitHub Copilot にこれらの項目を表示してもらいます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** は自律的に変更を計画してファイルを更新し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。このワークショップの作成時には Claude Sonnet 4.5 を使ったため、体験を安定させる目的で手順に記載しています。ただし、LLM の出力は確率的なため、ワークショップに記載されたものと異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> LLM の性質上、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. Copilot に次のプロンプトを送信して、Issue のバックログを尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーにとって有用だと考える順に、項目を優先順位付けしていることも確認できます。

## 命令ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを与える命令ファイルを確認しましょう。Web サイトに機能を追加する方法のコンテキストを含む [ユーザー インターフェイス (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) ファイルを使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全般的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイルの種類別の命令へのリンク。

> [!TIP]
> 命令ファイルでは、ほかの命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルからの相対パスです。これにより、再利用や、大きな命令セットを管理しやすい小さな単位に分割すること、Copilot に例やテンプレートを示すことができます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つの更新が必要です。

- データ レイヤー (`src/lib/`) に残りのフィルター ロジックを追加または調整する。
- フィルターの動作を検証するテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードをマージする前にテストを実行して合格する必要があります。Copilot エージェント モードを使ってこれらのタスクを実行しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、命令ファイルを追加します。

   ![UI 命令ファイルを追加する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選ばれていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選ばれていることを確認します。
5. バックログにある関連 Issue の要件に基づいて機能を実装するよう、Copilot に次のプロンプトを送ります。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを探します。データ レイヤーのヘルパー、UI、テストを見つけた後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など、いくつかのタスクを行います。タスクとコードベースの複雑さによっては数分かかる場合があります。作業中、コード エディターに **Keep** と **Undo** のボタンが表示されることがあります。Copilot が作業を完了すると、すべての変更に **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストを繰り返し、エラーが見つからなくなるまで作業します。

   ![Copilot エージェント モードの Chat セッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や AI ツールが生成したコードをレビューすることは常に重要です。

10. Web サイトを開いているブラウザーに戻り、新しい機能を確認します。
11. すべてが動作することを確認し、コードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェント モードを使って Tailspin Toys の Web サイトに新しい機能を追加しました。次の方法を学びました。

- GitHub Copilot エージェント モードで UI とデータ レイヤーにまたがる新機能を実装する。
- Copilot エージェント モードでプロジェクトを調べ、関連ファイルを特定し、複数の変更を連携して行う。
- Copilot エージェント モードが生成した変更やテストを、コードベースにマージする前にレビューする。

次は、[Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成しましょう。

### ボーナス演習 — ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだスキルを使い、Copilot にページネーションを実装するよう依頼してみましょう。次の点を検討してください。

- 既存の命令ファイルを使うなど、既存のベスト プラクティスに従います。
- ユーザーがページ サイズを選べるようにするか、固定値にするかなど、ページネーションの実装方法を検討します。
- 希望どおりの実装を得るために、必要なガイダンスを含めてプロンプトを作成します。
- GitHub Copilot に変更を依頼してコンテキストを伝えながら、何度か調整する場合があります。これは通常の進め方です。

## リソース

- [Copilot の Ask、Edit、Agent モード — 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
