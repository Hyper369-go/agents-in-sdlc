---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot アプリのレッスンに備えて、Tailspin Toys プロジェクト用の Node.js をインストールし、テンプレートからリポジトリのコピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot アプリは、Copilot と GitHub の両方を利用できるデスクトップアプリです。Issue とプルリクエストにすばやくアクセスでき、GitHub Copilot を使った開発にも対応しています。このワークショップでは、コンピューター上で Astro を使った Tailspin Toys アプリを更新し、GitHub Copilot アプリを使います。まず、ローカル環境に Node.js がインストールされていることを確認し、次に Copilot アプリをインストールします。

このレッスンでは、次のことを行います。

- プロジェクトのテストをコンピューター上で実行できるように Node.js をインストールします。
- テンプレートから Tailspin Toys プロジェクトのコピーを作成します。

## Node.js をインストールする

いくつかのレッスンでは、エージェントに機能を実装させ、Tailspin Toys のテストスイートを実行します。プロジェクトが必要とするランタイムは **[Node.js][nodejs]** のみです。現在の **LTS** リリースをインストールしてください。

すべてのプラットフォームで最も簡単なのは、公式インストーラーを使う方法です。

1. オペレーティングシステムで、Windows Terminal、macOS のターミナル、または普段使っているターミナルを開きます。
2. 次のコマンドを実行して、インストール済みの Node.js のバージョンを確認します。

   ```shell
   node --version
   npm --version
   ```

3. 両方のコマンドが成功し、Node.js のバージョンがプロジェクトの README と `package.json` に記載された要件を満たしている場合は、次のセクションに進めます。

> [!TIP]
> これらの手順は、Node.js がインストールされていない場合や、更新が必要な場合にのみ実行してください。

4. [Node.js のダウンロードページ][node-download]を開きます。
5. 使用しているオペレーティングシステム用の **LTS** ビルドをダウンロードします。
6. インストーラーを実行し、既定の設定を選びます。Windows では **Add to PATH** を選択したままにします。
7. インストール後、新しいターミナルウィンドウを開きます。
8. 次のコマンドを実行し、新しいターミナルでインストールを確認します。

   ```bash
   node --version
   npm --version
   ```

9. インストール済みの Node.js と npm のバージョンが表示されます。

> [!IMPORTANT]
> Copilot アプリのエージェントも `node --version` と `npm --version` を実行できる必要があります。普段のターミナルで成功しても、アプリから利用できるとは限りません。Node.js のインストールや PATH の変更後は、アプリを完全に終了して再起動してください。セッションでいずれかのコマンドが失敗する場合は、続行する前に[セッション環境のトラブルシューティング手順][session-environment]を確認してください。

> [!IMPORTANT]
> E2E チェックを実行するには、各ワークツリーにプロジェクトの依存関係と Playwright Chromium も必要です。ワークツリーの準備では Tailspin Toys リポジトリの README に従い、インストールの許可を求められた場合は内容を確認してください。

## ラボ用リポジトリをセットアップする

Tailspin Toys プロジェクトの自分用コピーを使います。[テンプレートリポジトリ][template-repository]から作成してください。新しいリポジトリにはラボに必要なファイルがすべて含まれます。アプリをインストールするときに、このリポジトリを接続します。

1. 新しいブラウザーウィンドウで、このラボの GitHub リポジトリ `https://github.com/github-samples/tailspin-toys` を開きます。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択し、次に **Create a new repository** を選択して、自分用のコピーを作成します。

   ![ドロップダウンから Create a new repository が選択された Use this template ボタン](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

   ![テンプレートに github-samples/tailspin-toys が設定され、リポジトリ名が入力された Create a new repository フォーム](../../../_images/app-0-create-repository.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えてください。この後のラボで参照します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップではこの Issue に取り組みます。自分で Issue を登録する必要はありません。

ワークショップテンプレートの新しいコピーを使ってください。リポジトリの指示、アプリケーションのコード、テスト、`quality-checks` スキル、既存のキャンバス拡張機能が含まれています。ワークショップではスキルをカスタマイズし、QA エージェントを作成します。古いコピーを使う場合は、必要なファイルが含まれていることをファシリテーターに確認してください。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- コンピューター上でプロジェクトのビルドとテストを実行できるように Node.js をインストールしました。
- テンプレートから Tailspin Toys リポジトリのコピーを作成しました。

次は [GitHub Copilot アプリをインストール][next-lesson]し、作成したリポジトリを接続して、ワークスペースの使い方を確認します。

## リソース

- [Node.js をダウンロードする][node-download]
- [テンプレートからリポジトリを作成する][template-repository]
- [GitHub Copilot アプリについて][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
