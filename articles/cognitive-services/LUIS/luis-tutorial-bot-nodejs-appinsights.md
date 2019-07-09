---
title: Application Insights Node.js
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
ms.openlocfilehash: cfed5477df75350f24e77786117e85b9c728c49a
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657743"
---
# <a name="add-luis-results-to-application-insights-from-a-bot-in-nodejs"></a>LUIS sonuçları Application Insights'a node.js'de bir Bot ekleyin
Bu öğreticide bot ve Language Understanding bilgileri ekler [Application Insights](https://azure.microsoft.com/services/application-insights/) telemetri veri depolama. Bu verileri aldıktan sonra bunu Kusto dil veya çözümlemek, toplama, Power BI ile sorgulayabilirsiniz ve hedefleri ve gerçek zamanlı utterance varlıklarının rapor. Bu analiz, eklediğinizde veya amaç ve varlıkları LUIS uygulamanızı düzenlemek, belirlemenize yardımcı olur.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bot ve dil Anlama veri Application ınsights'ta Yakala
> * Application Insights için Language Understanding verileri Sorgulama

## <a name="prerequisites"></a>Önkoşullar

* Application Insights ile oluşturulan bir Azure robot hizmeti Robotu.
* Bot kod önceki bot indirilen  **[öğretici](luis-nodejs-tutorial-bf-v4.md)** . 
* [Robot öykünücüsü](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)

Bu öğreticideki kod tüm kullanılabilir [Azure-Samples dil anlama GitHub deposu](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/v4/luis-nodejs-bot-johnsmith-src-telemetry). 

## <a name="add-application-insights-to-web-app-bot-project"></a>Application Insights web app botu projeye Ekle
Şu anda bu web app botu kullanılan Application Insights hizmeti için robot genel durumu telemetri toplar. LUIS bilgi toplamaz. 

LUIS bilgileri yakalamak için web app botu gereken **[Application Insights](https://www.npmjs.com/package/applicationinsights)** yüklenmiş ve yapılandırılmış bir NPM paketi.  

1. Tümleşik terminalde VSCode için robot proje kökündeki gösterilen komutunu kullanarak aşağıdaki NPM paketlerini ekleyin: 

    ```console
    npm install applicationinsights && npm install underscore
    ```
    
    **Alt çizgi** paket bakın ve Application ınsights'ı kullanmak daha kolay şekilde LUIS JSON yapı düzleştirmektir kullanılır.
    


## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Yakalama ve LUIS sorgu sonuçları Application Insights'a gönderme

1. VSCode içinde yeni bir dosya oluşturun **appInsightsLog.js** ve aşağıdaki kodu ekleyin:

    ```javascript
    const appInsights = require('applicationinsights');
    const _ = require("underscore");
    
    // Log LUIS results to Application Insights
    // must flatten as name/value pairs
    var appInsightsLog = (botContext,luisResponse) => {

        appInsights.setup(process.env.MicrosoftApplicationInsightsInstrumentationKey).start();
        const appInsightsClient = appInsights.defaultClient;

        // put bot context and LUIS results into single object
        var data = Object.assign({}, {'botContext': botContext._activity}, {'luisResponse': luisResponse});
    
        // Flatten data into name/value pairs
        flatten = (x, result, prefix) => {
            if(_.isObject(x)) {
                _.each(x, (v, k) => {
                    flatten(v, result, prefix ? prefix + '_' + k : k)
                })
            } else {
                result["LUIS_" + prefix] = x
            }
            return result;
        }
    
        // call fn to flatten data
        var flattenedData = flatten(data, {});
    
        // ApplicationInsights Trace 
        console.log(JSON.stringify(flattenedData));
    
        // send data to Application Insights
        appInsightsClient.trackTrace({message: "LUIS", severity: appInsights.Contracts.SeverityLevel.Information, properties: flattenedData});
    }
    
    module.exports.appInsightsLog = appInsightsLog;
    ```

    Bu dosya bot bağlam ve luıs yanıtı alır, her iki nesneleri düzleştirir ve bunları ekler bir **izleme** olay application ınsights. Olayın adı **LUIS**. 

1. Açık **iletişim kutuları** klasörü, ardından **luisHelper.js** dosya. Yeni dahil **appInsightsLog.js** gerekli bir dosya olarak ve bot bağlam ve LUIS yanıt yakalayın. Bu dosya için tam kod şöyledir: 

    ```javascript
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    const { LuisRecognizer } = require('botbuilder-ai');
    const { appInsightsLog } = require('../appInsightsLog');
    
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
    
                // APPINSIGHT: Log results to Application Insights
                appInsightsLog(context,recognizerResult);
    
    
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

## <a name="add-application-insights-instrumentation-key"></a>Application Insights izleme anahtarı Ekle 

Application ınsights'a veri eklemek için izleme anahtarı gerekir.

1. Bir tarayıcıda, [Azure portalında](https://portal.azure.com), botunuzun ait bulma **Application Insights** kaynak. Adını botun adı çoğunu olacaktır ve rastgele adının sonuna gibi karakterler `luis-nodejs-bot-johnsmithxqowom`. 
1. Application Insights kaynağı üzerinde **genel bakış** sayfasında, kopya **izleme anahtarını**.
1. VSCode içinde açın **.env** bot projenin köküne dosya. Bu dosya, tüm ortam değişkenleri tutar.  
1. Yeni bir değişken eklemek `MicrosoftApplicationInsightsInstrumentationKey` izleme anahtarınızı değerine sahip. Hiçbir put tırnak işaretleri içindeki değeri yapın. 

## <a name="start-the-bot"></a>Robotu başlatma

1. VSCode tümleşik terminalde bot başlatın:
    
    ```console
    npm start
    ```

1. Bot öykünücüyü başlatın ve bot açın. Bu [adım](luis-nodejs-tutorial-bf-v4.md#use-the-bot-emulator-to-test-the-bot) önceki öğreticide sağlanır.

1. Bot, bir soru sorun. Bu [adım](luis-nodejs-tutorial-bf-v4.md#ask-bot-a-question-for-the-book-flight-intent) önceki öğreticide sağlanır.

## <a name="view-luis-entries-in-application-insights"></a>Application ınsights'ta görünümü LUIS girişleri

LUIS girişlerini görmek için Application ınsights'ı açın. Bu verilerin Application Insights'da gösterilmesi birkaç dakika sürebilir.

1. İçinde [Azure portalında](https://portal.azure.com), botun Application Insights kaynağını açın. 
1. Kaynak açıldığında seçin **arama** ve son tüm veriler için arama **30 dakika** olay türü ile **izleme**. Adlı izlemenin seçin **LUIS**. 
1. Bot ve LUIS bilgileri altında kullanılabilir **özel özellikler**. 

    ![Uygulama anlayışları'nda depolanan LUIS özel özellikleri gözden geçirin](./media/luis-tutorial-appinsights/application-insights-luis-trace-custom-properties-nodejs.png)

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Amacını ve score utterance için Application Insights sorgu
Application Insights ile verileri sorgulamak için power size [Kusto](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview#what-language-do-log-queries-use) dışarı aktarma yanı dil [Power BI](https://powerbi.microsoft.com). 

1. Seçin **günlük (analiz)** . Bir sorgu penceresi üstündeki ve bir veri tablosu penceresi altındaki yeni bir pencere açılır. Veritabanları önce kullandıysanız, bu düzenleme tanıdık gelir. Sorgu, önceki filtrelenmiş verileri temsil eder. **CustomDimensions** sütununun bot ve LUIS bilgileri.
1. Üst amacı, Puanlama ve utterance çıkarmak için aşağıdaki yalnızca son satırı ekleyin ( `|top...` satır) sorgu penceresinde:

    ```kusto
    | extend topIntent = tostring(customDimensions.LUIS_luisResponse_luisResult_topScoringIntent_intent)
    | extend score = todouble(customDimensions.LUIS_luisResponse_luisResult_topScoringIntent_score)
    | extend utterance = tostring(customDimensions.LUIS_luisResponse_text)
    ```

1. Sorguyu çalıştırın. Yeni sütunlar topIntent, Puanlama ve utterance kullanılabilir. Sıralanacak topIntent sütun seçin.

Daha fazla bilgi edinin [Kusto sorgu dili](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) veya [verileri Power BI'a aktarma](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 

## <a name="next-steps"></a>Sonraki adımlar

Application ınsights veri eklemek isteyebileceğiniz diğer bilgileri içeren uygulama kimliği, sürüm kimliği, son model değişikliği tarihi, son tarih eğitmek, son yayımlama tarihi. Bu değerler ya da ayarlanabilir uç nokta URL'si (uygulama kimliği ve sürüm kimliği) veya geliştirme API çağrısı alınan sonra web app botu ayarlarında ve buradan çekilir.  

Birden fazla LUIS uygulaması için aynı uç nokta aboneliği kullanıyorsanız, abonelik kimliği ve paylaşılan anahtar olduğunu belirten bir özellik içermelidir. 

> [!div class="nextstepaction"]
> [Örnek konuşma hakkında daha fazla bilgi edinin](luis-how-to-add-example-utterances.md)
