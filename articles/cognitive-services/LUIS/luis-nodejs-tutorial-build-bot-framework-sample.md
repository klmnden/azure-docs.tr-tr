---
title: Azure'da Node.js için Bot Oluşturucu SDK'yı kullanarak bir bot HALUK tümleştirileceğini | Microsoft Docs
description: Bot Framework kullanarak bir HALUK uygulaması ile tümleşik bir bot oluşturun.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/06/2018
ms.author: v-geberr
ms.openlocfilehash: 12cc84942c139d3c5e981aec902557201c9c8092
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355769"
---
# <a name="integrate-luis-with-a-bot-using-the-bot-builder-sdk-for-nodejs"></a>Node.js için Bot Oluşturucu SDK'yı kullanarak bir bot HALUK tümleştirileceğini

Bu öğreticide, bir bot ile oluşturma sürecinde açıklanmaktadır [Bot Framework] [ BotFramework] HALUK uygulama ile tümleşiktir.

## <a name="prerequisite"></a>Önkoşul

Bot oluşturmadan önce adımları [bir uygulama oluşturmak](./luis-get-started-create-app.md) kullandığı HALUK uygulamanızı oluşturmak için.

Bot HALUK uygulamada olan HomeAutomation etki alanından amaçlar için yanıt verir. Her bu amaçlar için varlığa eşlenen bir hedefi HALUK uygulama sağlar. Bot HALUK algılar hedefi işleyen bir iletişim kutusu sağlar.

| Hedefi | Örnek utterance | Bot işlevi |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Üzerinde ışık açın. | Bot çağırır `TurnOnDialog` zaman `HomeAutomation.TurnOn` algılandı. Bu iletişim kutusu, burada bir aygıtı açın ve aygıtın açık üzerinde kullanıcıdan bir IOT hizmeti çağırma ' dir. |
| HomeAutomation.TurnOff | Yatak ışık devre dışı bırakın. | Bot çağırır `TurnOffDialog` zaman `HomeAutomation.TurnOff` algılandı. Burada bir aygıtı kapatın ve aygıtı devre dışı kullanıcı bildirmek üzere bir IOT hizmet çağırma bu iletişim kutusu. |


## <a name="create-a-language-understanding-bot-with-bot-service"></a>Bir dil anlama bot Bot hizmeti ile oluşturma

1. İçinde [Azure portal](https://portal.azure.com)seçin **yeni kaynak oluşturmak** menü dikey seçip **tümünü görmek**.

    ![Yeni kaynak oluştur](./media/luis-tutorial-node-bot/bot-service-creation.png)

2. Arama kutusuna arama **Web uygulaması Bot**. 

    ![Yeni kaynak oluştur](./media/luis-tutorial-node-bot/bot-service-selection.png)

3. İçinde **Bot hizmet** dikey penceresinde gerekli bilgileri sağlayın ve seçin **oluşturma**. Bu oluşturur ve bot hizmet ve HALUK uygulama için Azure dağıtır. Kullanmak istiyorsanız, [konuşma Hazırlama işlemi](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming), gözden [bölge gereksinimleri](luis-resources-faq.md#what-luis-regions-support-bot-framework-speech-priming) , bot oluşturmadan önce. 
    * Ayarlama **uygulama adı** bot kişinin adı. Bot (örneğin, mynotesbot.azurewebsites.net) buluta dağıtıldığında alt etki alanı adı kullanılır. <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
    * Abonelik seç [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), uygulama hizmeti planı ve [konumu](https://azure.microsoft.com/regions/).
    * Seçin **dil anlama (Node.js)** için şablon **Bot şablonu** alan.
    * Seçin **HALUK uygulamasının konumuna**. Yazma budur [bölge] [ LUIS] uygulama oluşturulur.
    * Yasal bildirim için onay kutusunu seçin. Yasal bildirim koşullarını onay kutusu var.

    ![Bot hizmeti dikey](./media/luis-tutorial-node-bot/bot-service-setting-callout-template.png)


4. Bot hizmet dağıtıldığını doğrulayın.
    * Bildirimleri (Azure portalı üst kenarına bulunan zil simgesine) seçin. Bildirim değiştirilecek **dağıtım başladı** için **dağıtım başarılı**.
    * Bildirim değişiklikler yapıldıktan sonra **dağıtım başarılı**seçin **kaynağa gidin** bu bildirim.

## <a name="try-the-default-bot"></a>Varsayılan bot deneyin

Bot denetleyerek dağıtıldığını doğrulayın **bildirimleri**. Bildirimleri değiştirilecek **dağıtımı devam ediyor...**  için **dağıtım başarılı**. Seçin **kaynağa gidin** bot ait kaynak dikey penceresini açmak için düğmesi.

<!-- this step isn't supposed to be necessary -->
## <a name="install-npm-resources"></a>NPM kaynakları yükleyin
NPM paket aşağıdaki adımlarla yükleyin:

1. Seçin **yapı** gelen **Bot Yönetim** Web uygulaması Bot bölümü. 

2. Bir tarayıcı penceresi açılır'yeni, ikinci. Seçin **açık çevrimiçi Kod düzenleyicisinde**.

3. Web uygulaması bot adı üst gezinti çubuğunda seçin `homeautomationluisbot`. 

4. Aşağı açılan listesinde, **açık Kudu Konsolu**.

5. Yeni bir tarayıcı penceresi açar. Konsolda, aşağıdaki komutu girin:

    ```
    cd site\wwwroot && npm install
    ```

    Yükleme işleminin tamamlanması için bekleyin. İlk tarayıcı penceresine dönün. 

## <a name="test-in-web-chat"></a>Web sohbeti test
Bot kaydedildiğinde, seçin **Web sohbeti testinde** Web sohbet bölmesini açmak için. "Web sohbet hello" yazın.

  ![Web sohbet bot test](./media/luis-tutorial-node-bot/bot-service-web-chat.png)

Bot "Tebrik ulaştınız. söyleyerek yanıt verir. Belirtilmektedir: hello ". Bu, bot, ileti aldı ve varsayılan olarak oluşturulan HALUK uygulama geçirilen doğrular. Bu varsayılan HALUK uygulama Tebrik hedefi algıladı. Sonraki adımda HALUK uygulama varsayılan yerine daha önce oluşturduğunuz HALUK App bot bağlanırsınız.

## <a name="connect-your-luis-app-to-the-bot"></a>Bot HALUK uygulamanızı bağlama

Açık **uygulama ayarları** ilk tarayıcı penceresinde ve düzenleme **LuisAppId** HALUK uygulamanızı uygulama Kimliğini içeren alan.

  ![Azure HALUK uygulama Kimliğini güncelleştir](./media/luis-tutorial-node-bot/bot-service-app-id.png)

HALUK uygulama kimliği yoksa, oturum [HALUK](luis-reference-regions.md) Azure'da oturum açma için kullandığınız aynı hesabı kullanarak Web sitesi. SELECT deyiminde **uygulamalarım**. 

1. Hedefleri ve HomeAutomation etki alanından varlıkları içeren daha önce oluşturduğunuz, HALUK uygulamayı bulun.

2. İçinde **ayarları** sayfasında HALUK, bulmak ve uygulama kimliği kopyalayın.

3. Uygulama eğitilmiş yapmadıysanız, seçin **eğitmek** uygulamanızı eğitmek için sağ üst düğmesi.

4. Uygulama yayımladıysanız henüz seçin **Yayımla** açmak için üst gezinti çubuğundaki **Yayımla** sayfası. Üretim yuvasına seçin ve **Yayımla** düğmesi.

## <a name="modify-the-bot-code"></a>Bot kodunu değiştirme

Hala açın veya ilk tarayıcı penceresinde seçin ise, ikinci bir tarayıcı penceresi Git **yapı** ve ardından **açık çevrimiçi Kod düzenleyicisinde**.

   ![Açık çevrimiçi Kod Düzenleyicisi](./media/luis-tutorial-node-bot/bot-service-build.png)

Kod düzenleyicisinde açın `app.js`. Aşağıdaki kod içerir:

```javascript
/*-----------------------------------------------------------------------------
A simple Language Understanding (LUIS) bot for the Microsoft Bot Framework. 
-----------------------------------------------------------------------------*/

var restify = require('restify');
var builder = require('botbuilder');
var botbuilder_azure = require("botbuilder-azure");

// Setup Restify Server
var server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
   console.log('%s listening to %s', server.name, server.url); 
});
  
// Create chat connector for communicating with the Bot Framework Service
var connector = new builder.ChatConnector({
    appId: process.env.MicrosoftAppId,
    appPassword: process.env.MicrosoftAppPassword,
    openIdMetadata: process.env.BotOpenIdMetadata 
});

// Listen for messages from users 
server.post('/api/messages', connector.listen());

/*----------------------------------------------------------------------------------------
* Bot Storage: This is a great spot to register the private state storage for your bot. 
* We provide adapters for Azure Table, CosmosDb, SQL Azure, or you can implement your own!
* For samples and documentation, see: https://github.com/Microsoft/BotBuilder-Azure
* ---------------------------------------------------------------------------------------- */

var tableName = 'botdata';
var azureTableClient = new botbuilder_azure.AzureTableClient(tableName, process.env['AzureWebJobsStorage']);
var tableStorage = new botbuilder_azure.AzureBotStorage({ gzipData: false }, azureTableClient);

// Create your bot with a function to receive messages from the user
// This default message handler is invoked if the user's utterance doesn't
// match any intents handled by other dialogs.
var bot = new builder.UniversalBot(connector, function (session, args) {
    session.send('You reached the default message handler. You said \'%s\'.', session.message.text);
});

bot.set('storage', tableStorage);

// Make sure you add code to validate these fields
var luisAppId = process.env.LuisAppId;
var luisAPIKey = process.env.LuisAPIKey;
var luisAPIHostName = process.env.LuisAPIHostName || 'westus.api.cognitive.microsoft.com';

const LuisModelUrl = 'https://' + luisAPIHostName + '/luis/v2.0/apps/' + luisAppId + '?subscription-key=' + luisAPIKey;

// Create a recognizer that gets intents from LUIS, and add it to the bot
var recognizer = new builder.LuisRecognizer(LuisModelUrl);
bot.recognizer(recognizer);

// Add a dialog for each intent that the LUIS app recognizes.
// See https://docs.microsoft.com/bot-framework/nodejs/bot-builder-nodejs-recognize-intent-luis 
bot.dialog('GreetingDialog',
    (session) => {
        session.send('You reached the Greeting intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Greeting'
})

bot.dialog('HelpDialog',
    (session) => {
        session.send('You reached the Help intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Help'
})

bot.dialog('CancelDialog',
    (session) => {
        session.send('You reached the Cancel intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Cancel'
})
```

App.js içinde varolan hedefleri göz ardı edilir. Bunları bırakabilirsiniz. 

## <a name="add-a-dialog-that-matches-homeautomationturnon"></a>Ekle HomeAutomation.TurnOn eşleşen bir iletişim kutusu

Aşağıdaki kodu kopyalayın ve bunu ekleyin `app.js`.

```javascript
bot.dialog('TurnOn',
    (session) => {
        session.send('You reached the TurnOn intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'HomeAutomation.TurnOn'
})
```

[Eşleşen] [ matches] seçeneği [triggerAction] [ triggerAction] bağlı iletişim hedefi adını belirtir. Tanıyıcı bot kullanıcıdan bir utterance alan her zaman çalışır. Algıladığı yüksek Puanlama amacıyla eşleşmesi durumunda bir `triggerAction` bir iletişim kutusu bağlı, bot bu iletişim kutusunu çağırır.

## <a name="add-a-dialog-that-matches-homeautomationturnoff"></a>Ekle HomeAutomation.TurnOff eşleşen bir iletişim kutusu

Aşağıdaki kodu kopyalayın ve bunu ekleyin `app.js`.

```javascript
bot.dialog('TurnOff',
    (session) => {
        session.send('You reached the TurnOff intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'HomeAutomation.TurnOff'
})
```
## <a name="test-the-bot"></a>Bot test

Azure Portalı'nda seçin **Test Web sohbet** bot test etmek için. Türü iletileri deneyin LIKE "üzerinde ışık kapatın" ve "devre dışı my ısıtıcı" kendisine eklenmiş hedefleri çağırmak için.
   ![Web sohbet HomeAutomation bot test](./media/luis-tutorial-node-bot/bot-service-chat-results.png)

> [!TIP]
> Bot her zaman doğru amacını veya varlıklar tanımıyor bulursanız, onu eğitmek için daha fazla örnek utterances vererek HALUK uygulamanızın performansı iyileştirir. HALUK uygulamanızı bot's kod kendilerine herhangi bir değişiklik olmadan yeniden eğitme. Bkz: [örnek utterances ekleme](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) ve [eğitmek ve HALUK uygulamanızı test](https://docs.microsoft.com/azure/cognitive-services/LUIS/train-test).

## <a name="learn-more-about-bot-framework"></a>Bot Framework hakkında daha fazla bilgi edinin
Daha fazla bilgi edinmek [Bot Framework](https://dev.botframework.com/) ve [3.x](https://github.com/Microsoft/BotBuilder) ve [4.x](https://github.com/Microsoft/botbuilder-js) SDK'ları.

## <a name="next-steps"></a>Sonraki adımlar

<!-- From trying the bot, you can see that the recognizer can trigger interruption of the currently active dialog. Allowing and handling interruptions is a flexible design that accounts for what users really do. Learn more about the various actions you can associate with a recognized intent.-->
Yardım, iptal et ve tebrik, gibi diğer amaçlar için HALUK uygulama eklemek deneyebilirsiniz. Ardından yeni hedefleri iletişim kutularında ekleyin ve bunları bot kullanarak test. 

<!-- 
> [!NOTE] 
> The default LUIS app that the template created contains example utterances for Cancel, Greeting, and Help intents. In the list of apps, find the app that begins with the name specified in **App name** in the **Bot Service** blade when you created the Bot Service. -->

> [!div class="nextstepaction"]
> [Hedefleri Ekle](./luis-how-to-add-intents.md)
> [konuşma Hazırlama işlemi](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)


[intentDialog]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.intentdialog.html

[intentDialog_matches]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.intentdialog.html#matches 

[NotesSample]: https://github.com/Microsoft/BotFramework-Samples/tree/master/docs-samples/Node/basics-naturalLanguage

[triggerAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#triggeraction

[confirmPrompt]: https://docs.botframework.com/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#confirmprompt

[waterfall]: bot-builder-nodejs-dialog-manage-conversation-flow.md#manage-conversation-flow-with-a-waterfall

[session_userData]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.session.html#userdata

[EntityRecognizer_findEntity]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.entityrecognizer.html#findentity

[matches]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#matches

[LUISAzureDocs]: https://docs.microsoft.com/azure/cognitive-services/LUIS/Home

[Dialog]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html

[IntentRecognizerSetOptions]: https://docs.botframework.com/node/builder/chat-reference/interfaces/_botbuilder_d_.iintentrecognizersetoptions.html

[LuisRecognizer]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.luisrecognizer

[LUISConcepts]: https://docs.botframework.com/node/builder/guides/understanding-natural-language/

[DisambiguationSample]: https://github.com/Microsoft/BotBuilder/tree/master/Node/examples/feature-onDisambiguateRoute

[IDisambiguateRouteHandler]: https://docs.botframework.com/node/builder/chat-reference/interfaces/_botbuilder_d_.idisambiguateroutehandler.html

[RegExpRecognizer]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.regexprecognizer.html

[AlarmBot]: https://github.com/Microsoft/BotBuilder/blob/master/Node/examples/basics-naturalLanguage/app.js

[LUISBotSample]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/intelligence-LUIS

[UniversalBot]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.universalbot.html


<!-- Old Links -->
[Github-BotFramework-Emulator-Download]: https://aka.ms/bot-framework-emulator
[Github-LUIS-Samples]: https://github.com/Microsoft/LUIS-Samples
[Github-LUIS-Samples-node-hotel-bot]:https://github.com/Microsoft/LUIS-Samples/tree/master/bot-integration-samples/hotel-finder/nodejs
[NodeJs]: https://nodejs.org/
[BFPortal]: https://dev.botframework.com/
[RegisterInstructions]: https://docs.microsoft.com/bot-framework/portal-register-bot
[BotFramework]: https://docs.microsoft.com/bot-framework/
[LUIS]:luis-reference-regions.md

