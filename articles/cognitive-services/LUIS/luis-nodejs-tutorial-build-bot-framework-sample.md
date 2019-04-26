---
title: Bot - Node.js - v3
titleSuffix: Azure Cognitive Services
description: Bir LUIS uygulaması ile Bot Framework ile tümleşik bir bot oluşturun 3.x ve Azure Web app botu.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/30/2019
ms.author: diberry
ms.openlocfilehash: 27ebe827e97c8361c7f93a125f09aa9e339a6a14
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60197056"
---
# <a name="luis-bot-in-nodejs-with-the-bot-framework-3x-and-the-azure-web-app-bot"></a>Bot Framework Node.js içindeki robotla LUIS 3.x ve Azure Web app botu

Node.js'yi kullanarak, dil anlama (LUIS) ile tümleşik bir sohbet robotu oluşturun. Bu sohbet Robotu, hızlı bir şekilde bir bot çözümü uygulamak için önceden oluşturulmuş HomeAutomation etki alanını kullanır. Bot, Bot Framework ile derlenir 3.x ve Azure Web app botu.

## <a name="prerequisite"></a>Önkoşul

Bot oluşturmadan önce adımları [uygulama oluşturma](./luis-get-started-create-app.md) kullandığı LUIS uygulaması oluşturun.

Bot LUIS uygulaması içinde bulunan HomeAutomation etki alanından hedefleri için yanıt verir. Her biri bu hedefleri için kendisine eşleyen bir hedefi LUIS uygulaması sağlar. Bot LUIS algılar hedefi işleyen bir iletişim kutusu sağlar.

| Amaç | Örnek konuşma | Bot işlevi |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Işıkları aç'ı açın. | Bot çağırır `TurnOnDialog` olduğunda `HomeAutomation.TurnOn` algılandı. Bu iletişim kutusu, burada cihaz kapalıyken üzerinde kullanıcı bir cihazı açın ve bir IOT hizmeti çağırmak ' dir. |
| HomeAutomation.TurnOff | Yatak odası Işıkları aç. | Bot çağırır `TurnOffDialog` olduğunda `HomeAutomation.TurnOff` algılandı. Burada cihaz devre dışı kullanıcı cihazı kapatıp ve bir IOT hizmeti çağırmak bu iletişim kutusu. |


## <a name="create-a-language-understanding-bot-with-bot-service"></a>Language Understanding bot ile Bot hizmeti oluşturma

1. İçinde [Azure portalında](https://portal.azure.com)seçin **yeni kaynak Oluştur** menü dikey seçip **tümünü gör**.

    ![Azure portalında tüm kaynaklara bakın](./media/luis-tutorial-node-bot/bot-service-creation.png)

2. Arama kutusuna arama **Web App Botu**. 

    ![Kaynak oluşturma işlemini başlatmak için web app botu seçin](./media/luis-tutorial-node-bot/bot-service-selection.png)

3. İçinde **Bot hizmeti** dikey penceresinde gerekli bilgileri sağlayın ve seçin **Oluştur**. Bu, oluşturur ve Azure bot hizmeti ve LUIS uygulaması dağıtır. Kullanmak istiyorsanız [konuşma Hazırlama işlemi](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming), gözden [bölge gereksinimleri](troubleshooting.md#what-luis-regions-support-bot-framework-speech-priming) botunuzun oluşturmadan önce. 
   * Ayarlama **uygulama adı** botunuzun kişinin adı. Botunuzun (örneğin, mynotesbot.azurewebsites.net) buluta dağıtıldığında alt etki alanı adı kullanılır. <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
   * Aboneliği seçin [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), App service planı ve [konumu](https://azure.microsoft.com/regions/).
   * İçin **Bot şablon**seçin:
       * **SDK'sı v3**
       * **Node.js**
       * **Dil anlama**
   * Seçin **LUIS uygulama konumu**. Bu yazma, [bölge] [ LUIS] uygulama oluşturulur.
   * Yasal bildirimi için onay kutusunu seçin. Yasal bildirimin koşullarını onay kutusunu ' dir.

     ![Bot hizmeti dikey](./media/luis-tutorial-node-bot/bot-service-setting-callout-template.png)


4. Bot hizmeti dağıtıldığını doğrulayın.
    * Bildirimleri (Azure portalının üst kenarı bulunan zil simgesi) seçin. Bildirim değiştirilecek **dağıtım başlatıldı** için **dağıtım başarılı**.
    * Bildirim değişiklikler yapıldıktan sonra **dağıtım başarılı**seçin **kaynağa Git** bu bildirim.

## <a name="try-the-default-bot"></a>Varsayılan bot deneyin

Bot denetleyerek dağıtıldığını doğrulayın **bildirimleri**. Bildirimleri değiştirilecek **dağıtım sürüyor...**  için **dağıtım başarılı**. Seçin **kaynağa Git** botun kaynak dikey penceresini açmak için düğmeyi.

<!-- this step isn't supposed to be necessary -->
## <a name="install-npm-resources"></a>NPM kaynakları yükleme
Aşağıdaki adımlarla NPM paketlerini yükle:

1. Seçin **derleme** gelen **Bot Yönetim** Web App Botu bölümü. 

2. Bir tarayıcı penceresi açılır'yeni, ikinci. Seçin **açık çevrimiçi Kod Düzenleyicisi**.

3. Üst gezinti çubuğunda, web app botu adını seçin `homeautomationluisbot`. 

4. Aşağı açılan listesinde seçin **Kudu konsolu aç**.

5. Yeni bir tarayıcı penceresi açılır. Konsolunda, aşağıdaki komutu girin:

    ```console
    cd site\wwwroot && npm install
    ```

    Yükleme işleminin tamamlanması için bekleyin. İlk tarayıcı penceresine dönün. 

## <a name="test-in-web-chat"></a>Test Web sohbeti
Bot kaydedildikten sonra seçin **Test Web sohbeti** Web Chat bölmesinde açmak için. "Web Chat hello" yazın.

  ![Bot Web Chat test edin.](./media/luis-tutorial-node-bot/bot-service-web-chat.png)

Bot "Karşılama sınırına ulaştınız. diyerek yanıt verir. Bununla birlikte,: hello ". Bu bot, bir ileti aldı ve varsayılan olarak oluşturulan LUIS uygulaması geçirilen doğrular. Bu varsayılan LUIS uygulaması Karşılama hedefi algılandı. Sonraki adımda, varsayılan LUIS uygulaması yerine daha önce oluşturduğunuz LUIS uygulaması için robot bağlanırsınız.

## <a name="connect-your-luis-app-to-the-bot"></a>İçin robot LUIS uygulamanızı bağlayın

Açık **uygulama ayarları** ilk tarayıcı penceresinde ve düzenleme **LuisAppId** LUIS uygulamanızı uygulama Kimliğini içeren alan.

  ![Azure'da LUIS uygulama kodunu güncelleştir](./media/luis-tutorial-node-bot/bot-service-app-id.png)

LUIS uygulama kimliği yoksa, oturum [LUIS](luis-reference-regions.md) Azure'da oturum açmak için kullandığınız hesabın aynısını kullanarak Web sitesi. SELECT deyiminde **uygulamalarım**. 

1. Amaç ve varlıkları HomeAutomation etki alanından içeren daha önce oluşturduğunuz, LUIS uygulaması bulun.

2. İçinde **ayarları** LUIS uygulaması için sayfasında, bulmak ve uygulama kimliği kopyalayın.

3. Uygulama eğitilmiş yapmadıysanız, seçin **eğitme** uygulamanızı geliştirmek için sağ üst köşedeki düğmesi.

4. Uygulamayı yayımladığınız yapmadıysanız, seçin **Yayımla** açmak için üst gezinti çubuğunda **Yayımla** sayfası. Production (Üretim) yuvasını ve ardından **Publish** (Yayımla) düğmesini seçin.

## <a name="modify-the-bot-code"></a>Bot kodu değiştirin

Yine de açmak veya ilk tarayıcı penceresinde seçin ise ikinci bir tarayıcı penceresi Git **derleme** seçip **açık çevrimiçi Kod Düzenleyicisi**.

   ![Açık çevrimiçi Kod Düzenleyicisi](./media/luis-tutorial-node-bot/bot-service-build.png)

Kod Düzenleyicisi'nde açın `app.js`. Bunu, aşağıdaki kodu içerir:

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

Var olan hedefleri app.js içinde göz ardı edilir. Bunları bırakabilirsiniz. 

## <a name="add-a-dialog-that-matches-homeautomationturnon"></a>HomeAutomation.TurnOn eşleşen bir iletişim kutusu Ekle

Aşağıdaki kodu kopyalayın ve eklemeniz `app.js`.

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

[Eşleşen] [ matches] seçeneğini [triggerAction] [ triggerAction] bağlı iletişim amaç adını belirtir. Tanıyıcı bot, kullanıcıdan bir utterance aldığı her zaman çalışır. Algıladığı yüksek Puanlama hedefi eşleşmesi durumunda bir `triggerAction` iletişim kutusuna bağlı, söz konusu iletişim bot çağırır.

## <a name="add-a-dialog-that-matches-homeautomationturnoff"></a>HomeAutomation.TurnOff eşleşen bir iletişim kutusu Ekle

Aşağıdaki kodu kopyalayın ve eklemeniz `app.js`.

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

Azure Portalı'nda seçin **Test Web sohbeti içinde** bot test etmek için. Türü iletileri deneyin benzer "kapatma ışıkları Aç" ve "my heater kapatma" ona eklediğiniz ıntents çağırmak için.
   ![HomeAutomation bot Web Chat test edin.](./media/luis-tutorial-node-bot/bot-service-chat-results.png)

> [!TIP]
> Botunuzun doğru hedefi veya varlıklar her zaman tanımıyor bulursanız, eğitmek için daha fazla örnek konuşma vererek LUIS uygulamanızın performansını artırın. LUIS uygulamanızı, botun kod değişiklik olmadan yeniden eğitebilir. Bkz: [örnek Konuşma ekleme](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) ve [eğitme ve LUIS uygulamanızı test etme](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-interactive-test).

## <a name="learn-more-about-bot-framework"></a>Bot Framework hakkında daha fazla bilgi edinin
Daha fazla bilgi edinin [Bot Framework](https://dev.botframework.com/) ve [3.x](https://github.com/Microsoft/BotBuilder) ve [4.x](https://github.com/Microsoft/botbuilder-js) SDK'ları.

## <a name="next-steps"></a>Sonraki adımlar

<!-- From trying the bot, you can see that the recognizer can trigger interruption of the currently active dialog. Allowing and handling interruptions is a flexible design that accounts for what users really do. Learn more about the various actions you can associate with a recognized intent.-->
Karşılama, Yardım ve iptal etme gibi diğer amaçlar için LUIS uygulaması eklemek deneyebilirsiniz. Ardından yeni hedefleri için iletişim kutularını ekleyin ve bunları botunu kullanarak test edin. 

<!-- 
> [!NOTE] 
> The default LUIS app that the template created contains example utterances for Cancel, Greeting, and Help intents. In the list of apps, find the app that begins with the name specified in **App name** in the **Bot Service** blade when you created the Bot Service. -->

> [!div class="nextstepaction"]
> [Hedef ekleme](./luis-how-to-add-intents.md)
> [konuşma Hazırlama işlemi](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)


[triggerAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#triggeraction

[matches]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#matches


[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions

