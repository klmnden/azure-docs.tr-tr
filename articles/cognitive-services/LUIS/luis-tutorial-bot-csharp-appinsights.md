---
title: Application InsightsC#
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, Application Insights telemetri verileri depolama alanına bot ve Language Understanding bilgi ekler.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/16/2019
ms.author: diberry
ms.openlocfilehash: 720352403fd5f5937669f9838f3974cb0d3f8797
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657794"
---
# <a name="add-luis-results-to-application-insights-from-a-bot-in-c"></a>LUIS sonuçları bir Bot içinde Application Insights ekleyinC#

Bu öğreticide bot ve Language Understanding bilgileri ekler [Application Insights](https://azure.microsoft.com/services/application-insights/) telemetri veri depolama. Bu verileri aldıktan sonra bunu Kusto dil veya çözümlemek, toplama, Power BI ile sorgulayabilirsiniz ve hedefleri ve gerçek zamanlı utterance varlıklarının rapor. Bu analiz, eklediğinizde veya amaç ve varlıkları LUIS uygulamanızı düzenlemek, belirlemenize yardımcı olur.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bot ve dil Anlama veri Application ınsights'ta Yakala
> * Application Insights için Language Understanding verileri Sorgulama

## <a name="prerequisites"></a>Önkoşullar

* Application Insights ile oluşturulan bir Azure robot hizmeti Robotu.
* Bot kod önceki bot indirilen  **[öğretici](luis-csharp-tutorial-bf-v4.md)** . 
* [Robot öykünücüsü](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)

Bu öğreticideki kod tüm kullanılabilir [Azure-Samples dil anlama GitHub deposu](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/v4/luis-csharp-bot-johnsmith-src-telemetry). 

## <a name="add-application-insights-to-web-app-bot-project"></a>Application Insights web app botu projeye Ekle

Şu anda bu web app botu kullanılan Application Insights hizmeti için robot genel durumu telemetri toplar. LUIS bilgi toplamaz. 

LUIS bilgileri yakalamak için web app botu gereken **[Microsoft.applicationınsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/)** NuGet paketi yüklenir ve yapılandırılır.  

1. Visual Studio'da bağımlılık çözüme ekleyin. İçinde **Çözüm Gezgini**, proje adını sağ tıklatın ve seçin **NuGet paketlerini Yönet...** . NuGet Paket Yöneticisi yüklü paketler listesini gösterir. 
1. Seçin **Gözat** arayın **Microsoft.applicationınsights**.
1. Paketi yükleyin. 

## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Yakalama ve LUIS sorgu sonuçları Application Insights'a gönderme

1. Açık `LuisHelper.cs` dosyasını açıp içeriğini aşağıdaki kodla değiştirin. **LogToApplicationInsights** yöntemi bot ve LUIS veri yakalama ve adlı bir izleme olayı Application Insights'a gönderir `LUIS`.

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
    using Microsoft.ApplicationInsights;
    using System.Collections.Generic;
    
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
    
                    LuisHelper.LogToApplicationInsights(configuration, turnContext, recognizerResult);
    
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
            public static void LogToApplicationInsights(IConfiguration configuration, ITurnContext turnContext, RecognizerResult result)
            {
                // Create Application Insights object
                TelemetryClient telemetry = new TelemetryClient();
    
                // Set Application Insights Instrumentation Key from App Settings
                telemetry.InstrumentationKey = configuration["BotDevAppInsightsKey"];
    
                // Collect information to send to Application Insights
                Dictionary<string, string> logProperties = new Dictionary<string, string>();
    
                logProperties.Add("BotConversation", turnContext.Activity.Conversation.Name);
                logProperties.Add("Bot_userId", turnContext.Activity.Conversation.Id);
    
                logProperties.Add("LUIS_query", result.Text);
                logProperties.Add("LUIS_topScoringIntent_Name", result.GetTopScoringIntent().intent);
                logProperties.Add("LUIS_topScoringIntentScore", result.GetTopScoringIntent().score.ToString());
    
    
                // Add entities to collected information
                int i = 1;
                if (result.Entities.Count > 0)
                {
                    foreach (var item in result.Entities)
                    {
                        logProperties.Add("LUIS_entities_" + i++ + "_" + item.Key, item.Value.ToString());
                    }
                }
    
                // Send to Application Insights
                telemetry.TrackTrace("LUIS", ApplicationInsights.DataContracts.SeverityLevel.Information, logProperties);
    
            }
        }
    }
    ```

## <a name="add-application-insights-instrumentation-key"></a>Application Insights izleme anahtarı Ekle 

Application ınsights'a veri eklemek için izleme anahtarı gerekir.

1. Bir tarayıcıda, [Azure portalında](https://portal.azure.com), botunuzun ait bulma **Application Insights** kaynak. Adını botun adı çoğunu olacaktır ve rastgele adının sonuna gibi karakterler `luis-csharp-bot-johnsmithxqowom`. 
1. Application Insights kaynağı üzerinde **genel bakış** sayfasında, kopya **izleme anahtarını**.
1. Visual Studio'da açın **appsettings.json** bot projenin köküne dosya. Bu dosya, tüm ortam değişkenleri tutar.
1. Yeni bir değişken eklemek `BotDevAppInsightsKey` izleme anahtarınızı değerine sahip. Değer tırnak işareti olmalıdır. 

## <a name="build-and-start-the-bot"></a>Oluşturun ve bot başlatın

1. Visual Studio'da derleyin ve bot çalıştırın. 
1. Bot öykünücüyü başlatın ve bot açın. Bu [adım](luis-csharp-tutorial-bf-v4.md#use-the-bot-emulator-to-test-the-bot) önceki öğreticide sağlanır.

1. Bot, bir soru sorun. Bu [adım](luis-csharp-tutorial-bf-v4.md#ask-bot-a-question-for-the-book-flight-intent) önceki öğreticide sağlanır.

## <a name="view-luis-entries-in-application-insights"></a>Application ınsights'ta görünümü LUIS girişleri

LUIS girişlerini görmek için Application ınsights'ı açın. Bu verilerin Application Insights'da gösterilmesi birkaç dakika sürebilir.

1. İçinde [Azure portalında](https://portal.azure.com), botun Application Insights kaynağını açın. 
1. Kaynak açıldığında seçin **arama** ve son tüm veriler için arama **30 dakika** olay türü ile **izleme**. Adlı izlemenin seçin **LUIS**. 
1. Bot ve LUIS bilgileri altında kullanılabilir **özel özellikler**. 

    ![Uygulama anlayışları'nda depolanan LUIS özel özellikleri gözden geçirin](./media/luis-tutorial-appinsights/application-insights-luis-trace-custom-properties-csharp.png)

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Amacını ve score utterance için Application Insights sorgu
Application Insights ile verileri sorgulamak için power size [Kusto](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview#what-language-do-log-queries-use) dışarı aktarma yanı dil [Power BI](https://powerbi.microsoft.com). 

1. Seçin **günlük (analiz)** . Bir sorgu penceresi üstündeki ve bir veri tablosu penceresi altındaki yeni bir pencere açılır. Veritabanları önce kullandıysanız, bu düzenleme tanıdık gelir. Sorgu, önceki filtrelenmiş verileri temsil eder. **CustomDimensions** sütununun bot ve LUIS bilgileri.
1. Üst amacı, Puanlama ve utterance çıkarmak için aşağıdaki yalnızca son satırı ekleyin ( `|top...` satır) sorgu penceresinde:

    ```kusto
    | extend topIntent = tostring(customDimensions.LUIS_topScoringIntent_Name)
    | extend score = todouble(customDimensions.LUIS_topScoringIntentScore)
    | extend utterance = tostring(customDimensions.LUIS_query)
    ```

1. Sorguyu çalıştırın. Yeni sütunlar topIntent, Puanlama ve utterance kullanılabilir. Sıralanacak topIntent sütun seçin.

Daha fazla bilgi edinin [Kusto sorgu dili](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) veya [verileri Power BI'a aktarma](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 


## <a name="learn-more-about-bot-framework"></a>Bot Framework hakkında daha fazla bilgi edinin

Daha fazla bilgi edinin [Bot Framework](https://dev.botframework.com/).

## <a name="next-steps"></a>Sonraki adımlar

Application ınsights veri eklemek isteyebileceğiniz diğer bilgileri içeren uygulama kimliği, sürüm kimliği, son model değişikliği tarihi, son tarih eğitmek, son yayımlama tarihi. Bu değerler ya da ayarlanabilir uç nokta URL'si (uygulama kimliği ve sürüm kimliği) veya geliştirme API çağrısı alınan sonra web app botu ayarlarında ve buradan çekilir.  

Birden fazla LUIS uygulaması için aynı uç nokta aboneliği kullanıyorsanız, abonelik kimliği ve paylaşılan anahtar olduğunu belirten bir özellik içermelidir.

> [!div class="nextstepaction"]
> [Örnek konuşma hakkında daha fazla bilgi edinin](luis-how-to-add-example-utterances.md)
