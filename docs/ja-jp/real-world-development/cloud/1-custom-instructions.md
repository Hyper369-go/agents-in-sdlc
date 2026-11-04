---
title: "演習 1 - カスタム指示 (クラウドエージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウドエージェント →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、実行する内容だけでなく、その方法も Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを対象にした指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では指示の更新前後に同じプロンプトを実行しません。Copilot クラウドエージェントは GitHub Issue に対して非同期で作業するため、リアルタイムで並べて効果を示すのは困難です。このハーネスの後半で、クラウドエージェントが作成するプルリクエストをレビューし、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインがあります。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードで、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コンテキストや希望する方法を Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、コードに含めるコメントの種類などを指定でき、より関連性の高い提案やコードスニペットを得るのに役立ちます。リポジトリ全体に適用する指示も、特定の種類のファイルやタスクに適用する指示も作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに関する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発しているものの概要、ベストプラクティス、全般的なガイダンスなど、ほとんどのチャットや CLI のリクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に関する指示を記載します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの作成などを対象にできます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、コードを生成するときにリポジトリレベルの指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の標準形式の指示ファイルにも対応しています。詳細は[カスタム指示のサポート状況][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトでは、一般的な管理方法を確認できます。基本的には、次のようにします。

- `copilot-instructions.md` には、開発するもの、プロジェクト構成、全体的なコーディング標準など、プロジェクト全体に関するガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイルの種類や、特定のタスクに対する指示を記載します。
- 自然な言葉で、明確なガイダンスを書きます。コードのよい例と悪い例も示します。

指示ファイルの作り方はひとつではありません。AI の使い方と同様に、プロジェクトに合う方法は試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意しましょう。このプロジェクトのファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用の指示があります。
>
> Copilot に指示ファイルを生成してもらうこともできます。VS Code の **Configure Chat → Generate Agent Instructions** や、Copilot CLI の `/init` など、環境によって方法が異なります。必要な場合は、利用中の環境向けのレッスンで手順を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェントなどが集まった [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認しましょう。基本となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** の指示もあります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob (リポジトリのルートからの相対パス) を指定しています。ここでは、`**/*.test.ts` に一致するファイルなど、TypeScript のテストファイルが対象になります。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクトの既存ファイルへのリンクを確認します。大きな指示セットを小さく再利用可能なファイルに分け、Copilot にコード生成の参考例を示せます (パスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント化はまだ必須ではありません。次の手順で TSDoc のドキュメントコメントとファイルコメントのヘッダーに関するルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は、コード内に TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーを記述するプロジェクト全体のルールを追加することです。後の演習で Issue を割り当てると、クラウドエージェントはこのルールを参照します。

編集の前にブランチを作成します。クラウドエージェントは Issue の対象ブランチにある指示を読みますが、変更を整理してコミットしましょう。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前に説明したように、`.github/copilot-instructions.md` はプロジェクト全体の情報を Copilot に伝えるためのファイルです。コードの提案を改善するために、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメント化に関するルールはありません。そのため、先ほど生成されたヘルパーにはドキュメントコメントがありませんでした。
3. 既存の標準の直後に、ファイルコメントのヘッダーと TSDoc のドキュメントコメントを追加する次の Markdown を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体のガイダンスには `.github/copilot-instructions.md` を使い、特定の言語、ファイルの種類、タスクには `*.instructions.md` を使います。今回追加したドキュメントコメントのルールのように、プロジェクト全体の標準はリポジトリレベルのファイルに記載します。

## 指示の更新をコミット、プッシュ、マージする

クラウドエージェントは、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てると、エージェントは `main` からブランチを作成します。指示を読み込めるように、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で Issue を割り当てると、クラウドエージェントが更新した指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接変更を加えたい場合は、ブランチを作成せずに `main` にコミットしてもかまいません。実際のプロジェクトでの手順に近づけるため、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半では、クラウドエージェントが生成するプルリクエストで標準が適用されていることを確認します。

次は[最初の Issue を Copilot クラウドエージェントに割り当て][next-lesson]ます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のよりよいカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
