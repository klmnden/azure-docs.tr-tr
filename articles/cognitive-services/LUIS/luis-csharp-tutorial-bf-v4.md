---
title: C# LUIS Robotu - Öğretici - Web uygulaması Robotu - Bot Framework SDK 4.0
titleSuffix: Azure Cognitive Services
description: C# kullanarak, dil anlama (LUIS) ile tümleşik bir sohbet robotu oluşturun. Bu sohbet robotu, bir robot çözümünü kısa sürede gerçekleştirmek için İnsan Kaynakları uygulamasını kullanır. Robot, Bot Framework sürümü 4 ve Azure Web uygulaması robotu ile geliştirilmiştir.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/25/2018
ms.author: diberry
ms.openlocfilehash: f8350d46fecff726dd9f591fe3df0272f556b3e7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47168222"
---
# <a name="tutorial-luis-bot-in-c"></a>Öğretici: C# dilinde LUIS robotu
C# kullanarak, dil anlama (LUIS) ile tümleşik bir sohbet robotu geliştirebilirsiniz. Bu robot, bir robot çözümünü gerçekleştirmek için HomeAutomation uygulamasını kullanır. Robot, [Bot Framework sürümü](https://github.com/Microsoft/botbuilder-js) v4 ile Azure [Web uygulaması robotu](https://docs.microsoft.com/azure/bot-service/) kullanılarak geliştirilmiştir.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

> [!div class="checklist"]
> * Bir Web uygulaması oluşturun. Bu işlem, sizin için yeni bir LUIS uygulaması oluşturur.
> * Yeni LUIS modeline önceden oluşturulmuş bir etki alanı ekleme
> * Web robot hizmeti tarafından oluşturulan projeyi indirme
> * Robotu ve öykünücüyü bilgisayarınızda yerel olarak başlatma
> * Robot kodunu yeni LUIS amaçları için değiştirme
> * Robotta konuşma sonuçlarını görüntüleme

## <a name="prerequisites"></a>Ön koşullar

* [Robot öykünücüsü](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/)


## <a name="create-web-app-bot"></a>Web uygulaması robotu oluşturma

1. [Azure portalda](https://portal.azure.com) **Yeni kaynak oluştur**'u seçin.

2. Arama kutusunda **Web Uygulaması Robotu**'nu arayıp seçin. **Oluştur**’u seçin.

3. **Robot Hizmeti**'nde gerekli bilgileri sağlayın:

    |Ayar|Amaç|Önerilen ayar|
    |--|--|--|
    |Robot adı|Kaynak adı|`luis-csharp-bot-` + `<your-name>`; örneğin `luis-csharp-bot-johnsmith`|
    |Abonelik|Robotun oluşturulacağı abonelik.|Birincil aboneliğiniz.
    |Kaynak grubu|Azure kaynaklarının mantıksal grubu|Bu robot ile kullanılan tüm kaynakları depolamak için yeni bir grup oluşturun ve grubu `luis-csharp-bot-resource-group` olarak adlandırın.|
    |Konum|Azure bölgesi - LUIS yazma veya yayımlama bölgesi ile aynı olması gerekmez.|`westus`|
    |Fiyatlandırma katmanı|Hizmet isteği sınırları ve faturalandırma için kullanılır.|`F0` ücretsiz katmanı gösterir.
    |Uygulama adı|Ad, robotunuz buluta dağıtıldığında alt etki alanı olarak kullanılır (örneğin humanresourcesbot.azurewebsites.net).|`luis-csharp-bot-` + `<your-name>`; örneğin `luis-csharp-bot-johnsmith`|
    |Robot şablonu|Bot Framework ayarları - sonraki tabloya bakın|
    |LUIS Uygulaması konumu|LUIS kaynak bölgesi ile aynı olmalıdır|`westus`|

4. **Robot şablon ayarları**'nda aşağıdakileri, ardından bu ayarların altındaki **Seç** düğmesini seçin:

    |Ayar|Amaç|Seçim|
    |--|--|--|
    |SDK sürümü|Robot Framework sürümü|**SDK v4**|
    |SDK dili|Robotun programlama dili|**C#**|
    |Yankı/Temel robot|Robot türü|**Temel robot**|
    
5. **Oluştur**’u seçin. Robot hizmetini oluşturur ve Azure'a dağıtır. Bu işlemin bir parçası olarak `luis-csharp-bot-XXXX` adlı bir LUIS uygulaması oluşturulur. Buradaki ad, önceki bölümdeki robot ve uygulama adlarını temel alır.

    [ ![Web uygulaması robotu oluşturma](./media/bfv4-csharp/create-web-app-service.png) ](./media/bfv4-csharp/create-web-app-service.png#lightbox)

6. Bu tarayıcı sekmesini açık bırakın. LUIS portalındaki bir adım için yeni bir tarayıcı sekmesi açın. Yeni robot hizmet dağıtıldığında sonraki bölüme devam edin.

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
2. **Uygulamalarım** sayfasında, uygulamaları oluşturma tarihine göre sıralamak için **Oluşturma tarihi** sütununu seçin. Azure Robot hizmeti önceki bölümde yeni bir uygulama oluşturmuştu. Adı `luis-csharp-bot-` + `<your-name>` + 4 rasgele karakterdir.
3. Uygulamayı açın ve üst gezinti bölmesinde **Derle** bölümünü seçin.
4. Sol gezinti bölmesinde **Önceden Oluşturulmuş Etki Alanları**'nı seçin.
5. **HomeAutomation** etki alanını kartının üzerindeki **Etki alanı ekle**'yi seçerek seçin.
6. Sağ üst menüde **Eğit**'i seçin.
7. Sağ üst menüde **Yayımla**'yı seçin. 

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

    [ ![Temel robot için Web uygulaması robot kaynak kodunu indirme](../../../includes/media/cognitive-services-luis/bfv4/download-code.png) ](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

4. Kaynak kodu ZIP olarak sıkıştırılmışsa, bir ileti kodu indirmek için bir bağlantı sağlar. Bağlantıyı seçin. 

5. ZIP dosyasını yerel bilgisayarınıza kaydedin ve dosyaları çıkarın. Projeyi açın. 

6. Bot.cs dosyasını açın ve `_services.LuisServices` ifadesini arayın. Robota girilen kullanıcı konuşmasının LUIS'e gönderildiği yer budur.

    ```csharp
    /// <summary>
    /// Run every turn of the conversation. Handles orchestration of messages.
    /// </summary>
    /// <param name="turnContext">Bot Turn Context.</param>
    /// <param name="cancellationToken">Task CancellationToken.</param>
    /// <returns>A <see cref="Task"/> representing the asynchronous operation.</returns>
    public async Task OnTurnAsync(ITurnContext turnContext, CancellationToken cancellationToken)
    {
        var activity = turnContext.Activity;

        if (activity.Type == ActivityTypes.Message)
        {
            // Perform a call to LUIS to retrieve results for the current activity message.
            var luisResults = await _services.LuisServices[LuisConfiguration].RecognizeAsync(turnContext, cancellationToken).ConfigureAwait(false);

            // If any entities were updated, treat as interruption.
            // For example, "no my name is tony" will manifest as an update of the name to be "tony".
            var topScoringIntent = luisResults?.GetTopScoringIntent();

            var topIntent = topScoringIntent.Value.intent;
            switch (topIntent)
            {
                case GreetingIntent:
                    await turnContext.SendActivityAsync("Hello.");
                    break;
                case HelpIntent:
                    await turnContext.SendActivityAsync("Let me try to provide some help.");
                    await turnContext.SendActivityAsync("I understand greetings, being asked for help, or being asked to cancel what I am doing.");
                    break;
                case CancelIntent:
                    await turnContext.SendActivityAsync("I have nothing to cancel.");
                    break;
                case NoneIntent:
                default:
                    // Help or no intent identified, either way, let's provide some help.
                    // to the user
                    await turnContext.SendActivityAsync("I didn't understand what you just said to me.");
                    break;
            }
        }
        else if (activity.Type == ActivityTypes.ConversationUpdate)
        {
            if (activity.MembersAdded.Any())
            {
                // Iterate over all new members added to the conversation.
                foreach (var member in activity.MembersAdded)
                {
                    // Greet anyone that was not the target (recipient) of this message.
                    // To learn more about Adaptive Cards, see https://aka.ms/msbot-adaptivecards for more details.
                    if (member.Id != activity.Recipient.Id)
                    {
                        var welcomeCard = CreateAdaptiveCardAttachment();
                        var response = CreateResponse(activity, welcomeCard);
                        await turnContext.SendActivityAsync(response).ConfigureAwait(false);
                    }
                }
            }
        }

    }
    ```

    Robot kullanıcının konuşmasını LUIS'e gönderir ve sonuçları alır. Söyleşinin akışını ilk amaç belirler. 


## <a name="start-the-bot"></a>Robotu başlatma
Herhangi bir kodu veya ayarı değiştirmeden önce robotun çalıştığından emin olun. 

1. Visual Studio'da çözüm dosyasını açın. 

2. Robot kodunun aradığı robot değişkenlerinin duracağı bir `appsettings.json` dosyası oluşturun:

    ```JSON
    {
    "botFileSecret": "",
    "botFilePath": ""

    }
    ```

    Değişkenlerin değerlerini **[Web uygulaması robotunu indirme](#download-the-web-app-bot)** bölümünün 1. Adımında Azure robot hizmetinin Uygulama Ayarları'ndan kopyaladığınız değerlere ayarlayın.

3. Visual Studio'da robotu başlatın. Web uygulaması robotunun `http://localhost:3978/` adresindeki sitesi ile bir tarayıcı penceresi açılır.

## <a name="start-the-emulator"></a>Öykünücüyü başlatma

1. Bot Emulator'ı başlatın.

2. Robot öykünücüsünde, projenin kökündeki *.bot dosyasını seçin. Bu `.bot` dosyası robotun iletiler için URL uç noktasını içerir:

    [ ![Bot emulator v4](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png) ](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png#lightbox)

3. **[Web uygulaması robotunu indirme](#download-the-web-app-bot)** bölümünün 1. Adımında Azure robot hizmetinin Uygulama Ayarları'ndan kopyaladığınız robot gizli dizesini girin. Bu, öykünücünün `.bot` dosyasındaki şifreli dosyalara erişmesine izin verir.

    ![Bot emulator secret v4](../../../includes/media/cognitive-services-luis/bfv4/bot-secret.png)

4. Robot öykünücüsünde `Hello` ifadesini girin ve temel robot için uygun yanıtı alın.

    [ ![Öykünücüde temel robot yanıtı](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png) ](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png#lightbox)

## <a name="modify-bot-code"></a>Robot kodunu değiştirme 

`BasicBot.cs` dosyasına yeni amaçları işleyecek kod ekleyin. 

1. Dosyanın en üstünde, **Desteklenen LUIS Amaçları** bölümünü bulun ve HomeAutomation amaçları için sabitler ekleyin:

    ```csharp
    // Supported LUIS Intents
    public const string GreetingIntent = "Greeting";
    public const string CancelIntent = "Cancel";
    public const string HelpIntent = "Help";
    public const string NoneIntent = "None";
    public const string TurnOnIntent = "HomeAutomation_TurnOn"; // new intent
    public const string TurnOffIntent = "HomeAutomation_TurnOff"; // new intent
    ```

    Etki alanı ile LUIS portalının uygulamasından alınan amaç arasındaki `.` karakterinin bir `_` karakteri ile değiştirildiğine dikkat edin. 

2. Konuşmanın LUIS tahminini alan **OnTurnAsync** yöntemini bulun. İki HomeAutomation amacının LUIS yanıtını döndürmek için switch deyimine kod ekleyin. 

    ```csharp
    case TurnOnIntent:
        await turnContext.SendActivityAsync("TurnOn intent found, JSON response: " + luisResults?.Entities.ToString());
        break;
    case TurnOffIntent:
        await turnContext.SendActivityAsync("TurnOff intent found, JSON response: " + luisResults?.Entities.ToString());
        break;
    ```

    Robot, bir LUIS REST API isteği ile tam olarak aynı yanıtı vermez, bu nedenle yanıt JSON dosyasına bakarak farkları öğrenmek önemlidir. Burada text (metin) ve intents (amaçlar) özellikleri aynıdır, ancak entities (varlıklar) özelliğinin değerleri değiştirilmiştir. 

    ```JSON
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



## <a name="view-results-in-bot"></a>Sonuçları robotta görüntüleme

1. Robot öykünücüsünde, konuşmayı girin: `Turn on the livingroom lights to 50%`

2. Robot şu yanıtı verir:

    ```JSON
    TurnOn intent found, JSON response: {"$instance":{“HomeAutomation_Device”:[{“startIndex”:23,“endIndex”:29,“score”:0.9776345,“text”:“lights”,“type”:“HomeAutomation.Device”}],“HomeAutomation_Room”:[{“startIndex”:12,“endIndex”:22,“score”:0.9079433,“text”:“livingroom”,“type”:“HomeAutomation.Room”}]},“HomeAutomation_Device”:[“lights”],“HomeAutomation_Room”:[“livingroom”]}
    ```    ```

## Learn more about Bot Framework
Azure Bot service uses the Bot Framework SDK. Learn more about the SDK and bot framework:

* [Azure Bot Service](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0) v4 documentation
* [Bot Builder Samples](https://github.com/Microsoft/botbuilder-samples)
* [Bot Builder SDK](https://docs.microsoft.com/en-us/javascript/api/botbuilder-core/?view=botbuilder-ts-latest)
* [Bot Builder tools](https://github.com/Microsoft/botbuilder-tools):

## Next steps

You created an Azure bot service, copied the bot secret and `.bot` file path, downloaded the zip file of the code. You added the prebuilt HomeAutomation domain to the LUIS app created as part of the new Azure bot service, then trained and published the app again. You extracted the code project, created an environment file (`.env`), and set the bot secret and the `.bot` file path. In the bot.js file, you added code to handle the two new intents. Then you tested the bot in the bot emulator to see the LUIS response for an utterance of one of the new intents. 


> [!div class="nextstepaction"]
> [Build a custom domain in LUIS](luis-quickstart-intents-only.md)
