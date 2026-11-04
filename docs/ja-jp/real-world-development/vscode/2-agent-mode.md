---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は必要なファイルを探して変更し、テストを実行し、問題をデバッグし、変更し忘れたファイルを見つけて、さらに修正を加えます。この作業は際限なく続くことがあります。

こうした作業に役立つのが Copilot エージェントモードです。

Copilot エージェントモードは IDE でより自律的に作業できるように設計されています。開発者のように既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードとクラウドエージェントは異なる用途向けに作られています。エージェントモードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。クラウドエージェントはチームの一員のように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを見つけて、複数のファイルを連携して変更する方法。
- GitHub Copilot エージェントモードで UI とデータレイヤーにわたる新機能を実装する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの数が増えてきたため、カテゴリや発行元で絞り込めるようにします。前の演習では発行元ヘルパーを追加しました。今度は Copilot エージェントモードを使い、残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを開いて現在の機能を確認します。

このサイトは開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示できます。単一の Astro アプリであり、ビルド時にページを静的 HTML としてレンダリングします。ページはフロントマターから Drizzle ORM を介してローカルの SQLite データベースを直接クエリします。独立したバックエンド API やクライアントサイド UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな動作には標準 DOM API を使う小規模でスコープを限定した Astro `<script>` を使います。

### Web サイトを起動する

Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace では次の手順で実行できます。

1. Codespace に戻ります。前の演習の後であれば、`main` ブランチにいるはずです。
2. Codespace で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くかどうか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL のリンクを選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。Codespace にプライベートトンネルが作成され、Web サーバーにアクセスできるようになります。

### Web サイトを確認する

Web サイトが起動したら機能を確認できます。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの情報を掲載した詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を開始します。演習ごとにモードとモデルの選択が異なります。必要な手順ではその都度説明します。

Web サイトの最初の実装は機能しますが、さらに改善したい機能があります。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェント選択メニューから **Agent** を選びます。**Agent** エージェントは、自律的に計画を立て、複数のファイルを変更し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデルの一覧から **Claude Sonnet 4.5** を選びます。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使ったため、手順にも記載しています。提案の一貫性を高め、良い学習体験につなげることを意図しています。ただし、LLM の出力は確率的なので、手順とは異なる提案が表示される場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログの Issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot は、ユーザーに最も役立つと考えた順に Issue を優先順位付けします。

## 指示ファイルを確認する

エージェントにコードを生成してもらう前に、Copilot にコンテキストを伝えるために使う指示ファイルを確認します。Web サイトへの機能追加の方法に関するコンテキストを含む、[ユーザーインターフェイス (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の全般的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - ファイルの種類別の指示へのリンク:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きな指示セットを小さく管理しやすいものに分割して、Copilot に参考例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つの変更が必要です。

- データレイヤー (`src/lib/`) に残りのフィルターロジックを追加または調整します。
- フィルターの動作に関するテストを追加または更新します。
- ゲーム一覧ページを更新し、フィルター UI を追加します。

さらに、コードベースにマージする前に、テストを実行して成功させる必要があります。Copilot エージェントモードにこれらの作業を依頼しましょう。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログ内の関連 Issue に基づいて機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、対象のファイルを見つける様子を確認します。データレイヤーのヘルパー、UI、テストなどを見つけ、ファイルの変更とテストの実行を始めます。

   ![Copilot がプロジェクトファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、コードベースの複雑さによっては数分かかります。作業中に、コードエディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業が完了すると、すべての変更に対して **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot から求められたら **Continue** を選択してテストを実行します。

   ![テストを実行してよいか確認する Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot は、タスクが完了してエラーが見つからなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新機能を試します。
11. すべてが動作し、コードのレビューも終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードで Tailspin Toys の Web サイトに機能を追加する方法を学びました。次のことを行いました。

- GitHub Copilot エージェントモードで UI とデータレイヤーにわたる新機能を実装する方法。
- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを見つけて、複数のファイルを連携して変更する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### ボーナス演習 - ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションの実装を依頼してみましょう。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定にするかを決めます。
- 希望する実装に必要なガイダンスを Copilot に伝えます。
- Copilot に変更やコンテキストを伝えながら、何度かやり取りすることがあります。これは Copilot を使う際の通常の進め方です。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
