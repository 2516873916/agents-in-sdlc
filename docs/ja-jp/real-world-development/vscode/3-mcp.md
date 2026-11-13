---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode でフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで動作を確認します。アプリを手動で操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を開きます。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot に新しいツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に機能をブラウザーで手動テストするよう依頼します。
- ブランチを公開してフィルタリング作業のプルリクエストを開きます。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部にアクセスできると、さらに強力になります。Copilot が外部のツールやサービスを利用する仕組みが Model Context Protocol (MCP) です。MCP は、エージェントが外部のツールやサービスと通信するための標準です。

![agent mode の内部構造と、コンテキスト、大規模言語モデル (LLM)、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスと通信できます。リソースを使って最新情報にアクセスしたり、ツールを使って開発者に代わって操作を実行したりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、AI エージェントと外部のツール (既存の API や NPM パッケージなどのローカルツール) との通信を管理します。各 MCP サーバーは、AI エージェントが利用できるツールとリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理するアプリケーション プログラミング インターフェイス (API) にアクセスできます。新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などをエージェントが行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。ウェブページへの移動、フォームへの入力、ボタンの選択などをエージェントが行えます。

他にも多くの MCP サーバーがあり、さまざまなツールやリソースにアクセスできます。GitHub は、エコシステム内での発見や貢献を促進するために[MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内の他の依存関係と同様に扱ってください。使用する前に、ソースコードを慎重に確認し、発行元を検証して、セキュリティへの影響を検討してください。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを構成します。

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

この `github` エントリはプロジェクトテンプレートに含まれているため、前の演習で Copilot がバックログの issue を読み取れました。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が制御できるブラウザーを提供します。機能をテストするために使います。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、ファイルが次のようになるようにします。

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

リモートサーバーの `github` とは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグは Playwright に画面のないブラウザーを使うよう指示します。デスクトップ環境がない codespace ではこれが必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストで Playwright をすでに使っているため、通常は Playwright が必要とするブラウザーもインストール済みです。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて必要としたときに VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、構成をレビューし、信頼する選択をしてサーバーを起動します。

手動での起動は任意です。省略した場合は、次の手順で Copilot がツールを初めて必要としたときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。利用者自身がアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、その結果を読み取って要約します。会話を続けながら機能が期待どおりに動作することを確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使って操作します。つまり、エージェントは支援技術と同様に、ラベル付きの構造化要素 (ボタン、リンク、リスト項目など) を使って判断します。そのため、機能の簡単な確認がアクセシビリティの基本チェックにもなります。

MCP サーバーを接続してアプリを起動した状態で、次のプロンプトを Copilot に送信してフィルタリング機能を試します。

1. Copilot に次のプロンプトを送信します。

   ```text
   Playwright MCP サーバーを使って、実行中のアプリ (http://localhost:4321) をブラウザーで開き、新しいゲームフィルタリング機能を確認してください。

   1. ゲームページに移動し、一覧に表示されているゲームの数を確認します。
   2. カテゴリフィルターを適用し、一覧がそのカテゴリのゲームだけに更新されることを確認します。
   3. フィルターを解除してから、パブリッシャーフィルターを適用し、一覧がそのパブリッシャーのゲームだけに更新されることを確認します。
   4. カテゴリとパブリッシャーのフィルターを組み合わせ、両方を反映した結果になることを確認します。

   各手順で確認したことを報告し、期待どおりに動作しない点があれば指摘してください。
   ```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して、結果を報告します。issue の受け入れ基準と照らして概要を読みます。問題がある場合は、プルリクエストを開く前に追加の質問をするか、コードを修正するよう依頼します。

> [!NOTE]
> このテストでは、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要になる場合があります。ブラウザーの不足が報告されたら、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームがレビューできるようプルリクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更をレビューします。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![Copilot が変更したファイルを表示する「Source Control」(ソース管理) パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、プルリクエストの作成方法はいくつかあります。GitHub Copilot を使って作業しているため、PR の作成も依頼しましょう。Copilot は関連 issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントドロップダウンで **Agent** が選択され、Copilot が GitHub ツールを使えることを確認します。
3. 次のプロンプトで Copilot に PR の作成を依頼します。

   ```plaintext
   リポジトリでカテゴリとパブリッシャーによるフィルタリングに関する issue を探してください。現在のブランチのプルリクエストを新しく作成し、正しい issue と関連付けてください。
   ```

4. Copilot が情報を収集して操作を行うために **Continue** を選択します。GitHub ツールを初めて使うときに GitHub への認証を求められた場合は、指示に従って認証します。
5. Copilot が issue を検索し、該当するものを見つけて PR を作成することを確認します。
6. Copilot が生成したリンクを選択してプルリクエストをレビューします。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot に新しいツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブランチを公開する前に、ブラウザーでフィルタリング機能を手動でテストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを開きました。

機能の**テスト**に MCP サーバーを使いましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は[カスタムエージェントを作成して][next-lesson]、アクセシビリティレビューなど、特定のタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
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
