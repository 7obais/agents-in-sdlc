---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを扱ったり、Copilot に自動化を追加したりする必要があります。そこで MCP サーバーを使います。MCP サーバーを使うと、CLI に組み込まれた機能に加えて、Copilot がさらに多くのツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を確認します。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させて、フィルタリング機能を確認します。

## シナリオ

ユニットテストとエンドツーエンド テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じようにサイトを使えるようにして、変更の作成をさらに自動化し、期待どおりに動作する確度を高めましょう。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP により、AI エージェントはリアルタイムで最新情報にアクセスし、ユーザーの代わりに操作を実行できます。

こうしたツールやリソースには、AI エージェントと外部サービスをつなぐ MCP サーバーを通じてアクセスします。各 MCP サーバーは、AI エージェントが利用できるツールやリソースのセットを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけたり貢献したりするための [MCP レジストリ][mcp-registry]を提供しています。

> [!CAUTION]
> MCP サーバーは、ほかのプロジェクト依存関係と同様に扱ってください。使用前にソースコードを確認し、発行者を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう追加します。

> [!NOTE]
> Playwright MCP が Copilot CLI ですでに設定されている場合は、セットアップを省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類に **STDIO** を指定し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のコマンドを貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って Copilot に機能を確認させる

これまでは、機能が期待どおりに動作することを手動で確認してきました。次は Copilot で利用できる Playwright MCP サーバーを使って、同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、MCP サーバーの使用を Copilot に指示しなくても、適切なサーバーを判断できます。ただし、使用するサーバーが分かっている場合は、明示するとより安定した結果につながり、トークンも節約できます。

2. Copilot がブラウザーで実行する手順を確認し、機能が動作するかを検証する様子を見ます。
3. Copilot の報告を読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、サーバーを停止してから結果を報告します。

## まとめと次のステップ

Copilot CLI から実際のブラウザーを使い、Playwright MCP サーバーで機能を確認しました。ここでは、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を確認しました。
- Playwright MCP サーバーが未設定の場合に追加しました。
- エージェントにブラウザーを操作させて、フィルタリング機能を調べました。

次は[QA カスタム エージェントを作成し][next-lesson]、スキルとブラウザー ツールを組み合わせた専門的な役割を設定します。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
