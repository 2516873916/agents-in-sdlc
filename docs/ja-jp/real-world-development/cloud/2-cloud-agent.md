---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフトプルリクエスト (PR) を作成します。これにより、面倒な作業を任せて開発プロセスを加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境のカスタマイズ。
- 操作が安全に行われるようにする方法。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のウェブサイトを作成した業務委託チームは、理想的とはいえない状態でドキュメントを残しました。つまり、ドキュメントがまったくありません。最初の対応として、アプリケーションのエクスポート関数すべてに TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトルなど、関連ゲームを表示してほしいとの要望があります。まだ洗練された実装は必要なく、UX の受け入れテストに使える状態を求めています。この作業は現在ブロッカーですが、優先度の高い他の issue もあります。

こうしたタスクは優先順位が下がりやすく、Copilot cloud agent に割り当てるのに適しています。Copilot cloud agent に非同期で作業させれば、開発者は別のタスクに集中し、後から戻って作業をレビューし、期待どおりの結果か確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。人間の開発者と協力するときと同じように、[GitHub issue を Copilot に割り当てる][assign-issue]方法などがあります。割り当てると、Copilot は進行状況を追跡するドラフトプルリクエストを作成し、環境をセットアップしてタスクに取り組み始めます。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、PR で開発者に通知します。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見単純な操作にも、詳細を見ていくと複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に定めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に進めながら、学習、実験、調整を続けます。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot の動作を導く方法を紹介しました。カスタム指示の演習をすでに行った場合は、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、この機会に簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリにすでに含まれる指示ファイルを読み取り専用で確認し、後でその影響を確認できるようにします。

すでに codespace を実行している場合は、GitHub のウェブ UI または codespace で次のファイルを確認します。

1. `.github/copilot-instructions.md` を開き、**Code standards** セクション、特に TypeScript の慣例と TSDoc のドキュメントコメントに関する要件を確認します。
2. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てる際は、後のレッスンでプルリクエストをレビューし、TSDoc のドキュメントコメント、TypeScript の慣例、コメントヘッダーがあるかを確認します。

## Copilot cloud agent の開発環境をセットアップする

誰が作業する場合でも、コード作成には通常、特定の環境や、状態を整えるためのセットアップスクリプトが必要です。人間のソフトウェアエンジニアと同じように作業を行う Copilot にタスクを割り当てる場合も同様です。

cloud agent は作業時の環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールと依存関係を利用できるよう環境をカスタマイズできます。ラボの進行を簡単にし、この学習機会を提供するため、この設定はあらかじめ用意されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードを適用した SQLite データベースを Copilot が利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # ここに設定する手順に必要な最小限の権限を設定します。Copilot は自身の操作用トークンを受け取ります。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリを複製する場合は、`contents: read` 権限が必要です。ここで複製しない場合は、手順の完了後に Copilot が自動的に複製します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 単一の Astro アプリのビルドとテスト用に、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

他の GitHub ワークフローと同様ですが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストで作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずにワークフローの正常性をテストするのに役立ちます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古いか、ドキュメントがありません。こうした技術的負債は放置されがちで、生産性を低下させ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成に役立つため、cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを生成してもらい、後の演習で作業をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   関数やメソッドには、有用な場合に TSDoc のドキュメントコメントを付けるという要件が組織にあります。しかし、最近の更新ではこの標準が守られていません。動作を明確にするドキュメントコメントが含まれるよう、既存のコードを更新する必要があります。
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当ての詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく生成されたプルリクエスト (PR) を開きます。タイトルは **[WIP]: Code lacks documentation** のようになります。PR が一覧に表示されない場合は、少し待ってからブラウザーウィンドウを更新します。
12. 数分後、Copilot が ToDo リストを作成したことを確認します。

> [!NOTE]
> Copilot の ToDo リストが PR に表示されるまで、数分かかる場合があります。Copilot は、前に説明したワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む最適な方法を決めています。

13. リストをレビューし、実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ウィンドウを更新する必要があります。

16. ライブセッションをスクロールし、Copilot の作業を確認します。コードを調査して状態を把握する様子、計画を考えるために一時停止する様子、コードを作成する様子を確認できます。

この作業には数分かかることがあります。Copilot cloud agent の主な目的は、非同期でタスクを実行して、開発者が別の作業に集中できるようにすることです。この利点を活用するため、もう 1 つのタスクを cloud agent に割り当てた後、アプリケーションに機能を追加する作業を進めます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

前述のとおり、GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業が短時間で終わるとしても、その時間を他のタスクに使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   同じカテゴリのゲームを関連ゲームとして各ゲームの詳細ページに表示し、利用者が他のゲームを見つけられるようにしたいと考えています。デザインチームは UX を検討し、受け入れテストを行いたいと考えています。要件は次のとおりです。

   - `src/lib/` に、指定されたゲームと同じカテゴリの他のゲームを返すデータアクセスヘルパーを追加する。ただし、指定されたゲーム自体は除外する
   - ヘルパーを使って、ゲーム詳細ページに「Related games」セクションを表示する
   - 関連ゲームがない場合に対応する
   - 新しいヘルパーのユニットテストを作成する
   - PR を作成する前に、すべてのテストに合格することを確認する
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業中であることを示す 👀 が issue の最初のコメントに表示されます。

![issue で Copilot が作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同様に作業するため、常に監視する必要はありません。完了後にレビューすればよいでしょう。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に issue を割り当てると、非同期で作業させることができます。技術的負債の解消、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などを依頼できます。

このレッスンでは、次のことを確認しました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確に定めた issue の重要性を確認しました。
- issue を Copilot に割り当てました。

Copilot cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントの作成と使用に取り組みます。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
