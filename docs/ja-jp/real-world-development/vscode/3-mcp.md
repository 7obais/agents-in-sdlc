---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードを使ってフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認しましょう。アプリを手動で操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot に新しいツールを追加するしくみを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にサーバーを使わせ、ブラウザーでフィルター機能を手動テストします。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外部にもアクセスできると、さらに強力になります。Copilot がそれを実現する方法が Model Context Protocol (MCP) です。エージェントが外部のツールやサービスとやり取りするための標準規格です。

![エージェント モードの内部構造と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP により、AI エージェントは外部のツールやサービスとリアルタイムにやり取りできます。そのため、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を実行したりできます。

これらのツールやリソースは、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じて利用します。MCP サーバーは、AI エージェントと外部のツール (既存の API や NPM パッケージなどのローカル ツール) との通信を管理します。それぞれの MCP サーバーは、AI エージェントがアクセスできる異なるツールやリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理するための API を提供します。AI エージェントは、新しいリポジトリの作成、既存のリポジトリの更新、Issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステムの発見と貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかの依存関係と同じように扱ってください。利用する前にソース コードを慎重に確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使用し、機密リソースや操作へのアクセスを許可するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルでは、この VS Code ワークスペースで利用できる MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに構成されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot は Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できます。機能をテストするのに役立ちます。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、画面を表示せずに Playwright を実行します。デスクトップ画面がない codespace では、ヘッドレス モードが必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはすでにエンドツーエンド テストに Playwright を使っているため、必要なブラウザーは通常インストールされています。Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot がサーバーのツールを初めて必要としたときにサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** アクションが表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code がサーバーを信頼するか確認したら、構成を確認して信頼するよう選択し、サーバーを起動します。

手動で起動するのは省略可能です。実行しない場合は、次の手順で Copilot がツールを初めて必要としたときに VS Code がサーバーを起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。アプリを手動で操作して確認する代わりに、エージェントにページを開かせ、移動、フィルターの適用、結果の読み取りを行わせ、その内容を要約してもらえます。会話を離れずに、機能が期待どおり動作するかをすばやく確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使って動作します。つまり、エージェントは支援技術と同じように、ボタン、リンク、リスト項目などの構造化され、ラベル付けされた要素を使って判断します。そのため、簡単な機能テストをアクセシビリティの基本的な確認にも利用できます。

サーバーに接続し、アプリを実行したら、Copilot に次のプロンプトを送ってフィルター機能を試します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。その内容を Issue の受け入れ条件と照らして確認します。問題がある場合は、プルリクエストを作成する前に追加で質問するか、コードの修正を依頼します。

> [!NOTE]
> このテストでは、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動してください。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーのダウンロードが必要な場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームでレビューするためにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更を示す Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。ここでは GitHub Copilot に PR を作成してもらいます。Copilot は関連する Issue を探し、その Issue と関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** が選ばれていることを確認します。これにより、Copilot は GitHub ツールを使えます。
3. 次のように Copilot に依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報収集や操作の実行を許可します。GitHub ツールを初めて使うときに GitHub での認証を求められる場合があります。指示に従って許可してください。
5. Copilot が Issue を検索して適切なものを見つけ、PR を作成します。
6. Copilot が示したリンクを選択してプルリクエストを確認します。ただし、まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot に新しいツールを追加するしくみを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Playwright MCP サーバーを使い、リリース前にブラウザーでフィルター機能を手動テストしました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能を**テスト**しましたが、Copilot を拡張する方法は MCP だけではありません。次は[カスタム エージェント][next-lesson]を作成し、アクセシビリティ レビューなどの専門的なタスクを効率化します。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
