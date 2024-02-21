---
lab:
  title: ボットを構築する
  module: Exercise 4
---

# 演習 4:ボットを構築する

## シナリオ

あなたがサポートしている IT サポート チームは、組織全体の従業員から、一般的で何度も繰り返される大量の問い合わせを受け取っていると想像してください。 これらの問い合わせには、パスワードのリセット、ソフトウェアのインストール手順、一般的なエラーのトラブルシューティングなどの簡単な問題が含まれています。

プロセスを合理化し、チームの負担を減らすために、あなたは Microsoft Teams 内でこれらの一般的な問い合わせを処理できるボットを作成することを決断します。

あなたは、ボットに対して "resetPassword" という名前の最初のコマンドを追加することを決断します。 ユーザーがこのコマンドを入力すると、ボットはパスワードをリセットする方法に関するステップバイステップの手順を返します。 これにより、ユーザーは IT サポート チームに直接連絡しなくても問題を解決できるようになり、チームは空いた時間でより複雑な問題に対処することができます。

あなたは、"resetPassword" コマンドに加えて、他の一般的な問い合わせを処理するコマンドをさらに追加し、ボットを組織の従業員向けの包括的なセルフサービス ツールにすることを計画します。

## 演習タスク

この演習を完了するには、以下のタスクを実行する必要があります。

1. Teams Toolkit を使用してボットを作成する
2. マニフェストを構成する
3. アダプティブ カードを作成する
4. コマンドを処理する

**推定完了時間:** 17 分

## タスク 1:Teams Toolkit を使用してボットを作成する

以下のようにコマンド ボット テンプレートを使用して、新しいボットを作成します。

1. Visual Studio Code で、サイド バーの **Teams Toolkit** に移動します。
2. Teams Toolkit の **[開発]** メニューで、**[新しいアプリの作成]** を選択します。
3. **[新しいプロジェクト]** メニューから、**[ボット]** を選択した後、**[チャット コマンド]** を選択してコマンド ボットを構築します。
4. [プログラミング言語] では、**[TypeScript]** を選択します。
5. **[ワークスペース フォルダー]** では、プロジェクト ファイルを保存するためのコンピューター上のフォルダーを選択するか作成します。
6. **[アプリケーション名]** では、「**SupportCommandBot**」と入力してから **Enter** キーを押します。  Teams Toolkit によって、ボット プロジェクトが作成され、準備が完了します。
7. Visual Studio Code のエクスプローラーを使用してプロジェクトのディレクトリとファイルを確認し、ソース コードを理解します。

## タスク 2:マニフェストを構成する

以下のようにアプリ マニフェストで `ResetPassword` コマンドを定義します。

1. `appPackage` フォルダーのファイル `manifest.json` を開きます。
2. `bots` オブジェクトで、`commandLists` を見つけます。  現状では、`helloWorld` というタイトルのコマンドが 1 つ存在します。
3. `commands` を次のコードに置き換え、以下のように新しい `ResetPassword` コマンドが含まれるようにします。

    ```typescript
            "commands": [
                {
                    "title": "helloWorld",
                    "description": "A helloworld command to send a welcome message"
                },
                {
                    "title": "resetPassword",
                    "description": "Request instructions to reset your password"
                }
            ]
    ```

## タスク 3:アダプティブ カードを作成する

以下のように `ResetPassword` コマンドへの応答として送信されるアダプティブ カードを作成します。

1. `src`/`adaptiveCards` に、`resetPassword.json` という名前の新しいファイルを作成します。
2. 次のコンテンツをファイルに追加して保存します。

   ```json
        {
            "type": "AdaptiveCard",
            "body": [
                {
                    "type": "TextBlock",
                    "size": "Medium",
                    "weight": "Bolder",
                    "text": "Reset Password Instructions"
                },
                {
                    "type": "TextBlock",
                    "text": "1. Navigate to the login page and select 'Forgot Password'.",
                    "wrap": true
                },
                {
                    "type": "TextBlock",
                    "text": "2. Enter your email or username and select 'Submit'.",
                    "wrap": true
                },
                {
                    "type": "TextBlock",
                    "text": "3. Check your email for a password reset link and select it.",
                    "wrap": true
                },
                {
                    "type": "TextBlock",
                    "text": "4. Enter and confirm your new password, then select 'Save'.",
                    "wrap": true
                },
                {
                    "type": "TextBlock",
                    "text": "5. Log in with your new password.",
                    "wrap": true
                }
            ],
            "actions": [
                {
                    "type": "Action.OpenUrl",
                    "title": "Go to Login Page",
                    "url": "https://www.adaptivecards.io/designer/"
                }
            ],
            "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
            "version": "1.4"
        }
   ```

## タスク 4:コマンドを処理する

次に、ボットのソース コードで `TeamsFxBotCommandHandler` クラスを使用してコマンドを処理します。  json ファイルから `resetPasswordCard` をインポートし、コマンドが呼び出されたときにこれをレンダリングします。

1. `src` フォルダーに、`resetPasswordCommandHandler.ts` という名前の新しいファイルを作成します。
2. 作成した生の `resetPassword` カードをインポートするためのステートメントを含め、次の import ステートメントをファイルに追加します。

   ```typescript
   import { Activity, CardFactory, MessageFactory, TurnContext } from "botbuilder";
    import { CommandMessage, TeamsFxBotCommandHandler, TriggerPatterns } from "@microsoft/teamsfx";
    import { AdaptiveCards } from "@microsoft/adaptivecards-tools";
    import rawResetPasswordCard from "./adaptiveCards/resetPassword.json";
   ```

3. import ステートメントの下に、コマンド ハンドラーを実装するための次のコードを追加した後、ファイルを保存します。

   ```typescript
       export class ResetPasswordCommandHandler implements TeamsFxBotCommandHandler {
          triggerPatterns: TriggerPatterns = "resetPassword";
        
          async handleCommandReceived(
            context: TurnContext,
            message: CommandMessage
          ): Promise<string | Partial<Activity> | void> {
            console.log(`App received message: ${message.text}`);
        
            const resetPasswordCard = AdaptiveCards.declareWithoutData(rawResetPasswordCard).render();
            await context.sendActivity({ attachments: [CardFactory.adaptiveCard(resetPasswordCard)] });
          }
        }
   ```

## タスク 5:新しいコマンドを登録する

新しい各コマンドは `ConversationBot` で構成する必要があります。これにより、コマンド ボット テンプレートの会話フローが強化されます。

1. `src/internal/initialize.ts` ファイルに移動します。
2. 次の import ステートメントを 2 行目に追加します。

    `import { ResetPasswordCommandHandler } from "../resetPasswordCommandHandler";`
3. 20 行目で、`command` プロパティの `commands` 配列を更新して、新しいハンドラーを初期化するステートメントを含めます。`new ResetPasswordCommandHandler().  The updated ` コマンド オブジェクトは次のようになります。

   ```json
   command: {    enabled: true,    commands: [new HelloWorldCommandHandler(), new ResetPasswordCommandHandler()],  },
    ```

## 作業の確認

以下のようにアプリをローカルで実行して機能をテストします。

1. **[ライフサイクル]** メニューで、**[Teams アプリのプレビュー]** を選択し (または `F5` キーを使用し)、好みのブラウザーで **[Teams () でデバッグ]** を選択します。  
2. Teams Toolkit によって、アプリのプロビジョニングが行われ、ブラウザー内でローカルにアプリが実行されます。
3. ブラウザーの [アプリのインストール] ダイアログで、**[追加]** を選択して Teams アプリをインストールします。  Teams によって、インストールされたボットとの会話が開かれます。
4. コマンド `resetPassword` を入力するか選択します。
5. ボットがパスワード リセット手順を含むアダプティブ カードで応答することを確認します。
