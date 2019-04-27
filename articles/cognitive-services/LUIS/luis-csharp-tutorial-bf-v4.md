---
title: Bot - C# - v4
titleSuffix: Language Understanding - Azure Cognitive Services
description: C# kullanarak, dil anlama (LUIS) ile tümleşik bir sohbet robotu oluşturun. Bu sohbet robotu, bir robot çözümünü kısa sürede gerçekleştirmek için İnsan Kaynakları uygulamasını kullanır. Robot, Bot Framework 4 sürümü ve Azure Web uygulaması robotu ile geliştirilmiştir.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: 028c06924e41606ba1d4e0b15fe26f2b7270db3c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60710140"
---
# <a name="tutorial-luis-bot-in-c-with-the-bot-framework-4x-and-the-azure-web-app-bot"></a>Öğretici: LUIS bot içinde C# ile Bot Framework 4.x ve Azure Web app botu
C# kullanarak, dil anlama (LUIS) ile tümleşik bir sohbet robotu geliştirebilirsiniz. Bu robot, bir robot çözümünü gerçekleştirmek için HomeAutomation uygulamasını kullanır. Robot, [Bot Framework sürümü](https://github.com/Microsoft/botbuilder-js) v4 ile Azure [Web uygulaması robotu](https://docs.microsoft.com/azure/bot-service/) kullanılarak geliştirilmiştir.

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
* [Visual Studio](https://visualstudio.microsoft.com/downloads/)


## <a name="create-web-app-bot"></a>Web uygulaması robotu oluşturma

1. [Azure portalda](https://portal.azure.com) **Yeni kaynak oluştur**'u seçin.

2. Arama kutusunda **Web Uygulaması Robotu**'nu arayın ve bunu seçin. **Oluştur**’u seçin.

3. **Robot Hizmeti**'nde gerekli bilgileri sağlayın:

    |Ayar|Amaç|Önerilen ayar|
    |--|--|--|
    |Robot adı|Kaynak adı|`luis-csharp-bot-` + `<your-name>`, örneğin `luis-csharp-bot-johnsmith`|
    |Abonelik|Robotun oluşturulacağı abonelik.|Birincil aboneliğiniz.
    |Kaynak grubu|Azure kaynaklarının mantıksal grubu|Bu robotla kullanılan tüm kaynakları depolamak için yeni bir grup oluşturun ve grubu `luis-csharp-bot-resource-group` olarak adlandırın.|
    |Konum|Azure bölgesi - LUIS yazma veya yayımlama bölgesi ile aynı olması gerekmez.|`westus`|
    |Fiyatlandırma katmanı|Hizmet isteği sınırları ve faturalandırma için kullanılır.|`F0` ücretsiz katmanı gösterir.
    |Uygulama adı|Ad, robotunuz buluta dağıtıldığında alt etki alanı olarak kullanılır (örneğin humanresourcesbot.azurewebsites.net).|`luis-csharp-bot-` + `<your-name>`, örneğin `luis-csharp-bot-johnsmith`|
    |Robot şablonu|Bot Framework ayarları - sonraki tabloya bakın|
    |LUIS Uygulaması konumu|LUIS kaynak bölgesi ile aynı olmalıdır|`westus`|

4. **Robot şablon ayarları**'nda aşağıdakileri seçin ve sonra da bu ayarların altındaki **Seç** düğmesini seçin:

    |Ayar|Amaç|Seçim|
    |--|--|--|
    |SDK sürümü|Robot Framework sürümü|**SDK v4**|
    |SDK dili|Robotun programlama dili|**C#**|
    |Yankı/Temel robot|Robot türü|**Temel robot**|
    
5. **Oluştur**’u seçin. Robot hizmetini oluşturur ve Azure'a dağıtır. Bu işlemin bir parçası olarak `luis-csharp-bot-XXXX` adlı bir LUIS uygulaması oluşturulur. Buradaki ad, önceki bölümdeki robot ve uygulama adlarını temel alır.

    [![Web app botu oluşturun](./media/bfv4-csharp/create-web-app-service.png)](./media/bfv4-csharp/create-web-app-service.png#lightbox)

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
2. **Uygulamalarım** sayfasında, uygulamaları oluşturma tarihine göre sıralamak için **Oluşturma tarihi** sütununu seçin. Azure Robot hizmeti önceki bölümde yeni bir uygulama oluşturmuştu. Adı `luis-csharp-bot-` + `<your-name>` + 4 rasgele karakterdir.
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

    Robot kullanıcının konuşmasını LUIS'e gönderir ve sonuçları alır. Konuşmanın akışını ilk amaç belirler. 


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

    [![Bot öykünücü v4](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png)](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png#lightbox)

3. **[Web uygulaması robotunu indirme](#download-the-web-app-bot)** bölümünün 1. Adımında Azure robot hizmetinin Uygulama Ayarları'ndan kopyaladığınız robot gizli dizisini girin. Bu, öykünücünün `.bot` dosyasındaki şifreli dosyalara erişmesine izin verir.

    ![Bot emulator secret v4](../../../includes/media/cognitive-services-luis/bfv4/bot-secret.png)

4. Robot öykünücüsünde `Hello` ifadesini girin ve temel robot için uygun yanıtı alın.

    [![Temel robot yanıt öykünücüsü](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png)](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png#lightbox)

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
    ```    

## <a name="learn-more-about-bot-framework"></a>Bot Framework hakkında daha fazla bilgi edinin
Azure Bot hizmeti, Bot Framework SDK'sını kullanır. SDK ve bot çerçevesi hakkında daha fazla bilgi edinin:

* [Azure Bot Hizmeti](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0) v4 belgeleri
* [Bot Builder Örnekleri](https://github.com/Microsoft/botbuilder-samples)
* [Bot Builder SDK'sı](https://docs.microsoft.com/javascript/api/botbuilder-core/?view=botbuilder-ts-latest)
* [Bot Builder araçları](https://github.com/Microsoft/botbuilder-tools):

## <a name="next-steps"></a>Sonraki adımlar

Azure bot hizmeti oluşturdunuz, robot gizli dizisini ve `.bot` dosyasının yolunu kopyaladınız, kodun zip dosyasını indirdiniz. Önceden oluşturulmuş HomeAutomation etki alanını yeni Azure robot hizmeti kapsamında oluşturulan LUIS uygulamasına eklediniz, sonra da uygulamayı yeniden eğittiniz ve yayımladınız. Kod projesini ayıkladınız, ortam dosyası (`.env`) oluşturdunuz ve bot gizli dizisiyle `.bot` dosyasının yolunu ayarladınız. Bot.js dosyasında, iki yeni amacı işleyecek kodu eklediniz. Ardından yeni amaçlardan birinin konuşmasına LUIS yanıtını görmek için robot öykünücüsünde robotu test ettiniz. 

Daha fazla bilgi bkz [örnekleri](https://github.com/Microsoft/AI) damıtarak konuşma bağlamında kullanılabilen bot ile. 

> [!div class="nextstepaction"]
> [LUIS'de özel etki alanı oluşturma](luis-quickstart-intents-only.md)
