---
lab:
  title: Teams タブを作成する
  module: Exercise 3
---

# 演習 3:Teams タブを作成する

## シナリオ

IT サポート チームが、サポート チケットを送信するときにユーザーが必要な情報にアクセスできる Teams タブを作成しようとしているとします。 たとえば、チームはチケットの処理とレポートのために、ユーザーのロケール コードを表示する必要があります。 ここでのタスクは、このタブを作成してユーザーのロケール コードを表示することです。 タブには、後で情報がさらに追加されます。

## 演習タスク

あなたのタスクは、Teams のコンテキストを使ってユーザーのロケールを取得して個人用タブにそれを表示する、Teams タブ アプリを作成することです。

この演習を完了するには、次のタスクを終える必要があります。

1. Visual Studio Code 用の Teams ツールキットを使って、タブ アプリを作成します。
1. ユーザーのロケールを取得して表示するようにアプリを更新します。

**推定完了時間:** 10 分

## タスク 1:Visual Studio Code 用の Teams ツールキットを使ってタブ アプリを作成する

1. Visual Studio Code を開きます。
1. サイドバーの **Microsoft Teams** アイコンを選んで、**[Teams ツールキット]** パネルを開きます。
1. **[新しいアプリの作成]** を選んで、**[タブ]** を選びます。
1. 使用できるオプションの一覧から **[Basic Tab] (基本タブ)** を選びます。
1. プログラミング言語として **[TypeScript]** を選びます。
1. **[ワークスペース フォルダー]** で、**[既定のフォルダー]** を選びます。
1. **[アプリケーション名]** に、「**UserInfoApp**」と入力します。

すべてのフォルダーとファイルが正常にスキャフォールディングされると通知が表示され、Visual Studio Code の新しいインスタンスで新しいプロジェクト フォルダーが開かれます。

**[エクスプローラー]** パネルの *src* フォルダーには、アプリのソース コードが含まれています。 *src* フォルダーの外部にあるファイルは、ボットなどのサーバー関連のものです。 ![エクスプローラー内のファイルのスクリーンショット](../../media/explorer-tab-file.png)

## タスク 2:ユーザーのロケールを取得して表示するようにアプリを更新する

これで、目的の機能をタブ アプリに追加できます。

1. `src` > `views` フォルダーに移動して、`hello.html` ファイルを開きます。
1. `<div>` 要素を見つけ、`<div>` タグと `</div>` タグの間に次の要素が含まれるように更新します。

    ```html
        <h1>Hello, User</h1>
      <span>
        <h2>Review your locale code:</h2>
        <p id="locale"></p>
      </span>
    ```

1. `src` > `static` > `scripts` フォルダーに移動して、`teamsapp.js` ファイルを開きます。
1.  ファイルの内容を次のコードに置き換えます。

    ```typescript
        (function () {
          "use strict";
        
          // Call the initialize API first
          microsoftTeams.app.initialize().then(function () {
            microsoftTeams.app.getContext().then(function (context) {
              if (context?.app?.locale) {
                updateLocale(context.app.locale);
              }
              else{
                updateLocale("unknown");
              }
            });
          });
        
          function updateLocale(locale) {
            if(locale){
              document.getElementById("locale").innerHTML = locale;
            }
          }
        })();
    ```

    このコードは、**context** オブジェクトを使ってユーザーのロケールを取得し、ロケール コードを表示するように HTML を更新します。

## 作業を確認

デバッグ モードでアプリを実行して、機能をテストします。

1. Visual Studio Code で **Microsoft Teams** アイコンを選んで、**[Teams ツールキット]** パネルを開きます。

2. 次のいずれかの方法を使ってデバッガーをアタッチし、アプリの実行を開始します。

   - F5 キーを選択します。
   - Visual Studio Code で **[実行とデバッグ]** メニューに移動します。  目的のブラウザー オプションで **[Teams でデバッグ]** を選び、**[デバッグの開始]** ボタンを選びます。
   - Teams ツールキットの **[環境]** セクションで、*local* フォルダーを開いてから、任意のブラウザーを選びます。

3. Visual Studio Code で **[コンソール]** タブに表示できるアクションについていくつかのチェックが実行された後、新しいブラウザー ウィンドウが開きます。 **[UserInfoApplocal]** ダイアログで、**[追加]** ボタンを選択してプレビューのためにアプリを Teams にインストールします。 ![UserInfoApplocal の追加のスクリーンショット](../../media/add-userinfoapplocal.png)

これで、アプリをサイドバーに表示できるようになります。 アプリには、次の 2 つのタブがあらかじめ構成されています。**[個人] タブ**と **[バージョン情報]**。 タブにロケール コードが表示されていることを確認します。![Web での UserInfoApplocal のスクリーンショット](../../media/userinfoapplocal-run.png)
