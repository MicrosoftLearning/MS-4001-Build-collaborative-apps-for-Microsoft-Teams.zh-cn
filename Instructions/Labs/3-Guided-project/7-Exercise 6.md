---
lab:
  title: 生成基本机器人
  module: Exercise 6
---

# 练习 6：生成基本机器人

## 场景
假设你被要求帮助 IT 支持团队生成 Teams 机器人。 此机器人将能够检索给定州的缩写，并基于其邮政编码获取特定区域的当前天气状况。

## 练习任务
在本练习中，你将使用 Teams 工具包模板创建简单的 Teams 机器人。 此机器人将使用 Teams AI 库处理与用户的消息，包括使用自适应卡片进行交互。 请注意，本练习不涉及 Teams AI 库和 LLM 之间的交互。

你需要完成以下任务才能完成练习：

1. 使用 Teams 工具包基本机器人模板创建 Teams 机器人。
1. 集成 Teams AI 库。
1. 创建自适应卡片。
1. 处理消息。

**预计完成时间：** 15 分钟

## 任务 1：使用 Teams 工具包模板创建 Teams 机器人

**目标**：使用 Visual Studio Code 中的 Teams 工具包初始化基本的 Teams 机器人项目。

使用命令机器人模板创建一个新机器人：

1. 打开 Visual Studio Code。
1. 在边栏上，选择“**Microsoft Teams**”图标以打开“**TEAMS 工具包**”面板。
1. 选择“**创建新应用**”按钮。
1. 从“**新建项目**”菜单中选择“**机器人**”，然后选择“**基本机器人**”，生成一个简单的机器人。
1. 对于“编程语言”，选择“TypeScript”。****
1. 对于“工作区文件夹”，选择或创建一个文件夹，用于在计算机上存储你的项目文件。****
1. 对于“**应用程序名称**”，请输入 **TypeAheadBot**，然后按 **Enter**。 Teams 工具包将为新应用搭建基架，并在 Visual Studio Code 中打开项目文件夹。
1. 你可能会从 Visual Studio Code 收到一条消息，询问你是否信任此文件夹中的文件创建者。 选择“**是，我信任作者**”按钮以继续。
1. 使用 Visual Studio Code 中的资源管理器查看项目目录和文件，以熟悉源代码。

## 任务 2：集成 Teams AI 库

**目标**：将 Teams AI 库添加到项目，以增强机器人的功能。

1. 在 Visual Code 中，按 ``Ctrl + ` `` 打开终端。
1. 在“终端”中，运行以下命令安装所需 Teams AI 库和 axios 包。 axios 包是一个基于承诺的 HTTP 客户端，我们将在本练习中使用该客户端来调用 Web API。
   ```shell
   npm install @microsoft/teams-ai axios --save
   ```
1. 在项目的根目录中创建一个 `app.ts` 文件。 在文件中添加以下代码，以定义和导出 `app` 对象。
   ``` typescript
   // See https://aka.ms/teams-ai-library to learn more about the Teams AI library.
   import { Application } from "@microsoft/teams-ai";
   
   const app = new Application();
   
   export default app;
   ```
1. 打开项目根目录中的 `index.ts` 文件。 用 `app` 对象替换对 `TeamsBot` 的引用。 `index.ts` 的最终文件可在 [index.ts](../../../Allfiles/Labs/Guided-Exercise6/index.ts) 中引用。
   1. 将“*此机器人的主对话。*” 部分替换为以下代码：
      
      插入代码片段
      ``` typescript
      import app from "./app";
      ``` 
      移除代码片段
      ``` typescript
      // This bot's main dialog.
      import { TeamsBot } from "./teamsBot";
      ```
   1. 移除代码片段“*创建将处理传入消息的机器人。*”
      ``` typescript
      // Create the bot that will handle incoming messages.
      const bot = new TeamsBot();
      ```
   1. 修改代码片段“*侦听传入请求。*” 使用 `app` 对象来响应消息。
      ``` typescript
      // Listen for incoming requests.
      server.post("/api/messages", async (req, res) => {
        await adapter.process(req, res, async (context) => {
          await app.run(context); //replace bot with app object
        });
      });
      ```

## 任务 3：创建自适应卡片

**目标**：在机器人中设计和实现静态和动态数据交互的自适应卡片。

1. 在项目根目录中创建名为 `cards` 的文件夹。
1. 在 `cards` 文件夹中，创建名为 `staticSearchCard.ts` 的文件，包含以下内容。 可以在 [staticSearchCard.ts](../../../Allfiles/Labs/Guided-Exercise6/staticSearchCard.ts) 中引用 `staticSearchCard.ts` 的最终文件。
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

1. 在 `cards` 文件夹中，创建一个名为 `dynamicSearchCard.ts` 的文件，文件内容如下。 可以在 [dynamicSearchCard.ts](../../../Allfiles/Labs/Guided-Exercise6/dynamicSearchCard.ts) 中引用 `dynamicSearchCard.ts` 的最终文件。
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

## 任务 4：处理消息

**目标**：开发机器人响应用户输入并通过自适应卡片进行交互的能力。

在此步骤中，我们将向 `app` 对象添加消息响应功能。

1. 打开 `app.ts` 文件。
1. 在创建应用对象 `const app = new Application();` 的代码下方，添加以下代码来处理不同的消息。
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

1. 在文件 `app.ts` 文件的顶部，按如下所示更新相关引用。 可以在 [app.ts](../../../Allfiles/Labs/Guided-Exercise6/app.ts) 中引用 `app.ts` 的最终文件。
    
    已更新
    ```` typescript
    import { ActivityTypes, TurnContext } from "botbuilder";
    import { createStaticSearchCard } from "./cards/staticSearchCard";
    import { createDynamicSearchCard } from "./cards/dynamicSearchCard";
    import axios from "axios";
    // See https://aka.ms/teams-ai-library to learn more about the Teams AI library.
    import { Application, TurnState, AdaptiveCardSearchResult } from "@microsoft/teams-ai";
    ````
    上一篇
    ```` typescript
    // See https://aka.ms/teams-ai-library to learn more about the Teams AI library.
    import { Application } from "@microsoft/teams-ai";
    ````    

## 检查你的工作

在本地运行你的应用以测试功能：

1. 打开 **TEAMS 工具包**平底板。 在“**开发**”菜单中，选择“**预览 Teams 应用**”（或使用 `F5` 键），然后选择使用你偏爱的浏览器“**在 Teams 中调试()**”。  
1. Teams Toolkit 将在浏览器中在本地预配和运行你的应用。
1. 在浏览器中的应用安装对话框中，选择“添加”以安装你的 Teams 应用。****  Teams 将打开与已安装机器人的对话。
1. 在消息对话框中输入 `static search`，然后按 Enter。 机器人将返回自适应卡片。
1. 在输入框中，选择或输入状态名称，然后选择“**提交**”按钮。 机器人将返回该状态名称的缩写。 ![自适应卡片静态搜索的屏幕截图](../../media/static-search.png)
1. 在消息对话框中输入 `dynamic search`，然后按 Enter。
1. 在输入对话框中，输入美国邮政编码并选择“**提交**”按钮。 机器人将返回该区域当前的天气。 ![自适应卡片动态搜索的屏幕截图](../../media/dynamic-search.png)