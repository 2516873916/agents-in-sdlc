---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーからフィルタリング機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が利用できる追加の自動化機能を用意する必要もあります。そこで MCP サーバーが役立ちます。MCP サーバーを使うと、Copilot は CLI に組み込まれた機能以外のツールやサービスも利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを理解します。
- まだ使用できない場合は、Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を調べます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、人工知能 (AI) エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムに通信できます。最新情報にアクセスしたり、開発者に代わって操作を実行したりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。各 MCP サーバーは、AI エージェントが利用できるツールとリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理するアプリケーション プログラミング インターフェイス (API) にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザーの自動化機能を提供します。

他にも多くの MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内の他の依存関係と同様に扱ってください。使用する前に、ソースコードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じようにウェブサイトを操作できるようにします。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP を構成済みの場合は、このセットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd>、次に <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright で機能を調べるよう Copilot に依頼する

これまでは機能が期待どおりに動作することを手動で確認しました。今度は Copilot で使用できる Playwright MCP サーバーに同じ確認を行わせます。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に指示します。

   ```plaintext
   アプリを起動し、Playwright MCP を使って issue と計画に照らしてフィルタリングを確認してください。変更は加えず、動作することと動作しないことを報告してください。完了したら、起動したサーバーを停止してください。
   ```

> [!NOTE]
> 通常は、Copilot が自動的に判断するため、MCP サーバーを使うよう明示する必要はありません。ただし、使用するツールが分かっている場合は、使うよう伝えても問題ありません。結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで操作を行い、機能を確認する手順を表示する様子を確認します。
3. レポートを読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright でウェブサイトを操作して、サーバーを停止し、結果を報告します。

## まとめと次のステップ

おめでとうございます。Playwright MCP サーバーを使い、Copilot CLI から実際のブラウザーで機能を調べました。内容を振り返ると、次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを学びました。
- まだ使用できない場合は、Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を調べました。

次は[QA カスタムエージェントを作成し][next-lesson]、スキルとブラウザーツールを専門的な役割にまとめます。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
