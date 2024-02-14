# 演習 3:Teams タブ アプリを発行する

この演習では、アプリを組織ストアに発行する方法について説明します。

## タスク 1:アプリをストアに発行する

1. Visual Studio Code のアクティビティ バーで、**Microsoft Teams アイコン**を選び、**Teams ツールキット パネル**を開きます。

    a. :::image type="content" source="../../media/publish-to-teams.png" alt-text="Teams ツールキット パネルが開き、[Teams への発行] オプションが強調表示されているスクリーンショット。":::

1. Teams ツールキット パネルの **[ライフサイクル]** で **[発行]** を選びます。

1. ダイアログには、アプリが Microsoft Teams 管理ポータルに正常に発行されたことが表示されます。

1. ダイアログで **[管理ポータルに移動]** を選び、**Microsoft Teams 管理センター**を開きます。

    a. :::image type="content" source="../../media/published-successfully.png" alt-text="アプリが組織ストアに発行されたときのトースト メッセージのスクリーンショット。":::

    a. :::image type="content" source="../../media/admin-portal.png" alt-text="Teams 管理センターのスクリーンショット。":::

1. Teams 管理センターの **[名前で検索]** ボックスに「**hello-tab**」と入力してアプリの一覧をフィルター処理します。 次に、**アプリを選んで**アプリの詳細を表示します。

    :::image type="content" source="../../media/search-app-dev-portal.png" alt-text="Teams 管理センターのアプリの検索を示すスクリーンショット。":::

1. **hello-tab** アプリの詳細パネルで、**[発行]** を選びます。

    :::image type="content" source="../../media/admin-publish-app.png" alt-text="Teams 管理センターでアプリを発行するスクリーンショット。":::

1. **[カスタム アプリを発行しますか?]** ダイアログで、**[発行]** を選びます。

1. 緑色のバナーは、hello-tab アプリが発行されたことを示します。

    :::image type="content" source="../../media/publish-status.png" alt-text="Teams 管理センターにある発行済みアプリの緑色のバナーを示すスクリーンショット。":::

アプリが組織ストアで発行されたら、Microsoft Teams を開き、組織ストアからアプリをインストールします。

## タスク 2:ストアからアプリをインストールする

1. Microsoft Teams で **[アプリ]** に移動し、組織ストアを表示します。 **[組織用に構築]** で、**[hello-tab]** タイルを選びます。

    a. :::image type="content" source="../../media/org-store.png" alt-text="hello-tab アプリが強調表示されている組織ストアのスクリーンショット。":::

1. アプリのインストール ダイアログで、**[追加]** を選びます。

    a. :::image type="content" source="../../media/add-app.png" alt-text="Microsoft Teams でのアプリの追加を示すスクリーンショット。":::

1. アプリが開き、"**お使いのアプリは Azure 環境で実行されています**" というメッセージが表示されます。

    :::image type="content" source="../../media/app-running-in-azure.png" alt-text="Microsoft Teams で実行されているアプリのスクリーンショット。":::