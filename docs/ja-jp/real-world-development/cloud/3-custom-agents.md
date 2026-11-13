---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや領域に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の種類の作業を効果的に行うための指示、ベストプラクティス、コーディングパターン、領域固有の知識を Copilot に提供します。チームの専門知識を再利用可能なエージェントとして形式化できます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングを徹底するセキュリティエージェント、一貫したテストパターンを維持するテストエージェントを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタムエージェントと agent skills の比較

カスタムエージェントと[agent skills][agent-skills-concept]には一部重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルや他のエージェントを調整するように設計されています。このラボのアクセシビリティカスタムエージェントは、アクセシビリティガイドラインに照らしてサイトをレビューし、更新します。その作業の一環として、プルリクエストワークフローのスキルやテスト実行・管理用のスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI の他の活用方法と同様に、環境やシナリオに合わせてテストと反復を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントを使って、次のことを確認します。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て方。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディングプラットフォームを使えるようにしたいと考えています。最近の利用者フィードバックでは、テキストと背景色のコントラストが不十分なため、現在のダークテーマを読みづらいと感じる人がいることが分かりました。このアクセシビリティ上の課題に対応するため、デザインチームは利用者が切り替えられるハイコントラストモードを求めています。

アクセシビリティは重要なので、この機能をできるだけ早く実装します。カスタムエージェントを使って機能を生成します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。ファイルを確認して、Copilot をどのように導くかを理解しましょう。

1. codespace に戻り、`.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルなウェブサイトのコードを生成する際の主な責任、アクセシビリティのベストプラクティス、HTML、CSS、JavaScript のコード例、よくある落とし穴や間違いが説明されています。

## issue を作成して割り当てる

Mission control は、環境内のすべてのエージェント作業を管理する場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加の指示を与えて進行中のセッションを導けます。まず、Copilot にハイコントラストモードの作成を依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   サイトにハイコントラストモードが必要です。利用者が設定できるハイコントラストの切り替え機能を追加してください。設定はブラウザーのローカルストレージに保存してください。
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. カスタムエージェント一覧から **Accessibility agent** を選択します。

   ![cloud agent の割り当て画面。カスタムエージェントと「アクセシビリティ」(Accessibility) が強調表示されている](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学び、特定のタスクや領域に合わせた専門的な AI アシスタントを確認しました。カスタムエージェントを使うと、チームの専門知識と標準を再利用可能なエージェントとして形式化し、Copilot がより効果的に作業できるように導けます。

次のことを確認しました。

- カスタムエージェントの定義方法を確認しました。
- カスタムエージェントにタスクを割り当てました。

Copilot がハイコントラストモードを実装する間に、次は[mission control からエージェントのセッションを監視して導きます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使用する準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
