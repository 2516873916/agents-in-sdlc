---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使う際にはコンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストにアクセスできるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。その中でも重要なのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成の指針を Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスで対象を限定した指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を適用した状態で、フィルタリング用の最初のデータスライス (パブリッシャーヘルパー) を生成します。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加します。
- 同じプロンプトを再実行し、生成されたコードが新しい標準に従うことを確認します。
- 指示の更新とフィルタリングのスライスを `main` にコミットし、次の演習で Copilot が最新のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は非決定的に動作します。この演習では、指示の更新後に動作傾向が変わることを確認します。生成結果が文字単位で一致することは目的ではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。具体的には、次のとおりです。

- データレイヤーには、必ずユニットテストが必要です。
- ユーザーインターフェイス (UI) はダークモードで、現代的な見た目にします。
- コード内のドキュメントには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿って作業できるよう必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を理解しやすくするためのコンテキストや希望を Copilot に提供できます。好みのコーディング規則、ライブラリ、追加してほしいコメントの種類などを指定し、より適切な提案やコードスニペットを得られます。リポジトリ全体の指示と、特定のファイル種類に対するタスク単位の指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**依頼で Copilot に送られる単一の指示ファイルです。技術スタック、構築するものの概要、ベストプラクティス、全般的なガイダンスなど、Copilot への大半の依頼に関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に指針を適用します。TypeScript や Astro などの言語、UI コンポーネントの作成やユニットテストの追加といったタスクのガイドラインを設定できます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の標準形式の指示ファイルも[サポートしています][custom-instructions-support]。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトの例から、一般的な方法を確認できます。

- `copilot-instructions.md` には、構築するものの説明、プロジェクト構成、全般的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種類や特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確なガイダンスを提供します。コードをどのように書くべきか、またどのように書くべきでないかを例示します。

指示ファイルの作成方法は 1 つではなく、AI の使い方にも決まった方法はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。各環境で方法は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。このハーネスのレッスンで、該当する方法を説明します。
>
> テンプレートや出発点を探していますか。[awesome-copilot][awesome-copilot] では、指示ファイル、カスタムエージェント、その他のリソースを確認できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みましょう。中核となる `copilot-instructions.md` が 1 つと、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub のウェブ UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指示もあります。これらは、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。冒頭の `applyTo` フィールドが、リポジトリのルートからの相対パスで適用対象を決める glob を設定していることを確認します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これにより、複雑な指示を小さく再利用可能なファイルに分割し、コード生成時に Copilot が従う例を示せます。そこにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc のドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## 変更用のブランチを作成する

ここではベストプラクティスに従い、作業用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 1. 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開きます。
> 2. **New Chat** を選択して新しい会話を始めます。
> 3. モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを適用した状態でプロンプトを送信します。その後、指示を更新して同じプロンプトをもう一度送信し、違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に示します。
3. Chat ビューのエージェントドロップダウンで **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts を作成または更新し、各パブリッシャーの名前と ID を含む一覧を返すデータアクセスヘルパーを追加してください。ファイルの変更を適用してください。
   ```

5. Copilot はプロジェクトを調査してコードを適用します。ヘルパーファイルとテストなど、複数のファイルに変更が及ぶ場合があります。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取る型付き関数であり、型付きのパブリッシャー配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規則によるものです。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。指示を更新した後の一貫性向上がこの演習の目的です。

## 新しいリポジトリ標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案の改善に役立つよう、リポジトリのコーディング標準を追加しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントに関するルールがありません。そのため、生成されたヘルパーにはドキュメントコメントがありませんでした。
3. 既存の標準の直下に、ファイルコメントヘッダーと TSDoc のドキュメントコメントを追加する次の Markdown を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、リポジトリ全体に適用する指示は `.github/copilot-instructions.md` に、特定の言語、ファイルの種類、タスクに適用する指示は `*.instructions.md` に追加します。ドキュメントコメントのルールのようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

更新した指示を使って同じプロンプトを再実行し、生成結果に変化があるかを確認します。

1. Copilot Chat に戻り、**New Chat** を選択して会話のバッファーをクリアします。
2. `src/lib/publishers.ts` を再度選択して、Copilot が対象ファイルに集中するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   src/lib/publishers.ts を作成または更新し、各パブリッシャーの名前と ID を含む一覧を返すデータアクセスヘルパーを追加してください。ファイルの変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習でも利用する実際のコードに適用しました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリ内の他の資産と同様に、ソース管理で管理します。変更をコミットしてブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新とヘルパーの基盤に関して Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加の関連変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けてプルリクエスト (PR) を作成します。手動で行う代わりに、GitHub ツールに接続済みの Copilot に作業を依頼します。issue を見つけて、issue をクローズする PR を作成するようプロンプトを送ります。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは、今回の演習が初めてです。プロジェクトテンプレートでは GitHub ツールがワークスペースに接続済みなので、セットアップは不要です。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められる場合があります。指示に従って認証してください。後の演習で、この接続の仕組み (Model Context Protocol) を学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を見つけ、現在のブランチから PR を作成するよう Copilot に依頼します。PR では、指示の更新と、今後のフィルタリング作業に向けたパブリッシャーヘルパーの基盤の両方を説明します。

   ```plaintext
   指示ファイルを更新する issue を探してください。現在のブランチから新しい PR を作成し、その issue をクローズすることを明記してください。また、今後のフィルタリング作業に向けたパブリッシャーヘルパーの基盤も追加したことを記載してください。
   ```

4. Copilot が issue を探し、PR を作成します。
5. Copilot に代わって GitHub 操作を実行するための **Allow** を求められたら、コマンドを確認して適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```plaintext
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードをプルして最新状態にしてください。
   ```

7. Copilot に代わって GitHub 操作やシェルコマンドを実行するための **Allow** を求められたら、コマンドを確認して適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って、次のことを行いました。

- リポジトリのカスタム指示とパスで対象を限定した指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認しました。
- **既存の**指示を適用してパブリッシャーヘルパーを生成し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が適用されることを確認しました。
- 指示の更新とヘルパーの基盤をコミットして `main` に反映し、次の演習で引き続き利用できるようにしました。

次は[agent mode][next-lesson]を使って、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
