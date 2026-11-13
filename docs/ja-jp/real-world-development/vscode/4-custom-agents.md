---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや領域に合わせて、専門的な人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の種類の作業を効果的に行うための指示、ベストプラクティス、コーディングパターン、領域固有の知識を Copilot に提供します。チームの専門知識を再利用可能なエージェントとして形式化できます。たとえば、[ウェブコンテンツ・アクセシビリティ・ガイドライン (WCAG)][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングを徹底するセキュリティエージェント、一貫したテストパターンを維持するテストエージェントを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタムエージェントと agent skills の比較

カスタムエージェントと[agent skills][agent-skills-concept]には一部重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルや他のエージェントを調整するように設計されています。このラボのアクセシビリティカスタムエージェントは、アクセシビリティガイドラインに照らしてサイトをレビューし、更新します。その作業の一環として、プルリクエストワークフローのスキルやテスト実行・管理用のスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI の他の活用方法と同様に、環境やシナリオに合わせてテストと反復を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディングプラットフォームを使えるようにしたいと考えています。最近の利用者フィードバックでは、テキストと背景色のコントラストが不十分なため、現在のダークテーマを読みづらいと感じる人がいることが分かりました。このアクセシビリティ上の課題に対応するため、デザインチームは利用者が切り替えられるハイコントラストモードを求めています。

アクセシビリティは重要なので、この機能をできるだけ早く実装します。カスタムエージェントを使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。内容を確認して、Copilot をどのように導くかを理解しましょう。

1. codespace に戻り、ターミナルを開いて、フィルタリング PR とは分けてアクセシビリティ作業用の新しいブランチを `main` から作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

4. 続くセクションを確認します。アクセシブルなウェブサイトのコードを生成する際の主な責任、アクセシビリティのベストプラクティス、HTML、CSS、JavaScript のコード例、よくある落とし穴や間違いが説明されています。

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` に定義されたすべてのカスタムエージェントが Copilot Chat ビュー下部のエージェントドロップダウンに表示されます。カスタムエージェントを選択すると、そのエージェントの指示とツールに合わせてチャットセッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 1. 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開きます。
> 2. **New Chat** を選択して新しい会話を始めます。
> 3. モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェントドロップダウンで、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェントドロップダウンを選択します。アクティブなエージェントが表示されます。既定では **default** です。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

   ```plaintext
   サイトにハイコントラストモードを追加してください。利用者が設定できるハイコントラストの切り替え機能を用意し、設定をブラウザーのローカルストレージに保存して、ページを再読み込みしても保持されるようにしてください。
   ```

5. Copilot Chat がコードベースを調査し、変更を提案して、プロジェクトファイルに編集を適用します。各編集はファイルパスと差分とともにチャットに表示されます。
6. 編集が適用されると、**Files changed** の表示が更新され、このセッションでエージェントが変更したファイルを確認できます。

> [!NOTE]
> この処理には数分かかる場合があります。Copilot はリポジトリに実際の変更を加えています。作業中に Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習で、進行中の作業をレビューして追加の指示を出します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学び、特定のタスクや領域に合わせた専門的な AI アシスタントを確認しました。カスタムエージェントを使うと、チームの専門知識と標準を再利用可能なエージェントとして形式化し、Copilot がより効果的に作業できるように導けます。

次のことを行いました。

- 既存のアクセシビリティカスタムエージェントを確認しました。
- Copilot Chat の agent mode でアクセシビリティエージェントを使い、ハイコントラストモードを実装しました。

次は[エージェントの作業を監視して導きます][next-lesson]。変更を進行中にレビューし、同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

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
