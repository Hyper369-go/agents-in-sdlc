---
title: "演習 2 - GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot のクラウドエージェントは、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、開発者がより大きな課題に集中できるようにします。

Copilot クラウドエージェントを使って、次のことを確認します。

- コード生成環境をカスタマイズする。
- 操作が安全に実行されるようにする。
- 範囲を明確にした Issue が重要であることを理解する。
- Copilot に Issue を割り当てる。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のバージョンを作った外部開発者は、ドキュメントを十分に作成しませんでした。まず、アプリケーションのエクスポート関数すべてに TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のゲームを表示し、続けて閲覧できるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度の高い他の Issue があるため後回しになっています。

どちらも、優先度が下がりやすいタスクです。Copilot クラウドエージェントに割り当てて非同期で進めてもらえば、開発者は他の作業に集中できます。後で戻って、Copilot の作業をレビューし、期待どおりか確認できます。

## GitHub Copilot クラウドエージェントとは

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法などがあります。割り当てると Copilot は進捗を追跡するドラフトのプルリクエストを作成し、環境をセットアップしてタスクを開始します。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな操作にも、詳しく見ると複雑な部分があります。[Copilot クラウドエージェントに依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階的に作業し、学びながら試して調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot をガイドする方法を確認しました。カスタム指示の演習をすでに行った場合は、`.github/copilot-instructions.md` が機能することを確認できています。まだの場合は、この機会に内容を簡単に確認しましょう。Copilot クラウドエージェントに作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認しておくと、後でその影響を見つけやすくなります。

GitHub の Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc ドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue をクラウドエージェントに割り当てたら、作成されたプルリクエストで TSDoc ドキュメントコメント、TypeScript の規約、コメントヘッダーを確認します。これらの指示ファイルが反映されています。後の演習で PR をレビューする際に、もう一度説明します。

## Copilot クラウドエージェントの開発環境を設定する

誰が作業する場合でも、コード作成には通常、特定の環境やセットアップスクリプトが必要です。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てる場合も同様です。

クラウドエージェントは作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業を始める前に実行するツールや依存関係を設定できます。ラボの流れをよくし、学習に集中できるよう、環境は事前に設定されています。単一の Astro アプリで使う Node.js、プロジェクトの依存関係、E2E テスト用のブラウザーバイナリ、マイグレーションと初期データ投入を済ませた SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # ここで必要な最小限の権限を設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は `contents: read` 権限が必要です。ここでクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
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

      # ローカルの SQLite データベースをマイグレーションして初期データを投入し、ビルドとテストでデータを使えるようにします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub Actions ワークフローと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot の実行を待たずに、ワークフローが成功するか確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古かったり、まったく存在しなかったりするプロジェクトもあります。こうした技術的負債が放置されると、生産性が低下し、コードベースの保守や新しい開発者の受け入れが難しくなります。Copilot はドキュメント作成も得意なので、クラウドエージェントに依頼するのに適したタスクです。Copilot はバックグラウンドで必要なドキュメントを作成します。後の演習で作業をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後に、Copilot が ToDo リストを作成します。

> [!NOTE]
> Copilot の ToDo リストが PR に表示されるまで数分かかる場合があります。前に説明したワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む方法を判断しています。

13. ToDo リストと、実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するために、ウィンドウの再読み込みが必要な場合があります。

16. ライブセッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調査して状態を把握し、計画を考えるために一時停止し、コードを作成する手順が表示されます。

この作業には数分かかることがあります。Copilot クラウドエージェントの主な目的は、非同期で作業して他のタスクに集中できるようにすることです。この機能を活用し、別のタスクも Copilot クラウドエージェントに割り当ててから、アプリケーションに機能を追加するコードを書きます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot クラウドエージェントの大きな利点は、作業を分けられることです。デザインチームが求めている関連ゲームのセクションは、長時間かからないかもしれませんが、その時間を他の作業に使えます。Copilot クラウドエージェントに割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、Issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことが分かります。

![Copilot が Issue に取り組んでいることを目の絵文字で示す](../../../_images/ex4-issue-eyes-emoji.png)

Copilot が 2 つ目の依頼に取り組んでいます。Copilot クラウドエージェントはソフトウェアエンジニアのように非同期で作業するため、常に監視する必要はありません。完了後にレビューすればかまいません。次はカスタムエージェントを作成して使う方法を確認します。

## まとめと次のステップ

[GitHub Copilot クラウドエージェント][copilot-agents]について学びました。クラウドエージェントに Issue を割り当て、非同期で作業させることができます。技術的負債への対応、新機能の作成、別のフレームワークからのコード移行などに活用できます。

この演習では、次のことを確認しました。

- コード生成環境をカスタマイズする。
- 操作が安全に実行されるようにする。
- 範囲を明確にした Issue が重要であることを理解する。
- Copilot に Issue を割り当てる。

クラウドエージェントがバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot クラウドエージェントは MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示にも対応しています。

## リソース

- [Copilot クラウドエージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

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
