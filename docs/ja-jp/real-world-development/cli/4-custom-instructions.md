---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際にはコンテキストが重要です。特定の方法でタスクを実行する必要がある場合、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、求めるコードだけでなく、コードの構成方法も説明できます。フィルタリング機能を構築したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスで対象を限定した指示ファイルがエージェントにどう適用されるかを確認します。
- コーディング標準に従うため、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントでは、コードの内容を言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で文書化します。`db` 引数を注入できる場合は、その引数も含めます。
- 再利用可能な Astro コンポーネントでは、`Props` の契約を文書化し、関連コードが変更されたときにコメントを最新の状態に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿って作業できるよう必要な情報を提供します。

## 指示ファイル

カスタム指示を使うと、コンテキストや希望する方法を Copilot に提供できます。これにより、コーディングスタイルや要件を伝えて、より適切な提案やコードスニペットを得やすくなります。リポジトリ全体に適用する指示も、特定の種類のファイルに適用するタスク単位の指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべての依頼で Copilot に送られる単一の指示ファイルです。大半の依頼に関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを提供します。

> [!NOTE]
> 他の指示形式やサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

変更を加える前に、スタータープロジェクトに含まれる指示ファイルを確認し、変更の影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングのガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いてファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。冒頭の `applyTo` フィールドが、適用対象を決める glob を設定していることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存プロジェクトファイルへのリンクを確認します。これにより、大きな指示を小さく再利用可能なファイルに分割し、Copilot が従う例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは良い出発点ですが、追加で必要な指示があります。`copilot-instructions.md` のコアファイルを編集して、新しく生成する TypeScript に TSDoc コメントを追加する標準を定めます。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を適用するため、機能のコンテキストを保ったままフィルタリングの会話を終了して再開します。

1. フィルタリングの会話を開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートから `game-filters-cli` ブランチで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   指示が表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   指示とコードのガイダンスを更新しました。生成したコードをそのガイダンスに合わせて更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しく生成された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルのコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパスで対象を限定した指示ファイルがエージェントにどう適用されるかを確認しました。
- コーディング標準に従うため、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
