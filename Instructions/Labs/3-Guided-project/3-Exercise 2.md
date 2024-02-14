---
lab:
  title: 着信 Webhook を作成する
  module: Exercise 2
---

# 演習 2:着信 Webhook を作成する

## シナリオ

IT サポート チームはサード パーティの通知サービスを使ってアラートとメッセージを管理しているとします。 最近、チームは、重要な更新プログラムに使われている Teams チャネルにメッセージを投稿するプロセスを自動化することにしました。  サード パーティのサービスは、Webhook 経由でメッセージを投稿するように設計されています。  

## 演習タスク

ここでのタスクは、これらのメッセージを受信するために、**Alerts** という名前の新しい着信 Webhook を作成することです。  また、Webhook をテストし、`"Testing the Alerts endpoint."` という文字列を含むメッセージを正しく受け入れて表示できることを、確認する必要があります。 タスクを完了すると、チームは Webhook エンドポイント URL を使ってサービスを更新するようになります。

この演習を完了するには、次のタスクを実行する必要があります。

1. 着信 Webhook を登録します。
2. メッセージを投稿して Webhook をテストします。

**推定完了時間:** 8 分

## タスク 1:着信 Webhook を登録する

最初に、着信 Webhook を登録します。

**注:**  この演習に使っている Teams アカウントに、チャネルを持つチームがまだない場合は、次の手順を行う前に新しいチャネルを作成してください。

1. Microsoft Teams で、Webhook を構成できるチャネルに移動します。
2. そのチャネルで、**[その他のオプション]** メニューを選んでから、**[コネクタ]** を選びます。  (注: チャネル一覧のメニューではなく、チャネル内のメニューを使ってください。)
3. `"webhook"` を検索して、"**Incoming Webhook**" (着信 Webhook) を選びます。

   :::image type="content" source="../../media/add-incoming-webhook.png" alt-text="検索バーの Webhook のスクリーンショット。":::

4. **[追加]** を選択します。
5. 概要ページで **[追加]** を選びます。
6. そのチャネルで、**[その他のオプション]** メニューをもう一度選んでから、**[コネクタ]** を選びます。
7. **[Incoming Webhook] (着信 Webhook)** の横の **[構成]** を選びます。
8. 名前に「**Alerts**」と入力します。
9. **［作成］** を選択します
10. 次のタスクで使うため、**[URL]** をコピーしておきます。
11. **完了** を選択します。

これで、チャネルに着信 Webhook が構成されました。

## タスク 2:メッセージを投稿して Webhook をテストする

Webhook をテストするには、PowerShell を使って Webhook エンドポイントにメッセージを送信します。

1. **PowerShell** を開きます。
2. 次のコマンドを実行してメッセージを送信します。

     ```powershell
     Invoke-RestMethod -Method post -ContentType 'Application/Json' -Body '{"text":"Testing the Alerts endpoint."}' -Uri <YOUR WEBHOOK URL>
    ```

## 作業を確認

1. Microsoft Teams クライアントで、構成されたチャネルの **[会話]** タブに移動します。
2. `"Testing the Alerts endpoint"` という `Alerts` からのメッセージが、チャネルに存在することを確認します。

 :::image type="content" source="../../media/final-alert-message.png" alt-text="Azure portal の [構成されたアクセス許可] ビューのスクリーンショット。":::
