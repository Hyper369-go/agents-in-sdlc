---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "GitHub Copilot アプリで最初のエージェントセッションを開始し、ゲームカードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認して簡単なチャットも試したので、**エージェントセッション**を開始し、プロジェクトに最初の変更を加えましょう。今回は小さな変更です。ゲームのデータには星評価がすでにありますが、ホームページのゲームカードには表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- エージェントセッションを開始し、その構成を学びます。
- エージェントに目的を絞った小さな変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。一方、ホームページのゲームカードにはタイトル、カテゴリ、発行元、説明しか表示されていません。最初の練習として、エージェントに各カードで既存の評価を表示させます。小さく完結した変更なので、最初のセッションにぴったりです。

## セッションの構成

**セッション**は、独立したワークスペースで実行されるエージェントとの会話です。各セッションには専用の **Git ワークツリーとブランチ**が割り当てられます。そのため、あるセッションで機能を追加し、別のセッションでバグを修正しても、変更を競合させずに複数のセッションを同時に実行できます。セッションはサイドバーでリポジトリごとにまとめられています。セッションを選ぶと切り替えられます。

セッションでは、エージェントとの**会話**、ファイルを調査・編集する際の**ツール操作**、差分を含む**変更されたファイル**の一覧を確認できます。

## セッションを開始して変更を依頼する

プロジェクトを確認して機能を実装するため、新しいセッションを開始します。[アプリのセットアップ][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリの新しいセッションを作成し、変更を依頼します。

1. GitHub Copilot アプリに戻るか、アプリを開きます。
2. サイドバー上部の **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプトボックスの下で **new working tree** と **Interactive** モードを選び、次のプロンプトを使って変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot アプリは、新しいワークツリーというプロジェクトの分離されたコピーを作成してから作業を始めます。その後、プロジェクトを調べて、機能追加に必要なファイルを特定し、コードを作成します。Copilot アプリを使って新しい機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージ前に必ずレビューします。Copilot アプリで変更を確認しましょう。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択すると、変更されたファイルとその差分が表示されます。

   ![Create PR の右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot アプリの上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの詳細を表示する中心的なファイルである `GameCard.astro` にコードが追加されていることを確認します。評価があれば表示し、`starRating` が `null` なら `No rating yet` を表示する、次のような小さなブロックになるはずです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> すべての生成 AI ツールと同様に、Copilot の出力は確率的であり、毎回同じとは限りません。コードは上記の例と完全には一致しない場合がありますが、似たものになるはずです。

## 変更を確認する

ブラウザーを開く前に、エージェントが実行した自動チェックの結果を確認します。数値の `starRating` と `null` の場合の代替表示の両方をテストで確認してください。前提条件が不足している場合や、チェックがスキップされた場合は成功とはみなさず、インストールの許可を求められた場合は内容を確認してから許可します。

コードを読むだけで動作すると判断するのではなく、Copilot に Web サイトを開いてもらい、更新後の UI を確認します。アプリを起動して、Copilot アプリ内のブラウザーキャンバスで開きましょう。

> [!TIP]
> キャンバスは、Copilot アプリ内で使えるインタラクティブなウィジェットです。この後のレッスンでカスタムキャンバスを使い、自分でも作成します。ここでは組み込みのブラウザーキャンバスを使います。

1. 次のプロンプトで、アプリの起動とブラウザーキャンバスでのページ表示を Copilot に依頼します。

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. しばらくするとアプリが起動し、Copilot アプリ内にブラウザーが開きます。
3. 評価があるゲームカードに 5 段階評価が表示されていることを確認します。
4. 確認が終わったら、次のプロンプトでこのセッション用に起動した開発サーバーを停止するよう Copilot に依頼します。

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## 最初のプルリクエストを作成してマージする

機能を追加できました。新しいコードを既存のコードベースにマージするため、プルリクエスト (PR) を作成しましょう。

1. 右上にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. リポジトリのポリシーにより、PR の作成前に関連する Issue が必要だと Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビュー用のペインで PR を開きます。ここで必要に応じてレビューできます。
6. 準備ができたら **Ready to merge** を選択します。
7. 表示されたダイアログで **Merge pull request** を選択し、プルリクエストをマージします。

## まとめと次のステップ

GitHub Copilot アプリを使って最初の変更をリリースできました。具体的には、次のことを行いました。

- エージェントセッションを開始し、その構成を学びました。
- エージェントにゲームカードの目的を絞った小さな変更を依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- 最初の PR を開き、チェック結果を確認して、明示的にマージしました。

次は[フィルター機能の Issue から始め、Plan と Autopilot の各モードを使って][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot アプリでエージェントセッションを使う][agent-sessions]
- [GitHub Copilot アプリについて][about-copilot-app]
- [GitHub Copilot アプリで Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-アプリをインストールして設定する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
