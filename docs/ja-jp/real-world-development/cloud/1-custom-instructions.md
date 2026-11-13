---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際にはコンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストにアクセスできるようにします。[指示ファイル][instruction-files]を使うと、実行する内容だけでなく方法も Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスで対象を限定した指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは更新前と更新後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を非同期に処理するため、リアルタイムで並べて効果を示すのが難しいからです。後の演習で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

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

## 新しいリポジトリ標準を追加する

ここで編集する内容は 1 つです。コード内のドキュメントには TSDoc のドキュメントコメントを使い、ファイルレベルのコメントヘッダーを付けるという、プロジェクト全体のルールを追加します。後の演習で issue を割り当てると、cloud agent はこのルールを参照します。

編集する前に、作業用のブランチを作成します。cloud agent は issue の対象ブランチから指示を読み込みますが、変更をコミットする際もブランチを使います。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 指示の変更をコミットしてプッシュする

cloud agent は、issue が対象としているブランチから指示ファイルを読み込みます。次の演習で Copilot に issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、指示の変更を `main` にマージしておく必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開いてマージします。次の演習で issue に割り当てたとき、cloud agent がこの指示を参照できるようになります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作成せず、`main` に直接コミットできます。ブランチを使う手順は、実際のプロジェクトでの進め方を再現するために設けています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りのハーネスでは、cloud agent が生成するプルリクエストをレビューして、その標準が適用されているかを確認します。

この演習では、次のことを行いました。

- リポジトリのカスタム指示とパスで対象を限定した指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加しました。

次は[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
