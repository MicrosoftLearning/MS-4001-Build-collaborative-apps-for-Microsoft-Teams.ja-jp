---
lab:
  title: 基本ボットを構築する
  module: Exercise 6
---

# 演習 6: 基本ボットを構築する

## シナリオ
IT サポート チームによる Teams ボットの構築を支援するよう依頼されました。 このボットには、特定の状態の省略形を取得し、郵便番号に基づいて特定の地域の現在の気象条件をフェッチする機能もあります。

## 演習タスク
この演習では、Teams Toolkit テンプレートを使用して、単純な Teams ボットを作成します。 このボットは、Teams AI ライブラリを利用してユーザーとのメッセージを処理し、アダプティブ カードを使用した対話を含めます。 この演習には、Teams AI ライブラリと LLM の間の対話は含まれないことに注意してください。

この演習を完了するには、以下のタスクを実行する必要があります。

1. Teams Toolkit の基本ボット テンプレートを使用して Teams ボットを作成します。
1. Teams AI ライブラリを統合します。
1. アダプティブ カードを作成します。
1. メッセージを処理します。

**推定所要時間:** 15 分

## タスク 1: Teams Toolkit テンプレートを使用して Teams ボットを作成する

**目標**: Visual Studio Code の Teams Toolkit を使用して、基本的な Teams ボット プロジェクトを初期化します。

以下のようにコマンド ボット テンプレートを使用して、新しいボットを作成します。

1. Visual Studio Code を開きます。
1. サイドバーの **Microsoft Teams** アイコンを選んで、**[Teams ツールキット]** パネルを開きます。
1. **[新しいアプリを作成]** ボタンを選択します。
1. **[新しいプロジェクト]** メニューから、**[ボット]** を選択した後、**[基本ボット]** を選択して簡易ボットを構築します。
1. [プログラミング言語] では、**[TypeScript]** を選択します。
1. **[ワークスペース フォルダー]** では、プロジェクト ファイルを保存するためのコンピューター上のフォルダーを選択するか作成します。
1. **[アプリケーション名]** では、「**TypeAheadBot**」と入力してから、**Enter** キーを押します。 Teams Toolkit によって新しいアプリの準備が行われ、Visual Studio Code でプロジェクト フォルダーが開かれます。
1. Visual Studio Code から、このフォルダー内のファイルの作成者を信頼するかどうかを確認するメッセージが表示される場合があります。 **[はい、作成者を信頼します]** ボタンを選んで、続行します。
1. Visual Studio Code のエクスプローラーを使用してプロジェクトのディレクトリとファイルを確認し、ソース コードを理解します。

## タスク 2: Teams AI ライブラリを統合する

**目標**: Teams AI ライブラリをプロジェクトに追加して、ボットの機能を強化します。

1. Visual Code で、``Ctrl + ` `` キーを押してターミナルを開きます。
1. ターミナルで、次のコマンドを実行して、必要な Teams AI ライブラリと axios パッケージをインストールします。 axios パッケージは、この演習で Web API を呼び出すために使用する Promise ベースの HTTP クライアントです。
   ```shell
   npm install @microsoft/teams-ai axios --save
   ```
1. プロジェクトのルート ディレクトリに `app.ts` ファイルを作成します。 次のコードをファイルに追加して、`app` オブジェクトを定義し、エクスポートします。
   ``` typescript
   // See https://aka.ms/teams-ai-library to learn more about the Teams AI library.
   import { Application } from "@microsoft/teams-ai";
   
   const app = new Application();
   
   export default app;
   ```
1. プロジェクトのルート ディレクトリで `index.ts` ファイルを開きます。 `TeamsBot` への参照を `app` オブジェクトに置き換えます。 `index.ts` の最終的なファイルは、[index.ts](../../../Allfiles/Labs/Guided-Exercise6/index.ts) で参照できます。
   1. *このボットのメイン ダイアログ*を置き換えます。 次のコードを持つセクション:
      
      コード スニペットの挿入
      ``` typescript
      import app from "./app";
      ``` 
      コード スニペットの削除
      ``` typescript
      // This bot's main dialog.
      import { TeamsBot } from "./teamsBot";
      ```
   1. *受信メッセージを処理するボットを作成する*コード スニペットを削除します。
      ``` typescript
      // Create the bot that will handle incoming messages.
      const bot = new TeamsBot();
      ```
   1. *受信要求のリッスン* コード スニペットを変更します。 `app` オブジェクトを使用してメッセージに応答するには。
      ``` typescript
      // Listen for incoming requests.
      server.post("/api/messages", async (req, res) => {
        await adapter.process(req, res, async (context) => {
          await app.run(context); //replace bot with app object
        });
      });
      ```

## タスク 3: アダプティブ カードを作成する

**目標**: ボット内での静的および動的データのやり取りのためのアダプティブ カードを設計して実装します。

1. このプロジェクトのルート ディレクトリに `cards` という名前のフォルダーを作成します。
1. `cards` フォルダーで、`staticSearchCard.ts` という名前のファイルを次の内容で作成します。 `staticSearchCard.ts` の最終的なファイルは、[staticSearchCard.ts](../../../Allfiles/Labs/Guided-Exercise6/staticSearchCard.ts) で参照できます。
   ```typescript
   import { Attachment, CardFactory } from 'botbuilder';

    export function createStaticSearchCard(): Attachment {
        return CardFactory.adaptiveCard({
            $schema: 'http://adaptivecards.io/schemas/adaptive-card.json',
            version: '1.2',
            type: 'AdaptiveCard',
            body: [
                {
                    text: 'Please search for the list of state abbreviations from static list.',
                    wrap: true,
                    type: 'TextBlock'
                },
                {
                    columns: [
                        {
                            width: 'stretch',
                            items: [
                                {
                                    choices:
                                        [
                                            { title: 'Alabama', value: 'AL' },
                                            { title: 'Alaska', value: 'AK' },
                                            { title: 'Arizona', value: 'AZ' },
                                            { title: 'Arkansas', value: 'AR' },
                                            { title: 'California', value: 'CA' },
                                            { title: 'Colorado', value: 'CO' },
                                            { title: 'Connecticut', value: 'CT' },
                                            { title: 'Delaware', value: 'DE' },
                                            { title: 'Florida', value: 'FL' },
                                            { title: 'Georgia', value: 'GA' },
                                            { title: 'Hawaii', value: 'HI' },
                                            { title: 'Idaho', value: 'ID' },
                                            { title: 'Illinois', value: 'IL' },
                                            { title: 'Indiana', value: 'IN' },
                                            { title: 'Iowa', value: 'IA' },
                                            { title: 'Kansas', value: 'KS' },
                                            { title: 'Kentucky', value: 'KY' },
                                            { title: 'Louisiana', value: 'LA' },
                                            { title: 'Maine', value: 'ME' },
                                            { title: 'Maryland', value: 'MD' },
                                            { title: 'Massachusetts', value: 'MA' },
                                            { title: 'Michigan', value: 'MI' },
                                            { title: 'Minnesota', value: 'MN' },
                                            { title: 'Mississippi', value: 'MS' },
                                            { title: 'Missouri', value: 'MO' },
                                            { title: 'Montana', value: 'MT' },
                                            { title: 'Nebraska', value: 'NE' },
                                            { title: 'Nevada', value: 'NV' },
                                            { title: 'New Hampshire', value: 'NH' },
                                            { title: 'New Jersey', value: 'NJ' },
                                            { title: 'New Mexico', value: 'NM' },
                                            { title: 'New York', value: 'NY' },
                                            { title: 'North Carolina', value: 'NC' },
                                            { title: 'North Dakota', value: 'ND' },
                                            { title: 'Ohio', value: 'OH' },
                                            { title: 'Oklahoma', value: 'OK' },
                                            { title: 'Oregon', value: 'OR' },
                                            { title: 'Pennsylvania', value: 'PA' },
                                            { title: 'Rhode Island', value: 'RI' },
                                            { title: 'South Carolina', value: 'SC' },
                                            { title: 'South Dakota', value: 'SD' },
                                            { title: 'Tennessee', value: 'TN' },
                                            { title: 'Texas', value: 'TX' },
                                            { title: 'Utah', value: 'UT' },
                                            { title: 'Vermont', value: 'VT' },
                                            { title: 'Virginia', value: 'VA' },
                                            { title: 'Washington', value: 'WA' },
                                            { title: 'West Virginia', value: 'WV' },
                                            { title: 'Wisconsin', value: 'WI' },
                                            { title: 'Wyoming', value: 'WY' }
                                        ],
                                    style: 'filtered',
                                    placeholder: 'Search for a state abbreviation',
                                    id: 'choiceSelect',
                                    type: 'Input.ChoiceSet'
                                }
                            ],
                            type: 'Column'
                        }
                    ],
                    type: 'ColumnSet'
                }
            ],
            actions: [
                {
                    id: 'staticSubmit',
                    type: 'Action.Submit',
                    title: 'Submit',
                    data: {
                        verb: 'StaticSubmit'
                    }
                }
            ]
        });
    }
   ```

1. `cards` フォルダーで、`dynamicSearchCard.ts` という名前のファイルを次の内容で作成します。 `dynamicSearchCard.ts` の最終的なファイルは、[dynamicSearchCard.ts](../../../Allfiles/Labs/Guided-Exercise6/dynamicSearchCard.ts) で参照できます。
   ```typescript
   import { Attachment, CardFactory } from 'botbuilder';

   export function createDynamicSearchCard(): Attachment {
        return CardFactory.adaptiveCard({
            $schema: 'http://adaptivecards.io/schemas/adaptive-card.json',
            version: '1.6',
            type: 'AdaptiveCard',
            body: [
                {
                    text: 'Please search for temperature using ZIP Code.',
                    wrap: true,
                    type: 'TextBlock'
                },
                {
                    columns: [
                        {
                            width: 'stretch',
                            items: [
                                {
                                    choices: [],
                                    'choices.data': {
                                        type: 'Data.Query',
                                        dataset: 'locations',
                                        value: 'value'
                                    },
                                    id: 'choiceSelect',
                                    type: 'Input.ChoiceSet',
                                    placeholder: 'ZIP Code',
                                    label: 'ZIP Code search',
                                    isRequired: false,
                                    errorMessage: 'There was an error test.',
                                    isMultiSelect: false,
                                    style: 'filtered'                                
                                }
                            ],
                            type: 'Column'
                        }
                    ],
                    type: 'ColumnSet'
                }
            ],
            actions: [
                {
                    id: 'submitdynamic',
                    type: 'Action.Submit',
                    title: 'Submit',
                    data: {
                        verb: 'DynamicSubmit'
                    }
                }
            ]
        });
    }

   ```

## タスク 4: メッセージを処理する

**目標**: アダプティブ カードを介してユーザーによる入力に応答し、やり取りするボットの機能を開発します。

この手順では、`app` オブジェクトにメッセージ応答機能を追加します。

1. `app.ts` ファイルを開きます。
1. アプリ オブジェクト `const app = new Application();` を作成するコードの下に、さまざまなメッセージを処理する次のコードを追加します。
    ``` typescript
    interface SubmitData {
        choiceSelect?: string;
    }

    app.message(/static/i, async (context, _state) => {
        const attachment = createStaticSearchCard();
        await context.sendActivity({ attachments: [attachment] });
    });

    app.message(/dynamic/i, async (context, _state) => {
        const attachment = createDynamicSearchCard();
        await context.sendActivity({ attachments: [attachment] });
    });

    // Listen for query from dynamic search card
    app.adaptiveCards.search('locations', async (context: TurnContext, state: TurnState, query) => {
        // Format search results
        const locations: AdaptiveCardSearchResult[] = [];
        // Execute query
        const searchQuery = query.parameters['queryText'] ?? '';
        if (searchQuery.length < 4) {
            return locations;
        }   

        const response = await axios.get(
            `https://zip-api.eu/api/v1/codes/postal_code=US-${searchQuery}*`
        );

        interface DataObject {
            state: string;
            place_name: string;
            postal_code: string;
            lat:string
            lng:string
        };

        //response data is an array of objects or an object
        response.data = Array.isArray(response.data) ? response.data : [response.data];
        response.data.forEach((obj: DataObject) => {
            const result: AdaptiveCardSearchResult = {
                title: `${obj.postal_code} - ${obj.place_name}, ${obj.state}`,
                value: `${obj.postal_code}|${obj.place_name}|${obj.lat}|${obj.lng}`
            };
            locations.push(result);
        });

        // Return search results
        return locations;
    });

    // Listen for submit buttons
    app.adaptiveCards.actionSubmit('DynamicSubmit', async (context, _state, data: SubmitData) => {
        let [postalCode, placeName, lat, lon] = data.choiceSelect?.split('|') ?? [];
        await context.sendActivity(`The dynamically selected place is: ${placeName}`);
        const weatherLocation = await axios.get(
            `https://api.weather.gov/points/${lat},${lon}`
        );
        const forcast = await axios.get(weatherLocation.data.properties.forecast);
        await context.sendActivity(`The weather in ${placeName}: ${forcast.data.properties.periods[0].detailedForecast}`);
        });

        app.adaptiveCards.actionSubmit('StaticSubmit', async (context, _state, data: SubmitData) => {
        await context.sendActivity(`The statically selected option is: ${data.choiceSelect}`);
        });

        // Listen for ANY message to be received. MUST BE AFTER ANY OTHER HANDLERS
        app.activity(ActivityTypes.Message, async (context, _state) => {
        await context.sendActivity(`Try saying "static search" or "dynamic search".`);
    });
    ```

1. ファイル `app.ts` ファイルの先頭で、関連する参照を次のように更新します。 `app.ts` の最終的なファイルは、[app.ts](../../../Allfiles/Labs/Guided-Exercise6/app.ts) で参照できます。
    
    Updated
    ```` typescript
    import { ActivityTypes, TurnContext } from "botbuilder";
    import { createStaticSearchCard } from "./cards/staticSearchCard";
    import { createDynamicSearchCard } from "./cards/dynamicSearchCard";
    import axios from "axios";
    // See https://aka.ms/teams-ai-library to learn more about the Teams AI library.
    import { Application, TurnState, AdaptiveCardSearchResult } from "@microsoft/teams-ai";
    ````
    前へ
    ```` typescript
    // See https://aka.ms/teams-ai-library to learn more about the Teams AI library.
    import { Application } from "@microsoft/teams-ai";
    ````    

## 作業を確認

以下のようにアプリをローカルで実行して機能をテストします。

1. **[TEAMS TOOLKIT]** パネルを開きます。 **[開発]** メニューで、**[Teams アプリのプレビュー]** を選択し (または `F5` キーを使用し)、好みのブラウザーで **[Teams () でデバッグ]** を選択します。  
1. Teams Toolkit によって、アプリのプロビジョニングが行われ、ブラウザー内でローカルにアプリが実行されます。
1. ブラウザーの [アプリのインストール] ダイアログで、**[追加]** を選択して Teams アプリをインストールします。  Teams によって、インストールされたボットとの会話が開かれます。
1. メッセージ ダイアログ ボックスで、`static search` と入力し、Enter キーを押します。 ボットはアダプティブ カードを返します。
1. 入力ボックスで、状態名を選択または入力し、**[送信]** ボタンを選択します。 ボットはその状態名の省略形を返します。 ![アダプティブ カードの静的検索のスクリーンショット](../../media/static-search.png)
1. メッセージ ダイアログ ボックスで、`dynamic search` と入力し、Enter キーを押します。
1. 入力ダイアログ ボックスで、米国の郵便番号を入力し、**[送信]** ボタンを選択します。 ボットはその地域の現在の天気を返します。 ![アダプティブ カードの動的検索のスクリーンショット](../../media/dynamic-search.png)