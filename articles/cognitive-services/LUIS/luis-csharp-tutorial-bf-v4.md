---
title: Dil anlama Bot C# v4
titleSuffix: Language Understanding - Azure Cognitive Services
description: C# kullanarak, dil anlama (LUIS) ile tümleşik bir sohbet robotu oluşturun. Bot, Azure Web app botu hizmeti ile Bot Framework sürüm 4 ile oluşturulmuştur.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/17/2019
ms.author: diberry
ms.openlocfilehash: f74becc24e5d04cefdd05066b8431946578cc35e
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67151114"
---
# <a name="tutorial-use-a-web-app-bot-enabled-with-language-understanding-in-c"></a>Öğretici: Kullanım Language Understanding ile etkin bir Web App BotuC#

Kullanım C# dil anlama (LUIS) ile tümleşik bir sohbet Robotu oluşturulacak. Bot, Azure ile yerleşik [Web app botu](https://docs.microsoft.com/azure/bot-service/) kaynak ve [Bot Framework sürümü](https://github.com/Microsoft/botbuilder-dotnet) V4.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Web uygulaması robotu oluşturma. Bu işlem sizin için yeni bir LUIS uygulaması oluşturur.
> * Web bot hizmeti tarafından oluşturulan robot projesini indirin
> * Robotu ve öykünücüyü bilgisayarınızda yerel olarak başlatma
> * Robotta konuşma sonuçlarını görüntüleme

## <a name="prerequisites"></a>Önkoşullar

* [Robot öykünücüsü](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/)


## <a name="create-a-web-app-bot-resource"></a>Bir web app botu kaynak oluştur

1. [Azure portalda](https://portal.azure.com) **Yeni kaynak oluştur**'u seçin.

1. Arama kutusunda **Web Uygulaması Robotu**'nu arayın ve bunu seçin. **Oluştur**’u seçin.

1. **Robot Hizmeti**'nde gerekli bilgileri sağlayın:

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
    |App service planı/konumu|Sağlanan varsayılan değeri değiştirmeyin.|
    |Application Insights|Sağlanan varsayılan değeri değiştirmeyin.|
    |Microsoft uygulama kimliği ve parola|Sağlanan varsayılan değeri değiştirmeyin.|

1. İçinde **Bot şablon**, aşağıdakileri seçin ve sonra seçin **seçin** düğmesi bu ayarlar altında:

    |Ayar|Amaç|Seçim|
    |--|--|--|
    |SDK sürümü|Robot Framework sürümü|**SDK v4**|
    |SDK dili|Robotun programlama dili|**C#**|
    |Bot|Robot türü|**Temel robot**|
    
1. **Oluştur**’u seçin. Robot hizmetini oluşturur ve Azure'a dağıtır. Bu işlemin bir parçası olarak `luis-csharp-bot-XXXX` adlı bir LUIS uygulaması oluşturulur. Bu ad /Azure Bot hizmeti uygulaması adınıza temel alır.

    [![Web app botu oluşturun](./media/bfv4-csharp/create-web-app-service.png)](./media/bfv4-csharp/create-web-app-service.png#lightbox)

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

    ![Azure portal ekran görüntüsü, 'hello' metni girin.](./media/bfv4-csharp/ask-bot-question-in-portal-test-in-web-chat.png)

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

1. Açık **LuisHelper.cs** dosya. Robota girilen kullanıcı konuşmasının LUIS'e gönderildiği yer budur. LUIS gelen yanıt yönteminden döndürülen bir **BookDetails** nesne. Kendi bot oluşturduğunuzda, ayrıca kendi nesne LUIS ayrıntılarını döndürecek şekilde oluşturmanız gerekir. 


    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    using System;
    using System.Linq;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.AI.Luis;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;
    
    namespace Microsoft.BotBuilderSamples
    {
        public static class LuisHelper
        {
            public static async Task<BookingDetails> ExecuteLuisQuery(IConfiguration configuration, ILogger logger, ITurnContext turnContext, CancellationToken cancellationToken)
            {
                var bookingDetails = new BookingDetails();
    
                try
                {
                    // Create the LUIS settings from configuration.
                    var luisApplication = new LuisApplication(
                        configuration["LuisAppId"],
                        configuration["LuisAPIKey"],
                        "https://" + configuration["LuisAPIHostName"]
                    );
    
                    var recognizer = new LuisRecognizer(luisApplication);
    
                    // The actual call to LUIS
                    var recognizerResult = await recognizer.RecognizeAsync(turnContext, cancellationToken);
    
                    var (intent, score) = recognizerResult.GetTopScoringIntent();
                    if (intent == "Book_flight")
                    {
                        // We need to get the result from the LUIS JSON which at every level returns an array.
                        bookingDetails.Destination = recognizerResult.Entities["To"]?.FirstOrDefault()?["Airport"]?.FirstOrDefault()?.FirstOrDefault()?.ToString();
                        bookingDetails.Origin = recognizerResult.Entities["From"]?.FirstOrDefault()?["Airport"]?.FirstOrDefault()?.FirstOrDefault()?.ToString();
    
                        // This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
                        // TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
                        bookingDetails.TravelDate = recognizerResult.Entities["datetime"]?.FirstOrDefault()?["timex"]?.FirstOrDefault()?.ToString().Split('T')[0];
                    }
                }
                catch (Exception e)
                {
                    logger.LogWarning($"LUIS Exception: {e.Message} Check your LUIS configuration.");
                }
    
                return bookingDetails;
            }
        }
    }
    ```

1. Açık **BookingDetails.cs** nesne LUIS bilgileri nasıl soyutlar görüntülemek için. 

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    namespace Microsoft.BotBuilderSamples
    {
        public class BookingDetails
        {
            public string Destination { get; set; }
    
            public string Origin { get; set; }
    
            public string TravelDate { get; set; }
        }
    }
    ```

1. Açık **iletişim kutuları -> BookingDialog.cs** BookingDetails nesne konuşma akışını yönetmek için nasıl kullanıldığını öğrenin. Seyahat ayrıntıları adımlarda istenir, ardından tüm kayıt Onaylandı ve son kullanıcıya geri yinelenir. 

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Recognizers.Text.DataTypes.TimexExpression;
    
    namespace Microsoft.BotBuilderSamples.Dialogs
    {
        public class BookingDialog : CancelAndHelpDialog
        {
            public BookingDialog()
                : base(nameof(BookingDialog))
            {
                AddDialog(new TextPrompt(nameof(TextPrompt)));
                AddDialog(new ConfirmPrompt(nameof(ConfirmPrompt)));
                AddDialog(new DateResolverDialog());
                AddDialog(new WaterfallDialog(nameof(WaterfallDialog), new WaterfallStep[]
                {
                    DestinationStepAsync,
                    OriginStepAsync,
                    TravelDateStepAsync,
                    ConfirmStepAsync,
                    FinalStepAsync,
                }));
    
                // The initial child Dialog to run.
                InitialDialogId = nameof(WaterfallDialog);
            }
    
            private async Task<DialogTurnResult> DestinationStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                if (bookingDetails.Destination == null)
                {
                    return await stepContext.PromptAsync(nameof(TextPrompt), new PromptOptions { Prompt = MessageFactory.Text("Where would you like to travel to?") }, cancellationToken);
                }
                else
                {
                    return await stepContext.NextAsync(bookingDetails.Destination, cancellationToken);
                }
            }
    
            private async Task<DialogTurnResult> OriginStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                bookingDetails.Destination = (string)stepContext.Result;
    
                if (bookingDetails.Origin == null)
                {
                    return await stepContext.PromptAsync(nameof(TextPrompt), new PromptOptions { Prompt = MessageFactory.Text("Where are you traveling from?") }, cancellationToken);
                }
                else
                {
                    return await stepContext.NextAsync(bookingDetails.Origin, cancellationToken);
                }
            }
            private async Task<DialogTurnResult> TravelDateStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                bookingDetails.Origin = (string)stepContext.Result;
    
                if (bookingDetails.TravelDate == null || IsAmbiguous(bookingDetails.TravelDate))
                {
                    return await stepContext.BeginDialogAsync(nameof(DateResolverDialog), bookingDetails.TravelDate, cancellationToken);
                }
                else
                {
                    return await stepContext.NextAsync(bookingDetails.TravelDate, cancellationToken);
                }
            }
    
            private async Task<DialogTurnResult> ConfirmStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                bookingDetails.TravelDate = (string)stepContext.Result;
    
                var msg = $"Please confirm, I have you traveling to: {bookingDetails.Destination} from: {bookingDetails.Origin} on: {bookingDetails.TravelDate}";
    
                return await stepContext.PromptAsync(nameof(ConfirmPrompt), new PromptOptions { Prompt = MessageFactory.Text(msg) }, cancellationToken);
            }
    
            private async Task<DialogTurnResult> FinalStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                if ((bool)stepContext.Result)
                {
                    var bookingDetails = (BookingDetails)stepContext.Options;
    
                    return await stepContext.EndDialogAsync(bookingDetails, cancellationToken);
                }
                else
                {
                    return await stepContext.EndDialogAsync(null, cancellationToken);
                }
            }
    
            private static bool IsAmbiguous(string timex)
            {
                var timexProperty = new TimexProperty(timex);
                return !timexProperty.Types.Contains(Constants.TimexTypes.Definite);
            }
        }
    }
    ```


## <a name="start-the-bot-code-in-visual-studio"></a>Bot kodu Visual Studio'da başlatma

Visual Studio'da robotu başlatın. Web uygulaması robotunun `http://localhost:3978/` adresindeki sitesi ile bir tarayıcı penceresi açılır. Botunuzun hakkında bilgi içeren bir giriş sayfasında gösterilir.

![Botunuzun hakkında bilgi içeren bir giriş sayfasında gösterilir.](./media/bfv4-csharp/running-bot-web-home-page-success.png)

## <a name="use-the-bot-emulator-to-test-the-bot"></a>Bot test etmek için robot öykünücüyü kullanma

1. Bot öykünücü başlar ve seçin **açık Bot**.
1. İçinde **bir bot açın** açılır iletişim kutusu, robot URL'nizi girin `http://localhost:3978/api/messages`. `/api/messages` Bot web adresini yoldur.
1. Girin **Microsoft uygulama kimliği** ve **Microsoft App parola**bölümüyle **appsettings.json** kök indirdiğiniz bot kod dosyasında.

    İsteğe bağlı olarak, yapılandırma ve kopyalama yeni bir bot oluşturabilirsiniz `appId` ve `appPassword` gelen **appsettings.json** bot Visual Studio Proje dosyasında. Robot yapılandırma dosyasının adı bot adıyla aynı olmalıdır. 

    ```json
    {
        "name": "<bot name>",
        "description": "<bot description>",
        "services": [
            {
                "type": "endpoint",
                "appId": "<appId from appsettings.json>",
                "appPassword": "<appPassword from appsettings.json>",
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

    [![Temel robot yanıt öykünücüsü](./media/bfv4-csharp/ask-bot-emulator-a-question-and-get-response.png)](./media/bfv4-csharp/ask-bot-emulator-a-question-and-get-response.png#lightbox)


## <a name="ask-bot-a-question-for-the-book-flight-intent"></a>Kitap uçuş amaç için robot soru

1. Bot öykünücüsü'nde, aşağıdaki utterance girerek bir kitap: 

    ```bot
    Book a flight from Paris to Berlin on March 22, 2020
    ```

    Onaylamak robot öykünücü ister. 

1. Seçin **Evet**. Bot eylemlerinin özetini ile yanıt verir. 
1. Bot öykünücü günlüğünden içeren satırı seçin `Luis Trace`. Bu, luıs'den JSON yanıtı utterance varlıklarının ve hedefi için görüntüler.

    [![Temel robot yanıt öykünücüsü](./media/bfv4-csharp/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png)](./media/bfv4-csharp/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png#lightbox)

## <a name="learn-more-about-the-web-app-bot-and-framework"></a>Web App Botu ve framework hakkında daha fazla bilgi edinin

Azure Bot hizmeti, Bot Framework SDK'sını kullanır. SDK ve bot çerçevesi hakkında daha fazla bilgi edinin:

* [Azure Bot Hizmeti](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0) v4 belgeleri
* [Bot Builder Örnekleri](https://github.com/Microsoft/botbuilder-samples)
* [Bot Builder C# SDK'sı](https://github.com/Microsoft/botbuilder-dotnet)
* [Bot Builder araçları](https://github.com/Microsoft/botbuilder-tools):

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi bkz [örnekleri](https://github.com/microsoft/botframework-solutions) damıtarak konuşma bağlamında kullanılabilen bot ile. 

> [!div class="nextstepaction"]
> [Bir özel konu etki alanı ile bir konuşma tanıma uygulaması derleme](luis-quickstart-intents-only.md)
