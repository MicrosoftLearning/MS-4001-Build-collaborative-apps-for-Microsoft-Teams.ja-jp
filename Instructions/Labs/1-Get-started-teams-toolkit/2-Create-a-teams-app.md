# 演習 2:ギャラリー内のサンプルから Teams アプリを作成する

Teams Toolkit for Visual Studio Code には、ベース アプリの確認と作成にすぐに使用できるサンプルのコレクションが用意されています。 この演習では、サンプル ギャラリーから初めての Microsoft Teams アプリを作成します。

## タスク 1:サンプルからアプリを作成する

1. Teams Toolkit for Visual Studio Code で、**[サンプルを表示する]** を選んで、サンプル アプリのカタログを表示します。
   :::image type="content" source="../../media/view-samples.png" alt-text="サンプルを表示するオプションのスクリーンショット。":::
2. 目的のサンプルを見つけて、スクリーンショットを選び、そのサンプルのページを開きます。  すぐに実行するには、サンプルの詳細ページのサンプル タイトルの下に "Ready for debug (デバッグの準備完了)" と表示されているサンプルを選びます。  (その他のサンプルには、"Manual configurations required (手動構成が必要)" と表示されます)。
3. サンプル ページで **[作成]** を選び、プロジェクトを保存するフォルダーを選びます。 プロジェクトは、このローカル フォルダーにスキャフォールディングされます。
    :::image type="content" source="../../media/create-sample.png" alt-text="サンプルからのアプリの作成を示すスクリーンショット。":::
4. スキャフォールディングが完了すると、Hello World Bot プロジェクトが読み込まれた新しい Visual Studio Code ウィンドウが表示されます。  プロジェクトがスキャフォールディングされた後、Visual Studio Code から、このフォルダー内のファイルの作成者を信頼するかどうかを確認するメッセージが表示されることがあります。 **[はい、作成者を信頼します]** ボタンを選んで、続行します。
    :::image type="content" source="../../media/trust-authors.png" alt-text="作成者の信頼ダイアログのスクリーンショット。":::
5. これで、以下を含むプロジェクト コードを表示できるようになりました。

- Teams アプリ コード。
- appPackage フォルダー内のデプロイおよびマニフェスト ファイル。
- env フォルダー内の環境変数。
- アプリの実行、デバッグ、デプロイに必要な手順が記載された README ファイル。

    :::image type="content" source="../../media/bot-project-code.png" alt-text="スキャフォールディングされたアプリのスクリーンショット。":::