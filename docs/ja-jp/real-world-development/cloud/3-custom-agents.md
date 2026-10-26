---
title: "演習 3 - カスタム エージェント"
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

カスタム エージェントを使って、次のことを学びます。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることを重視しています。最近、現在のダーク テーマはテキストと背景色のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティの課題に対処するため、デザイン チームはユーザーが切り替えられる高コントラスト モードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタム エージェントを使って機能を生成しましょう。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントがすでに作成されています。内容を確認し、Copilot をどのように導くかを把握しましょう。

リポジトリに戻り、アクセシビリティ カスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタム エージェントに必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責任。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、作業中のセッションに指示を追加できます。まず、高コントラスト モードの作成を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` を設定します。
6. **Description** に次の内容を設定します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

   ![アクセシビリティ エージェントを指定した cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクを開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に特化した GitHub Copilot の AI アシスタントである[カスタム エージェント][custom-agents]について学びました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot が特定の作業をより効果的に行えるようにできます。

次の概念について学びました。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方法。

Copilot が高コントラスト モードの実装に取り組んでいる間に、[mission control からエージェント セッションを監視して方向付け][next-lesson]します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
