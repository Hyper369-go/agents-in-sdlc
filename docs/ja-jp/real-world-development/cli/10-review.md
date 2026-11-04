---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズについて振り返り、参考資料を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys で一連の作業を進めながら、GitHub Copilot CLI を活用しました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを調べ、あらかじめ登録されたフィルター機能の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードで内容を定義し、Autopilot モードで実装して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を試しました。
- 要件、テスト範囲、スキルの結果、ブラウザーでの根拠を評価する QA カスタムエージェントを作成して選択しました。
- フィルター機能の変更全体をレビューし、フィルター機能の PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## リリースしたもの

このワークショップには、それぞれ更新済みの `main` から作成したブランチ上で進める、2 つの PR の節目があります。

1. **星評価**: 既存の `starRating` と未評価であることをゲームカードに表示します。
2. **フィルター機能と品質ワークフロー**: フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズして QA プロファイルを作成し、関連するテストを追加します。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、その作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作など、いくつかの方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA はこれらの結果に加えて要件とテスト範囲を確認してから、PR を作成しました。

追加するテストは、実際に存在する不足を補うものである必要があります。QA を実行してテストの追加が不要と判断される場合もあります。マージを許可する前に、コードと根拠をレビューしてください。変更後は、影響を受ける根拠を更新します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル** を選びます。作成前に進め方を考えるには **Plan**、範囲を絞った変更を見ながら進めるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集にはより高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

コンテキストも、インフラストラクチャと同じくらい重要です。何を、なぜ、どのように作りたいのかを明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローを終えました。Copilot CLI には、他にも確認しておきたい機能があります。

- `/review` で、コードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で、問題について対話し、別の視点を得ます。
- `/fleet` で、独立したサブタスクを並行して調整します。
- `/worktree` で、別のタスクを分離します。
- `/delegate` で、Copilot クラウドエージェントにタスクを委任します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。実際のコードにも、趣味のプロジェクトにも、ずっと作りたかった小さなアプリにも使ってみましょう。チームで学んだことを共有し、互いの経験から学んでください。そして、ドキュメントも引き続き確認しましょう。

ターミナルから Tailspin Toys を拡張したい場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境については、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または [Copilot クラウドエージェントのワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
