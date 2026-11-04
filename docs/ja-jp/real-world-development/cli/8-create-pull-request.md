---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが、1 つのブランチに保存されています。これらをまとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は手動でマージしました。今回は **Agent Merge** にマージの手順を任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを別々の PR に分けます。ここではワークショップを簡潔にするため、フィルター機能と品質ワークフロー全体を 1 つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージまでの作業を自動化する仕組みを学びます。
- 機能全体の変更と検証結果を確認します。
- フィルター機能の PR を作成します。
- レビューの後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能の作業を通じて、Copilot で機能を計画、実装、検証しました。Tailspin Toys は、マージの承認権限を開発者に残しながら、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージ可能な状態にするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題を解決します。たとえば、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースを行います。その後、リポジトリで許可されていれば、GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、変更をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの準備ができたら、まとめてレビューし、PR を作成して、残りの作業を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を実行してエージェントのダイアログを開きます。
3. 一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべてのチェックが成功すると、マージが実行されます。
7. 「PR #14 was squash-merged successfully.」のようなメッセージが Copilot から表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、または不足している権限を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで可能であればレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、いくつかの開発作業を自動化しました。具体的には、次のことを行いました。

- Agent Merge と、マージまでの作業を自動化する仕組みを学びました。
- 機能全体の変更と検証結果を確認しました。
- フィルター機能の PR を作成しました。
- レビューの後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使える[その他の Copilot CLI スラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
