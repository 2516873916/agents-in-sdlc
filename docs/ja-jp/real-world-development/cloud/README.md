---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
description: "GitHub issue を cloud agent に割り当て、作業を導き、進捗を監視して、作成されたプルリクエストをレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調査し、変更を行い、プルリクエスト (PR) を開きます。その間、開発者は別の作業を続けられます。

各演習では、まず cloud agent が従うカスタム指示を追加し、その後 GitHub issue を割り当てて実装を任せます。カスタムエージェントを使って方針を定め、エージェントのダッシュボードからセッションを監視して指示し、最後にプルリクエストをレビューして結果を反復的に改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従うカスタム指示を追加する |
| [2. Cloud Agent][ex2] | 非同期エージェント | issue を Copilot cloud agent に割り当てる |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して使う |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して指示する |
| [5. 反復作業][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して次のステップを選ぶ |

## 前提条件

このワークショップに参加する前に、次の条件を満たしていることを確認してください。

- [ ] アクティブな **Copilot Student、Pro、Pro+、Business、Enterprise** プランを持つ GitHub アカウント
- [ ] アカウントまたは組織で Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランを利用していませんか。認証済みの学生は、[GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free を除くすべてのプランが対象です。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは Model Context Protocol (MCP) を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## はじめに

**[演習 0: 前提条件から開始する →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
