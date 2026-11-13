---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的とする複数のタスクで、GitHub Copilot と作業してきました。コードへのドキュメント追加、デザインチームが改善を検討する関連ゲーム機能の作成、ハイコントラストモードとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスに沿って作業します。そのため、チームのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関して GitHub Copilot が提案した内容を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監視されずにタスクを実行するため、安全性を維持するためにいくつかのセキュリティ制約が設けられています。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセス権はコード用ブランチ**のみ**に限定されます。
- cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- Model Context Protocol (MCP) サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が生成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。MCP サーバーへの呼び出しを含む外部リソースへのアクセスは、Copilot では既定で制限されているため、**想定される動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** の表示があります。作業が完了するまで待ってください。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![変更されたファイルを表示する「Files changed」タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードを確認します。TSDoc のドキュメントコメントやその他のドキュメントが追加されています。正確な変更内容は異なります。

   変更を確認するときは、TSDoc のドキュメントコメント、TypeScript の慣例、コメントヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの一部が承認待ちになっていることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン (Approve and run workflows)](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションでワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業する際、やり取りは一方通行ではありません。チームメンバーと同様に、PR やコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、それに対応する新しいセッションを開始します。結果は非決定的なため、何を依頼するかを厳密には指定できません。次のような変更を Copilot に依頼できます。

- 各コードファイルの先頭に、ファイルの役割を簡単に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例から選ぶか、コードベースに必要なドキュメントについて別の提案を使い、通常のユーザーと同じように **@copilot** をメンションして変更を依頼するコメントを追加します。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始することを確認します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで、引き続き反復的に作業できます。満足できたら、PR をドラフトからレビュー準備完了に変更し、`main` ブランチにマージします。

![PR をレビュー準備完了に変更する操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue を解決するため、Copilot が生成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローの一部が承認待ちになっていることを示す通知を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン (Approve and run workflows)](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェックセクションでワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストすることもできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えます)。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。これまでと同様に、Copilot cloud agent と反復的に作業し、変更を依頼できます。たとえば、表示する関連ゲームの数を調整するよう依頼したり、コメントヘッダーや TSDoc のドキュメントコメントが追加されているかを確認したりできます (この issue はカスタム指示を更新する**前**に割り当てたことを思い出してください)。これまでと同様に、**Conversation** タブでコメントを追加すると、Copilot がコメントを確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントを使って実装したアクセシビリティ機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の mission control で追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** の表示があります。作業が完了するまで待ってください。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - モードを切り替えるユーザーインターフェイス (UI) コンポーネント。
   - 利用者の設定を保存するためのローカルストレージの使用。
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの一部が承認待ちになっていることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン (Approve and run workflows)](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションでワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認します。

カスタムアクセシビリティエージェントが、アクセシビリティのベストプラクティスに従って Copilot の機能実装を導いたことを確認します。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent は、実際のバックログ項目を任せて開発者が別の作業に集中できる場合に力を発揮します。習慣として身につけるために、リポジトリにいくつか issue を作成して Copilot に割り当ててみてください。次のような作業を依頼できます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、後で cloud agent の出力に与える影響を確認できるようにしました。
- **issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを**作成して使用し**、ハイコントラストとライトモードの切り替えを追加しました。
- Copilot エージェントページを mission control として使い、アクセシビリティのセッションを監視して途中で指示しました。
- cloud agent のプルリクエストをレビューして反復的に改善し、変更を依頼するときに `@copilot` をメンションしてワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。引き続き確認する場合は、ここで学んだ内容を他のハーネスで補完できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat の agent mode と MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、agent skills、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のアイデアを試してみてください。

- リファクタリングやテストカバレッジの issue を cloud agent に割り当てる。
- 別の領域向けにカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent ファイアウォールの構成][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
