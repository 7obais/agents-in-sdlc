---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業をより効果的に行うための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に伝えます。チームの専門知識を再利用可能なエージェントにまとめることができます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティ エージェント、セキュア コーディングのプラクティスに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなどです。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` フォルダーにある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、そのエージェントの動作、専門知識、指示を定義する Markdown プロンプトを記載します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、論理的に重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。最も分かりやすく区別するなら、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには専用のコンテキスト ウィンドウがあり、作業の一環としてスキルやほかのエージェントを調整できます。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに従ってサイトをレビューし、更新します。その作業の一環として、プルリクエスト ワークフローのスキルや、テストを実行して管理するスキルを呼び出すことができます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI のほかの活用方法と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることを重視しています。最近、現在のダーク テーマはテキストと背景色のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティの課題に対処するため、デザイン チームはユーザーが切り替えられる高コントラスト モードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタム エージェントを使って機能を生成しましょう。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、高コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントがすでに作成されています。内容を確認し、Copilot をどのように導くかを把握しましょう。

codespace に戻り、ターミナルを開いてアクセシビリティ作業用に `main` から新しいブランチを作成します。演習 3 のフィルター PR は別にしておきます。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタム エージェントに必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責任。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` に定義されたすべてのカスタム エージェントが Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。カスタム エージェントを選択すると、その指示とツールに合わせてチャット セッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェント ドロップダウンで、**Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します。既定では、現在のエージェントとして **default** が表示されています。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案し、プロジェクト ファイルを編集します。各編集は、ファイル パスとレビュー可能な差分とともにチャットに表示されます。
6. 編集が反映されると、**Files changed** の表示が更新され、このエージェントが変更したファイルが示されます。

> [!NOTE]
> この処理には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなどの既存ファイルを実際に変更します。

次の演習では、進行中の作業をレビューして方向付けします。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に特化した GitHub Copilot の AI アシスタントである[カスタム エージェント][custom-agents]について学びました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot が特定の作業をより効果的に行えるようにできます。

次の概念について学びました。

- カスタム エージェントの定義方法。
- Copilot Chat のエージェント モードでカスタム エージェントを使う方法。

次は[エージェントの作業を監視して方向付け][next-lesson]、変更を確認し、同じセッションにライト モードの追加を依頼します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
