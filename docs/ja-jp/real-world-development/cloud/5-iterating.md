---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードにドキュメントを追加し、デザインチームが試せる関連ゲーム機能を作り、ハイコントラストとライトモードの切り替えなどのアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝えて改善してもらいましょう。

### シナリオ

何度も説明したように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードを必ずレビューし、通常の DevOps プロセスに沿って作業します。チームメンバーがレビューできるように設定する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認しましょう。

## セキュリティと GitHub Copilot クラウドエージェント

Copilot クラウドエージェントは、監視なしで非同期に作業します。そのため、安全性を保つためにいくつかのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みができるのは作業に使うブランチ**のみ**です。
- クラウドエージェントは GitHub Actions 内で動作し、作業用の一時的な独立環境を作成します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- 既定では [MCP サーバーを含む外部リソースへのアクセスが制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

GitHub Copilot クラウドエージェントが最初に作成したプルリクエスト (PR) を開き、コードへのドキュメント追加を確認しましょう。標準的な GitHub.com の PR インターフェイスを使います。

> [!NOTE]
> PR を開くと、GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。これは想定された動作です。既定では、Copilot は外部 MCP サーバーへの接続など、外部リソースへのアクセスが制限されています。必要であれば、[Copilot クラウドエージェントのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。表示されている場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら **Files changed** タブを選び、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のドキュメントコメントなど、更新されたコードを確認します。実際の変更は環境によって異なります。

   変更を確認するときは、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションにワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot とやり取りするのは、一方通行ではありません。チームメンバーと同じように、PR のコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確率的なので、依頼文をひとつに決めることはできません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、内容を簡単に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** をメンションし、生成されたドキュメントへの変更を依頼するコメントを追加します。上記の例や、コードベースに追加したい他のドキュメントを依頼できます。
2. **View Session** を選択して、Copilot が作業する様子を確認します。Copilot が変更用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選び、変更をレビューします。

問題がなくなるまで、Copilot に変更を依頼できます。満足できたら、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェックセクションにワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (Copilot が作成したブランチ名に置き換えてください。例: **copilot/fix-8**)。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。クラウドエージェントに変更を依頼して、作業を反復して改善できます。たとえば、表示する関連ゲームの数の変更や、コメントヘッダーおよび TSDoc のドキュメントコメントの追加を依頼できます (この Issue はカスタム指示を更新する**前**に割り当てました)。前の PR と同様に、**Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。表示されている場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認します。特に次の点を確認してください。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保存するためのローカルストレージの使用。
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションにワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (Copilot が作成したブランチ名に置き換えてください)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えを試します。ページを再読み込みしても設定が維持されることを確認してください。

アクセシビリティ用カスタムエージェントにより、Copilot はアクセシビリティのベストプラクティスに従って機能を実装できました。アクセシビリティ上の問題や改善点がある場合は、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 - 作業の委任を続ける

実際のバックログのタスクをクラウドエージェントに割り当て、その間に別の作業を進めると効果的です。習慣づけるために、リポジトリに Issue を追加して Copilot に割り当ててみましょう。次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

クラウドエージェントのハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、後でクラウドエージェントの出力に反映される様子を把握しました。
- **Issue を Copilot クラウドエージェントに割り当て**、環境のセットアップ、計画、非同期の作業を確認しました。
- **アクセシビリティ用のカスタムエージェントを作成して使い**、ハイコントラストとライトモードの切り替えを追加しました。
- **Copilot エージェントページをミッションコントロールとして使い**、進行中のアクセシビリティセッションを監視して方向を調整しました。
- **クラウドエージェントのプルリクエストをレビューして改善し**、**@copilot** に変更を依頼したり、ワークフローを承認したりしました。

## レビューと次のステップ

クラウドエージェントのハーネスを完了しました。引き続き学びたい場合は、他のハーネスで今回の内容をさらに実践できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードや MCP 連携を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、任意の `/delegate` によるクラウドエージェントへの連携を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでも、次のような作業を試してみましょう。

- リファクタリングやテスト範囲の拡充に関する Issue をクラウドエージェントに割り当てます。
- 別の分野用にカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot クラウドエージェントのファイアウォールを設定する][agent-firewall]

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
