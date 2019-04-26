---
title: Bot - Node.js - v4
titleSuffix: Azure Cognitive Services
description: Node.js'yi kullanarak, dil anlama (LUIS) ile tümleşik bir sohbet robotu oluşturun. Bu sohbet robotu, bir robot çözümünü kısa sürede gerçekleştirmek için İnsan Kaynakları uygulamasını kullanır. Robot, Bot Framework 4 sürümü ve Azure Web uygulaması robotu ile geliştirilmiştir.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 01/30/2019
ms.author: diberry
ms.openlocfilehash: 54bae5548764ed1f89a2ffb7992eb222a058c706
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60194163"
---
# <a name="tutorial-luis-bot-in-nodejs-with-the-bot-framework-4x-and-the-azure-web-app-bot"></a>Öğretici: Node.js'de LUIS bot ile Bot Framework 4.x ve Azure Web app botu
Node.js'yi kullanarak, dil anlama (LUIS) ile tümleşik bir sohbet robotu oluşturabilirsiniz. Bu robot, bir robot çözümü gerçekleştirmek için HomeAutomation uygulamasını kullanır. Robot, [Bot Framework sürümü](https://github.com/Microsoft/botbuilder-js) v4 ile Azure [Web uygulaması robotu](https://docs.microsoft.com/azure/bot-service/) kullanılarak geliştirilmiştir.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

> [!div class="checklist"]
> * Web uygulaması robotu oluşturma. Bu işlem sizin için yeni bir LUIS uygulaması oluşturur.
> * Yeni LUIS modeline önceden oluşturulmuş bir etki alanı ekleme
> * Web robot hizmeti tarafından oluşturulan projeyi indirme
> * Robotu ve öykünücüyü bilgisayarınızda yerel olarak başlatma
> * Yeni LUIS amaçları için robot kodunu değiştirme
> * Robotta konuşma sonuçlarını görüntüleme

## <a name="prerequisites"></a>Önkoşullar

* [Robot öykünücüsü](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)


## <a name="create-web-app-bot"></a>Web uygulaması robotu oluşturma

1. [Azure portalda](https://portal.azure.com) **Yeni kaynak oluştur**'u seçin.

2. Arama kutusunda **Web Uygulaması Robotu**'nu arayın ve bunu seçin. **Oluştur**’u seçin.

3. **Robot Hizmeti**'nde gerekli bilgileri sağlayın:

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

4. **Robot şablon ayarları**'nda aşağıdakileri seçin ve sonra da bu ayarların altındaki **Seç** düğmesini seçin:

    |Ayar|Amaç|Seçim|
    |--|--|--|
    |SDK sürümü|Robot Framework sürümü|**SDK v4**|
    |SDK dili|Robotun programlama dili|**Node.js**|
    |Yankı/Temel robot|Robot türü|**Temel robot**|
    
5. **Oluştur**’u seçin. Robot hizmetini oluşturur ve Azure'a dağıtır. Bu işlemin bir parçası olarak `luis-nodejs-bot-XXXX` adlı bir LUIS uygulaması oluşturulur. Buradaki ad, önceki bölümdeki robot ve uygulama adlarını temel alır.

    [![Web app botu oluşturun](./media/bfv4-nodejs/create-web-app-service.png)](./media/bfv4-nodejs/create-web-app-service.png#lightbox)

6. Bu tarayıcı sekmesini açık bırakın. LUIS portalındaki her adım için yeni bir tarayıcı sekmesi açın. Yeni robot hizmet dağıtıldığında sonraki bölüme devam edin.

## <a name="add-prebuilt-domain-to-model"></a>Modele önceden oluşturulmuş etki alanı ekleme
Robot hizmetinin dağıtımı sırasında amaçlar ve örnek konuşmalar içeren yeni bir LUIS uygulaması oluşturulur. Robot, yeni LUIS uygulamasına şu amaçlar için amaç eşlemesi sağlar: 

|Temel robot LUIS amaçları|örnek konuşma|
|--|--|
|İptal|`stop`|
|Karşılama|`hello`|
|Yardım|`help`|
|None|Uygulamanın etki alanı dışındaki her şey.|

Şunun gibi konuşmalar için önceden oluşturulmuş HomeAutomation uygulamasını modele ekleyin: `Turn off the living room lights`

1. [LUIS](https://www.luis.ai) portalına gidin ve oturum açın.
2. **Uygulamalarım** sayfasında, uygulamaları oluşturma tarihine göre sıralamak için **Oluşturma tarihi** sütununu seçin. Azure Robot hizmeti önceki bölümde yeni bir uygulama oluşturmuştu. Adı `luis-nodejs-bot-` + `<your-name>` + 4 rasgele karakterdir.
3. Uygulamayı açın ve üst gezintide **Derle** bölümünü seçin.
4. Sol gezintide **Önceden Oluşturulmuş Etki Alanları**'nı seçin.
5. **HomeAutomation** etki alanını seçmek için, kartının üzerindeki **Etki alanı ekle**'yi seçin.
6. Sağ üstteki menüde **Eğit**'i seçin.
7. Sağ üstteki menüde **Yayımla**'yı seçin. 

    Azure Robot hizmeti tarafından oluşturulan uygulamada artık yeni amaçlar bulunur:

    |Temel robot yeni amaçları|örnek konuşma|
    |--|--|
    |HomeAutomation.TurnOn|`turn the fan to high`
    |HomeAutomation.TurnOff|`turn off ac please`|

## <a name="download-the-web-app-bot"></a>Web uygulaması robotunu indirme 
Web uygulaması robot kodunu geliştirmek için, yerel bilgisayarınızda kodu indirin ve kullanın. 

1. Azure portalda, yine Web uygulama robotu kaynağında **Uygulama Ayarları**'nı seçin, sonra da **botFilePath** ve **botFileSecret** değerlerini kopyalayın. Bunları daha sonra bir ortam dosyasına eklemeniz gerekir. 

2. Azure portalda, **Robot yönetimi** bölümünden **Derle**'yi seçin. 

3. **Robot kaynak kodunu indir**'i seçin. 

    [![Temel robot için Web app botu kaynak kodunu indirebilir](../../../includes/media/cognitive-services-luis/bfv4/download-code.png)](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

4. Kaynak kodu .zip dosyasına sıkıştırılmışsa, bir iletide kodu indirme bağlantısı sağlanır. Bağlantıyı seçin. 

5. Bu .zip dosyasını yerel bilgisayarınıza kaydedin ve dosyaları ayıklayın. Projeyi açın. 

6. Bot.js dosyasını açın ve `const results = await this.luisRecognizer.recognize(context);` ifadesini arayın. Robota girilen kullanıcı konuşmasının LUIS'e gönderildiği yer burasıdır.

   ```javascript
    /**
     * Driver code that does one of the following:
     * 1. Display a welcome card upon startup
     * 2. Use LUIS to recognize intents
     * 3. Start a greeting dialog
     * 4. Optionally handle Cancel or Help interruptions
     *
     * @param {Context} context turn context from the adapter
     */
    async onTurn(context) {
        // Create a dialog context
        const dc = await this.dialogs.createContext(context);

        if(context.activity.type === ActivityTypes.Message) {
            // Perform a call to LUIS to retrieve results for the current activity message.
            const results = await this.luisRecognizer.recognize(context);
            
            const topIntent = LuisRecognizer.topIntent(results);

            // handle conversation interrupts first
            const interrupted = await this.isTurnInterrupted(dc, results);
            if(interrupted) {
                return;
            }

            // Continue the current dialog
            const dialogResult = await dc.continue();

            switch(dialogResult.status) {
                case DialogTurnStatus.empty:
                    switch (topIntent) {
                        case GREETING_INTENT:
                            await dc.begin(GREETING_DIALOG);
                            break;

                        case NONE_INTENT:
                        default:
                            // help or no intent identified, either way, let's provide some help
                            // to the user
                            await dc.context.sendActivity(`I didn't understand what you just said to me. topIntent ${topIntent}`);
                            break;
                    }

                case DialogTurnStatus.waiting:
                    // The active dialog is waiting for a response from the user, so do nothing
                break;

                case DialogTurnStatus.complete:
                    await dc.end();
                    break;

                default:
                    await dc.cancelAll();
                    break;

            }

        } else if (context.activity.type === 'conversationUpdate' && context.activity.membersAdded[0].name === 'Bot') {
            // When activity type is "conversationUpdate" and the member joining the conversation is the bot
            // we will send our Welcome Adaptive Card.  This will only be sent once, when the Bot joins conversation
            // To learn more about Adaptive Cards, see https://aka.ms/msbot-adaptivecards for more details.
            const welcomeCard = CardFactory.adaptiveCard(WelcomeCard);
            await context.sendActivity({ attachments: [welcomeCard] });
        }
    }
    ```

    Robot kullanıcının konuşmasını LUIS'e gönderir ve sonuçları alır. Konuşmanın akışını ilk amaç belirler. 


## <a name="start-the-bot"></a>Robotu başlatma
Herhangi bir kodu veya ayarı değiştirmeden önce robotun çalıştığından emin olun. 

1. Visual Studio Code'da bir terminal penceresi açın. 

2. Bu robot için npm bağımlılıklarını yükleyin. 

    ```bash
    npm install
    ```
3. Robot kodunun aradığı ortam değişkenlerini barındıracak bir dosya oluşturun. Dosyayı `.env` olarak adlandırın. Aşağıdaki ortam değişkenlerini ekleyin:

    <!--there is no code language that represents an .env file correctly-->
    ```env
    botFilePath=
    botFileSecret=
    ```

    Ortam değişkenlerinin değerlerini **[Web uygulaması robotunu indirme](#download-the-web-app-bot)** bölümünün 1. Adımında Azure robot hizmetinin Uygulama Ayarları'ndan kopyaladığınız değerlere ayarlayın.

4. Robotu izleme modunda başlatın. Bu başlatma sonrasında kodda yaptığınız tüm değişiklikler uygulamanın otomatik olarak yeniden başlatılmasına neden olur.

    ```bash
    npm run watch
    ```

5. Robot başlatıldığında, terminal penceresinde robotun üzerinde çalıştırıldığı yerel bağlantı noktası görüntülenir:

    ```console
    > basic-bot@0.1.0 start C:\Users\pattiowens\repos\BFv4\luis-nodejs-bot-src
    > node ./index.js NODE_ENV=development

    restify listening to http://[::]:3978
    
    Get the Emulator: https://aka.ms/botframework-emulator
    
    To talk to your bot, open the luis-nodejs-bot-pattiowens.bot file in the Emulator
    ```

## <a name="start-the-emulator"></a>Öykünücüyü başlatma

1. Bot Emulator'ı başlatın. 

2. Robot öykünücüsünde, projenin kökündeki *.bot dosyasını seçin. Bu `.bot` dosyası robotun iletiler için URL uç noktasını içerir:

    [![Bot öykünücü v4](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png)](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png#lightbox)

3. **[Web uygulaması robotunu indirme](#download-the-web-app-bot)** bölümünün 1. Adımında Azure robot hizmetinin Uygulama Ayarları'ndan kopyaladığınız robot gizli dizisini girin. Bu, öykünücünün .bot dosyasındaki şifreli alanlara erişmesine izin verir.

    ![Bot emulator gizli dizisi v4](../../../includes/media/cognitive-services-luis/bfv4/bot-secret.png)


4. Robot öykünücüsünde `Hello` ifadesini girin ve temel robot için uygun yanıtı alın.

    [![Temel robot yanıt öykünücüsü](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png)](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png#lightbox)

## <a name="modify-bot-code"></a>Robot kodunu değiştirme 

`bot.js` dosyasına yeni amaçları işleyecek kod ekleyin. 

1. Dosyanın en üstünde, **Desteklenen LUIS Amaçları** bölümünü bulun ve HomeAutomation amaçları için sabitler ekleyin:

   ```javascript
    // Supported LUIS Intents
    const GREETING_INTENT = 'Greeting';
    const CANCEL_INTENT = 'Cancel';
    const HELP_INTENT = 'Help';
    const NONE_INTENT = 'None';
    const TURNON_INTENT = 'HomeAutomation_TurnOn'; // new intent
    const TURNOFF_INTENT = 'HomeAutomation_TurnOff'; // new intent
    ```

    Etki alanı ile LUIS portalının uygulamasından alınan amaç arasındaki `.` karakterinin bir `_` karakteri ile değiştirildiğine dikkat edin. 

2. Konuşmanın LUIS tahminini alan **isTurnInterrupted** öğesini bulun ve konsola sonucun çıkışını almak için bir satır ekleyin.

   ```javascript
    /**
     * Look at the LUIS results and determine if we need to handle
     * an interruptions due to a Help or Cancel intent
     *
     * @param {DialogContext} dc - dialog context
     * @param {LuisResults} luisResults - LUIS recognizer results
     */
    async isTurnInterrupted(dc, luisResults) {
        console.log(JSON.stringify(luisResults));
    ...
    ```

    Robot, bir LUIS REST API isteği ile tam olarak aynı yanıtı vermez, bu nedenle yanıt JSON dosyasına bakarak farkları öğrenmek önemlidir. Burada text (metin) ve intents (amaçlar) özellikleri aynıdır, ancak entities (varlıklar) özelliğinin değerleri değiştirilmiştir. 

    ```json
    {
        "$instance": {
            "HomeAutomation_Device": [
                {
                    "startIndex": 23,
                    "endIndex": 29,
                    "score": 0.9776345,
                    "text": "lights",
                    "type": "HomeAutomation.Device"
                }
            ],
            "HomeAutomation_Room": [
                {
                    "startIndex": 12,
                    "endIndex": 22,
                    "score": 0.9079433,
                    "text": "livingroom",
                    "type": "HomeAutomation.Room"
                }
            ]
        },
        "HomeAutomation_Device": [
            "lights"
        ],
        "HomeAutomation_Room": [
            "livingroom"
        ]
    }
    ```

3. `DialogTurnStatus.empty` durumu için onTurn yönteminin switch deyimine amaçları ekleyin:

   ```javascript
    switch (topIntent) {
        case GREETING_INTENT:
            await dc.begin(GREETING_DIALOG);
            break;

        // New HomeAutomation.TurnOn intent
        case TURNON_INTENT: 

            await dc.context.sendActivity(`TurnOn intent found, entities included: ${JSON.stringify(results.entities)}`);
            break;

        // New HomeAutomation.TurnOff intent
        case TURNOFF_INTENT: 

            await dc.context.sendActivity(`TurnOff intent found, entities included: ${JSON.stringify(results.entities)}`);
            break;

        case NONE_INTENT:
        default:
            // help or no intent identified, either way, let's provide some help
            // to the user
            await dc.context.sendActivity(`I didn't understand what you just said to me. topIntent ${topIntent}`);
            break;
    }
    ```

## <a name="view-results-in-bot"></a>Sonuçları robotta görüntüleme

1. Robot öykünücüsünde, konuşmayı girin: `Turn on the livingroom lights to 50%`

2. Robot şu yanıtı verir:

    ```json
    TurnOn intent found, entities included: {"$instance":{“HomeAutomation_Device”:[{“startIndex”:23,“endIndex”:29,“score”:0.9776345,“text”:“lights”,“type”:“HomeAutomation.Device”}],“HomeAutomation_Room”:[{“startIndex”:12,“endIndex”:22,“score”:0.9079433,“text”:“livingroom”,“type”:“HomeAutomation.Room”}]},“HomeAutomation_Device”:[“lights”],“HomeAutomation_Room”:[“livingroom”]}
    ```

## <a name="learn-more-about-bot-framework"></a>Bot Framework hakkında daha fazla bilgi edinin
Azure Bot hizmeti, Bot Framework SDK'sını kullanır. SDK ve bot çerçevesi hakkında daha fazla bilgi edinin:

* [Azure Bot Hizmeti](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0) v4 belgeleri
* [Bot Builder Örnekleri](https://github.com/Microsoft/botbuilder-samples)
* [Bot Builder SDK'sı](https://docs.microsoft.com/javascript/api/botbuilder-core/?view=botbuilder-ts-latest)
* [Bot Builder araçları](https://github.com/Microsoft/botbuilder-tools):

## <a name="next-steps"></a>Sonraki adımlar

Azure robot hizmeti oluşturdunuz, robot gizli dizisini ve .bot dosyasının yolunu kopyaladınız, kodun zip dosyasını indirdiniz. Önceden oluşturulmuş HomeAutomation etki alanını yeni Azure robot hizmeti kapsamında oluşturulan LUIS uygulamasına eklediniz, sonra da uygulamayı yeniden eğittiniz ve yayımladınız. Kod projesini ayıkladınız, ortam dosyası (`.env`) oluşturdunuz ve robot gizli dizisiyle .bot dosyasının yolunu ayarladınız. Bot.js dosyasında, iki yeni amacı işleyecek kodu eklediniz. Ardından yeni amaçlardan birinin konuşmasına LUIS yanıtını görmek için robot öykünücüsünde robotu test ettiniz. 


> [!div class="nextstepaction"]
> [LUIS'de özel etki alanı oluşturma](luis-quickstart-intents-only.md)
