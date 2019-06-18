---
title: Dil anlama Bot Node.js v4
titleSuffix: Azure Cognitive Services
description: Node.js'yi kullanarak, dil anlama (LUIS) ile tümleşik bir sohbet robotu oluşturun. Bu sohbet robotu, bir robot çözümünü kısa sürede gerçekleştirmek için İnsan Kaynakları uygulamasını kullanır. Robot, Bot Framework 4 sürümü ve Azure Web uygulaması robotu ile geliştirilmiştir.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/15/2019
ms.author: diberry
ms.openlocfilehash: 832a62c5cc5440d81f4b92d2463a563f5bb884a3
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67150872"
---
# <a name="tutorial-use-a-web-app-bot-enabled-with-language-understanding-in-nodejs"></a>Öğretici: Node.js'de Language Understanding ile etkin bir Web App Botu kullanın 

Dil anlama (LUIS) ile tümleşik bir sohbet Robotu oluşturmak için node.js kullanma. Bot, Azure ile yerleşik [Web app botu](https://docs.microsoft.com/azure/bot-service/) kaynak ve [Bot Framework sürümü](https://github.com/Microsoft/botbuilder-dotnet) V4.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Web uygulaması robotu oluşturma. Bu işlem sizin için yeni bir LUIS uygulaması oluşturur.
> * Web bot hizmeti tarafından oluşturulan robot projesini indirin
> * Robotu ve öykünücüyü bilgisayarınızda yerel olarak başlatma
> * Robotta konuşma sonuçlarını görüntüleme

## <a name="prerequisites"></a>Önkoşullar

* [Robot öykünücüsü](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)


## <a name="create-a-web-app-bot-resource"></a>Bir web app botu kaynak oluştur

1. [Azure portalda](https://portal.azure.com) **Yeni kaynak oluştur**'u seçin.

1. Arama kutusunda **Web Uygulaması Robotu**'nu arayın ve bunu seçin. **Oluştur**’u seçin.

1. **Robot Hizmeti**'nde gerekli bilgileri sağlayın:

    |Ayar|Amaç|Önerilen ayar|
    |--|--|--|
    |Robot adı|Kaynak adı|`luis-nodejs-bot-` + `<your-name>`, örneğin `luis-nodejs-bot-johnsmith`|
    |Abonelik|Robotun oluşturulacağı abonelik.|Birincil aboneliğiniz.
    |Kaynak grubu|Azure kaynaklarının mantıksal grubu|Bu robotla kullanılan tüm kaynakları depolamak için yeni bir grup oluşturun ve grubu `luis-nodejs-bot-resource-group` olarak adlandırın.|
    |Konum|Azure bölgesi - LUIS yazma veya yayımlama bölgesi ile aynı olması gerekmez.|`westus`|
    |Fiyatlandırma katmanı|Hizmet isteği sınırları ve faturalandırma için kullanılır.|`F0` ücretsiz katmanı gösterir.
    |Uygulama adı|Ad, robotunuz buluta dağıtıldığında alt etki alanı olarak kullanılır (örneğin humanresourcesbot.azurewebsites.net).|`luis-nodejs-bot-` + `<your-name>`, örneğin `luis-nodejs-bot-johnsmith`|
    |Robot şablonu|Bot Framework ayarları - sonraki tabloya bakın|
    |LUIS Uygulaması konumu|LUIS kaynak bölgesi ile aynı olmalıdır|`westus`|
    |App service planı/konumu|Sağlanan varsayılan değeri değiştirmeyin.|
    |Application Insights|Sağlanan varsayılan değeri değiştirmeyin.|
    |Microsoft uygulama kimliği ve parola|Sağlanan varsayılan değeri değiştirmeyin.|

1. İçinde **Bot şablon**, aşağıdakileri seçin ve sonra seçin **seçin** düğmesi bu ayarlar altında:

    |Ayar|Amaç|Seçim|
    |--|--|--|
    |SDK sürümü|Robot Framework sürümü|**SDK v4**|
    |SDK dili|Robotun programlama dili|**Node.js**|
    |Bot|Robot türü|**Temel robot**|
    
1. **Oluştur**’u seçin. Robot hizmetini oluşturur ve Azure'a dağıtır. Bu işlemin bir parçası olarak `luis-nodejs-bot-XXXX` adlı bir LUIS uygulaması oluşturulur. Bu ad /Azure Bot hizmeti uygulaması adınıza temel alır.

    [![Web app botu oluşturun](./media/bfv4-nodejs/create-web-app-service.png)](./media/bfv4-nodejs/create-web-app-service.png#lightbox)

    Bot hizmeti, devam etmeden önce oluşturulana kadar bekleyin.

## <a name="the-bot-has-a-language-understanding-model"></a>Bot, bir dil anlama modeli vardır

Bot hizmeti oluşturma işlemi, ayrıca örnek konuşma amacı ile yeni bir LUIS uygulaması oluşturur. Robot, yeni LUIS uygulamasına şu amaçlar için amaç eşlemesi sağlar: 

|Temel robot LUIS amaçları|örnek konuşma|
|--|--|
|Kitap uçuş|`Travel to Paris`|
|İptal|`bye`|
|None|Uygulamanın etki alanı dışındaki her şey.|

## <a name="test-the-bot-in-web-chat"></a>Bot Web Chat test edin.

1. Yeni botu için Azure Portalı'nda hala seçin **Test Web sohbeti**. 
1. İçinde **iletinizi yazın** metin, metin girin `hello`. Bot, bir uçuştaki paris'e kayıt gibi belirli LUIS modeline için örnek sorgular yanı sıra bot framework hakkındaki bilgilerle yanıt verir. 

    ![Azure portal ekran görüntüsü, 'hello' metni girin.](./media/bfv4-nodejs/ask-bot-question-in-portal-test-in-web-chat.png)

    Botunuzun hızlı bir şekilde test etmek için test işlevleri kullanabilirsiniz. Daha fazla bilgi için hata ayıklama da dahil olmak üzere test tamamlamak bot kodu indirmek ve Visual Studio'yu kullanın. 

## <a name="download-the-web-app-bot-source-code"></a>Web app botu kaynak kodunu indirebilir
Web uygulaması robot kodunu geliştirmek için, yerel bilgisayarınızda kodu indirin ve kullanın. 

1. Azure portalda, **Robot yönetimi** bölümünden **Derle**'yi seçin. 

1. **Robot kaynak kodunu indir**'i seçin. 

    [![Temel robot için Web app botu kaynak kodunu indirebilir](../../../includes/media/cognitive-services-luis/bfv4/download-code.png)](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

1. Açılır iletişim kutusu sorduğunda **uygulama ayarları indirilen ZIP dosyasına eklenecek?** seçin **Evet**.

1. Kaynak kodu .zip dosyasına sıkıştırılmışsa, bir iletide kodu indirme bağlantısı sağlanır. Bağlantıyı seçin. 

1. Bu .zip dosyasını yerel bilgisayarınıza kaydedin ve dosyaları ayıklayın. Projeyi Visual Studio ile açın. 

## <a name="review-code-to-send-utterance-to-luis-and-get-response"></a>Utterance LUIS için gönderme ve yanıt almak için kod gözden geçirme

1. Açık **iletişim kutuları -> luisHelper.js** dosya. Robota girilen kullanıcı konuşmasının LUIS'e gönderildiği yer budur. LUIS gelen yanıt yönteminden döndürülen bir **bookDetails** JSON nesnesi. Kendi bot oluşturduğunuzda, ayrıca kendi nesne LUIS ayrıntılarını döndürecek şekilde oluşturmanız gerekir. 

    ```nodejs
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    const { LuisRecognizer } = require('botbuilder-ai');
    
    class LuisHelper {
        /**
         * Returns an object with preformatted LUIS results for the bot's dialogs to consume.
         * @param {*} logger
         * @param {TurnContext} context
         */
        static async executeLuisQuery(logger, context) {
            const bookingDetails = {};
    
            try {
                const recognizer = new LuisRecognizer({
                    applicationId: process.env.LuisAppId,
                    endpointKey: process.env.LuisAPIKey,
                    endpoint: `https://${ process.env.LuisAPIHostName }`
                }, {}, true);
    
                const recognizerResult = await recognizer.recognize(context);
    
                const intent = LuisRecognizer.topIntent(recognizerResult);
    
                bookingDetails.intent = intent;
    
                if (intent === 'Book_flight') {
                    // We need to get the result from the LUIS JSON which at every level returns an array
    
                    bookingDetails.destination = LuisHelper.parseCompositeEntity(recognizerResult, 'To', 'Airport');
                    bookingDetails.origin = LuisHelper.parseCompositeEntity(recognizerResult, 'From', 'Airport');
    
                    // This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
                    // TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
                    bookingDetails.travelDate = LuisHelper.parseDatetimeEntity(recognizerResult);
                }
            } catch (err) {
                logger.warn(`LUIS Exception: ${ err } Check your LUIS configuration`);
            }
            return bookingDetails;
        }
    
        static parseCompositeEntity(result, compositeName, entityName) {
            const compositeEntity = result.entities[compositeName];
            if (!compositeEntity || !compositeEntity[0]) return undefined;
    
            const entity = compositeEntity[0][entityName];
            if (!entity || !entity[0]) return undefined;
    
            const entityValue = entity[0][0];
            return entityValue;
        }
    
        static parseDatetimeEntity(result) {
            const datetimeEntity = result.entities['datetime'];
            if (!datetimeEntity || !datetimeEntity[0]) return undefined;
    
            const timex = datetimeEntity[0]['timex'];
            if (!timex || !timex[0]) return undefined;
    
            const datetime = timex[0].split('T')[0];
            return datetime;
        }
    }
    
    module.exports.LuisHelper = LuisHelper;
    ```

1. Açık **iletişim kutuları -> bookingDialog.js** BookingDetails nesne konuşma akışını yönetmek için nasıl kullanıldığını öğrenin. Seyahat ayrıntıları adımlarda istenir, ardından tüm kayıt Onaylandı ve son kullanıcıya geri yinelenir. 

    ```nodejs
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    const { TimexProperty } = require('@microsoft/recognizers-text-data-types-timex-expression');
    const { ConfirmPrompt, TextPrompt, WaterfallDialog } = require('botbuilder-dialogs');
    const { CancelAndHelpDialog } = require('./cancelAndHelpDialog');
    const { DateResolverDialog } = require('./dateResolverDialog');
    
    const CONFIRM_PROMPT = 'confirmPrompt';
    const DATE_RESOLVER_DIALOG = 'dateResolverDialog';
    const TEXT_PROMPT = 'textPrompt';
    const WATERFALL_DIALOG = 'waterfallDialog';
    
    class BookingDialog extends CancelAndHelpDialog {
        constructor(id) {
            super(id || 'bookingDialog');
    
            this.addDialog(new TextPrompt(TEXT_PROMPT))
                .addDialog(new ConfirmPrompt(CONFIRM_PROMPT))
                .addDialog(new DateResolverDialog(DATE_RESOLVER_DIALOG))
                .addDialog(new WaterfallDialog(WATERFALL_DIALOG, [
                    this.destinationStep.bind(this),
                    this.originStep.bind(this),
                    this.travelDateStep.bind(this),
                    this.confirmStep.bind(this),
                    this.finalStep.bind(this)
                ]));
    
            this.initialDialogId = WATERFALL_DIALOG;
        }
    
        /**
         * If a destination city has not been provided, prompt for one.
         */
        async destinationStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            if (!bookingDetails.destination) {
                return await stepContext.prompt(TEXT_PROMPT, { prompt: 'To what city would you like to travel?' });
            } else {
                return await stepContext.next(bookingDetails.destination);
            }
        }
    
        /**
         * If an origin city has not been provided, prompt for one.
         */
        async originStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            // Capture the response to the previous step's prompt
            bookingDetails.destination = stepContext.result;
            if (!bookingDetails.origin) {
                return await stepContext.prompt(TEXT_PROMPT, { prompt: 'From what city will you be travelling?' });
            } else {
                return await stepContext.next(bookingDetails.origin);
            }
        }
    
        /**
         * If a travel date has not been provided, prompt for one.
         * This will use the DATE_RESOLVER_DIALOG.
         */
        async travelDateStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            // Capture the results of the previous step
            bookingDetails.origin = stepContext.result;
            if (!bookingDetails.travelDate || this.isAmbiguous(bookingDetails.travelDate)) {
                return await stepContext.beginDialog(DATE_RESOLVER_DIALOG, { date: bookingDetails.travelDate });
            } else {
                return await stepContext.next(bookingDetails.travelDate);
            }
        }
    
        /**
         * Confirm the information the user has provided.
         */
        async confirmStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            // Capture the results of the previous step
            bookingDetails.travelDate = stepContext.result;
            const msg = `Please confirm, I have you traveling to: ${ bookingDetails.destination } from: ${ bookingDetails.origin } on: ${ bookingDetails.travelDate }.`;
    
            // Offer a YES/NO prompt.
            return await stepContext.prompt(CONFIRM_PROMPT, { prompt: msg });
        }
    
        /**
         * Complete the interaction and end the dialog.
         */
        async finalStep(stepContext) {
            if (stepContext.result === true) {
                const bookingDetails = stepContext.options;
    
                return await stepContext.endDialog(bookingDetails);
            } else {
                return await stepContext.endDialog();
            }
        }
    
        isAmbiguous(timex) {
            const timexPropery = new TimexProperty(timex);
            return !timexPropery.types.has('definite');
        }
    }
    
    module.exports.BookingDialog = BookingDialog;
    ```


## <a name="install-dependencies-and-start-the-bot-code-in-visual-studio"></a>Bağımlılıkları yükler ve bot kodu Visual Studio'da başlatma

1. VSCode içinde tümleşik terminalde bağımlılıklar yükleme komutu ile `npm install`.
1. Ayrıca tümleşik terminalde bot komutu ile başlatın `npm start`. 


## <a name="use-the-bot-emulator-to-test-the-bot"></a>Bot test etmek için robot öykünücüyü kullanma

1. Bot öykünücü başlar ve seçin **açık Bot**.
1. İçinde **bir bot açın** açılır iletişim kutusu, robot URL'nizi girin `http://localhost:3978/api/messages`. `/api/messages` Bot web adresini yoldur.
1. Girin **Microsoft uygulama kimliği** ve **Microsoft App parola**bölümüyle **.env** kök indirdiğiniz bot kod dosyasında.

    İsteğe bağlı olarak, yapılandırma ve kopyalama yeni bir bot oluşturabilirsiniz `MicrosoftAppId` ve `MicrosoftAppPassword` gelen **.env** bot Visual Studio Proje dosyasında. Robot yapılandırma dosyasının adı bot adıyla aynı olmalıdır. 

    ```json
    {
        "name": "<bot name>",
        "description": "<bot description>",
        "services": [
            {
                "type": "endpoint",
                "appId": "<appId from .env>",
                "appPassword": "<appPassword from .env>",
                "endpoint": "http://localhost:3978/api/messages",
                "id": "<don't change this value>",
                "name": "http://localhost:3978/api/messages"
            }
        ],
        "padlock": "",
        "version": "2.0",
        "overrides": null,
        "path": "<local path to .bot file>"
    }
    ```

1. Bot öykünücüde girin `Hello` ve temel robot, alınan olarak aynı yanıt alın **Test Web sohbeti**.

    [![Temel robot yanıt öykünücüsü](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png)](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png#lightbox)


## <a name="ask-bot-a-question-for-the-book-flight-intent"></a>Kitap uçuş amaç için robot soru

1. Bot öykünücüsü'nde, aşağıdaki utterance girerek bir kitap: 

    ```bot
    Book a flight from Paris to Berlin on March 22, 2020
    ```

    Onaylamak robot öykünücü ister. 

1. Seçin **Evet**. Bot eylemlerinin özetini ile yanıt verir. 
1. Bot öykünücü günlüğünden içeren satırı seçin `Luis Trace`. Bu, luıs'den JSON yanıtı utterance varlıklarının ve hedefi için görüntüler.

    [![Temel robot yanıt öykünücüsü](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png)](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png#lightbox)

## <a name="learn-more-about-the-web-app-bot-and-framework"></a>Web App Botu ve framework hakkında daha fazla bilgi edinin

Azure Bot hizmeti, Bot Framework SDK'sını kullanır. SDK ve bot çerçevesi hakkında daha fazla bilgi edinin:

* [Azure Bot Hizmeti](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0) v4 belgeleri
* [Bot Builder Örnekleri](https://github.com/Microsoft/botbuilder-samples)
* [Bot Builder Node.js SDK'sı](https://github.com/Microsoft/botbuilder-js)
* [Bot Builder araçları](https://github.com/Microsoft/botbuilder-tools):

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi bkz [örnekleri](https://github.com/microsoft/botframework-solutions) damıtarak konuşma bağlamında kullanılabilen bot ile. 

> [!div class="nextstepaction"]
> [Bir özel konu etki alanı ile bir konuşma tanıma uygulaması derleme](luis-quickstart-intents-only.md)