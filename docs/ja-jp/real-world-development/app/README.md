---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot アプリ"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot アプリ](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は、Copilot CLI を基盤とするデスクトップアプリケーションです。エージェント駆動の開発に集中できるワークスペースを提供します。複数のエージェントセッションの並行実行、セッションモードの切り替え、共有キャンバス、GitHub Issue とプルリクエストのネイティブな管理に対応しています。さらに、リベース、レビューコメント、継続的インテグレーション (CI) の修正、マージをプルリクエストに合わせて進める **Agent Merge** も利用できます。

このワークショップでは、Tailspin Toys で一連の作業を進めます。

1. プロジェクトを準備し、アプリをインストールしてリポジトリを接続し、ワークスペースと登録済みのバックログを確認します。
2. 星評価の小さな変更を加え、ブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージします。
3. フィルター機能の Issue から始め、**Plan** モードで内容を定義し、**Autopilot** モードで実装して、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルター機能を試します。
7. 品質保証 (QA) のカスタムエージェントを作成し、要件、テスト範囲、検証結果をレビューします。
8. フィルター機能の変更全体を確認し、2 つ目の PR に Agent Merge を使います。
9. キャンバスの利用状況を確認し、Database Explorer を使ってリポジトリに基づくトリアージキャンバスを作成・テストします。必要な機能を利用できない場合は、ブロッカーを記録し、読み取り専用の方法を使うか、まとめに進みます。

ワークショップに集中できるよう、PR は 2 つ作成します。星評価の変更と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルター機能です。各 PR は更新済みの `main` から開始します。フィルター機能と品質ワークフローでは、同じセッション、ワークツリー、ブランチを使います。最後のキャンバス演習では PR ワークフローを繰り返さず、共有画面の作成とテストに集中できるよう、同じセッションを使います。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトのコピーを作成する |
| [1. Copilot アプリをインストールする][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認する |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR をマージする |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で作成して、Interactive モードでレビューする |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認して更新し、フィルター機能に適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーによる確認 | Customize で MCP を設定し、フィルターの動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門プロファイルを作成して選び、最後の検証結果を集める |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | フィルター、指示、スキル、QA プロファイル、テストをレビューし、2 つ目の PR に Agent Merge を使う |
| [9. キャンバスを確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリに基づくトリアージキャンバスを作成してテストする |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、参考資料を確認する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランの GitHub アカウント
- [ ] **macOS、Linux、Windows** を実行するコンピューター
- [ ] コンピューターに [Git をインストール][install-git]していること

> [!TIP]
> 有料プランを利用していませんか？認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI が含まれているため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot アプリは Codespace ではなく自分のコンピューターで動作します。[前提条件の演習][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトのコピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合、管理者が **GitHub Copilot app** ポリシーを有効にしている必要があります。これは Copilot CLI のポリシーとは別です。

## はじめに

**[前提条件から開始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
