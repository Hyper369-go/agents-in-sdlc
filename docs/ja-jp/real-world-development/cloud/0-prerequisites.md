---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot クラウドエージェントの演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、指示ファイルの編集やクラウドエージェントが作成した内容のレビューに使う [Codespace][codespaces] を起動します。

## ラボ用リポジトリをセットアップする

作業用のリポジトリを作るには、[テンプレート][template-repository]から新しいインスタンスを作成します。新しいインスタンスにはラボに必要なファイルがすべて含まれます。演習中はこのリポジトリを使います。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、自分用のコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えてください。この後のラボで参照します。

## Codespace を作成する

次に、ラボの演習に使う Codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードの作成、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、機能豊富な IDE が用意されています。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手作業でインストールするよりもずっと早く完了します。その間に GitHub Copilot の他の機能を確認してみましょう。

> [!CAUTION]
> 後の演習でこの Codespace に戻ります。今はブラウザーのタブを閉じずに残しておいてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも、スムーズに進めるために必要な前提条件が整った環境を提供します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、表示されたときに **Reopen in Container** を選択してください。Codespace と同じ開発コンテナーが VS Code によって構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成できました。Copilot クラウドエージェントと一緒に作業するときに使う Codespace の作成も開始しました。

## 次のステップ

次は、クラウドエージェントが従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進みましょう。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
