---
title: Application Insights Node.js
titleSuffix: Azure Cognitive Services
description: LUIS uygulama ve Node.js kullanarak Application Insights ile tümleşik bir bot oluşturun.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/30/2019
ms.author: diberry
ms.openlocfilehash: bde1983f89cb2fcd0a6fddadc2c3379dee4310be
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399626"
---
# <a name="add-luis-results-to-application-insights-and-azure-functions"></a>LUIS sonuçlarını Application Insights ve Azure işlevleri ekleme
Bu öğreticide LUIS istek ve yanıt bilgileri ekler [Application Insights](https://azure.microsoft.com/services/application-insights/) telemetri veri depolama. Bu verileri aldıktan sonra bunu Kusto dil veya çözümlemek, toplama, Power BI ile sorgulayabilirsiniz ve hedefleri ve gerçek zamanlı utterance varlıklarının rapor. Bu analiz, eklediğinizde veya amaç ve varlıkları LUIS uygulamanızı düzenlemek, belirlemenize yardımcı olur.

Bot, Bot Framework ile derlenir 3.x ve Azure Web app botu. A [Bot Framework 4.x LUIS öğreticisiyle](luis-nodejs-tutorial-bf-v4.md) de kullanılabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir web app botu için Application Insights kitaplığı Ekle
> * Yakalama ve LUIS sorgu sonuçları Application Insights'a gönderme
> * Üst amacı, Puanlama ve utterance için Application Insights sorgu

## <a name="prerequisites"></a>Önkoşullar

* Öğesinden, LUIS web app botu **[önceki öğreticide](luis-nodejs-tutorial-build-bot-framework-sample.md)** açık Application Insights ile. 

> [!Tip]
> Zaten bir aboneliğiniz yoksa, kaydolabilirsiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).

Bu öğreticideki kod tüm kullanılabilir [Azure örnekleri GitHub deposunda](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/nodejs) ve Bu öğretici ile ilişkili her bir satır ile geçersiz kılınan `//APPINSIGHT:`. 

## <a name="web-app-bot-with-luis"></a>LUIS ile Web app botu
Bu öğreticide kod aşağıdaki gibi görünüyor veya tamamladığınızdan emin olduğunuz varsayılır [diğer öğretici](luis-nodejs-tutorial-build-bot-framework-sample.md): 

   [!code-javascript[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/nodejs/app.js "Web app bot with LUIS")]

## <a name="add-application-insights-library-to-web-app-bot"></a>Web app botu için Application Insights kitaplığı Ekle
Şu anda bu web app botu kullanılan Application Insights hizmeti için robot genel durumu telemetri toplar. Denetleyin ve amaç ve varlıkları düzeltmek için ihtiyaç duyduğunuz LUIS istek ve yanıt bilgi toplamaz. 

LUIS istek ve yanıt yakalamak için web app botu gereken **[Application Insights](https://www.npmjs.com/package/applicationinsights)** NPM paket yüklü ve yapılandırılmış **app.js** dosya. Daha sonra LUIS, Application Insights istek ve yanıt bilgileri göndermek hedefi iletişim işleyicileri gerekir. 

1. Azure portalında web app botu hizmeti seçin **derleme** altında **Bot Yönetim** bölümü. 

    ![Azure portalında web uygulama bot hizmeti, "Bot Yönetimi" bölümünde "Derleme"'i seçin.](./media/luis-tutorial-appinsights/build.png)

2. App Service Düzenleyicisi ile yeni bir tarayıcı sekmesi açılır. Uygulama adı üst çubuktaki seçip **Kudu konsolu aç**. 

    ![Üst çubuktaki uygulama adı seçin, sonra "Kudu konsolu aç" seçin.](./media/luis-tutorial-appinsights/kudu-console.png)

3. Konsolda, Application Insights ve alt çizgi paketleri yüklemek için aşağıdaki komutu girin:

    ```console
    cd site\wwwroot && npm install applicationinsights && npm install underscore
    ```

    ![Application Insights ve alt çizgi paketleri yüklemek için npm komutları kullanın.](./media/luis-tutorial-appinsights/npm-install.png)

    Bekleme yüklenecek paketler için:

    ```console
    luisbot@1.0.0 D:\home\site\wwwroot
    `-- applicationinsights@1.0.1 
      +-- diagnostic-channel@0.2.0 
      +-- diagnostic-channel-publishers@0.2.1 
      `-- zone.js@0.7.6 
    
    npm WARN luisbot@1.0.0 No repository field.
    luisbot@1.0.0 D:\home\site\wwwroot
    +-- botbuilder-azure@3.0.4
    | `-- azure-storage@1.4.0
    |   `-- underscore@1.4.4 
    `-- underscore@1.8.3 
    ```

    Kudu Konsolu tarayıcı sekmesine ile gerçekleştirilir.

## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Yakalama ve LUIS sorgu sonuçları Application Insights'a gönderme
1. App Service Düzenleyicisi tarayıcı sekmesinde açmak **app.js** dosya.

2. Varolan altında aşağıdaki NPM kitaplıkları ekleme `requires` satırlar:

   [!code-javascript[Add NPM packages to app.js](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=12-16 "Add NPM packages to app.js")]

3. Application Insights nesnesi oluşturun ve web app botu uygulama ayarı kullanmak **BotDevInsightsKey**: 

   [!code-javascript[Create the Application Insights object](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=68-80 "Create the Application Insights object")]

4. Ekleme **appInsightsLog** işlevi:

   [!code-javascript[Add the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=82-109 "Add the appInsightsLog function")]

    Son satırının işlevi burada verileri Application Insights'a eklenir. Olayın adı **LUIS sonuçları**, herhangi bir telemetri verisi dışında benzersiz bir ad, bu web app botu tarafından toplanır. 

5. Kullanım **appInsightsLog** işlevi. Her amaç iletişim kutusuna ekleyin:

   [!code-javascript[Use the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=117-118 "Use the appInsightsLog function")]

6. Web app botu test etmek için **Test Web sohbeti içinde** özelliği. Tüm iş Application Insights'ta bot yanıtlarında olmadığından fark görmeniz gerekir.

## <a name="view-luis-entries-in-application-insights"></a>Application ınsights'ta görünümü LUIS girişleri
LUIS girişlerini görmek için Application ınsights'ı açın. 

1. Portalında **tüm kaynakları** ardından web app botu adına göre filtreleyin. Kaynak türü ile tıklayın **Application Insights**. Application ınsights bir ampul simgedir. 

    ! [[Azure portalında app ınsights arayın](./media/luis-tutorial-appinsights/search-for-app-insights.png)

2. Kaynak açıldığında tıklayarak **arama** Sağdaki panelde Büyüteç simgesi. Yeni bir panel için doğru görüntüler. Bağlı olarak ne kadar telemetri verilerini bulunduğunda paneli görüntülemek için birkaç saniye sürebilir. Arama `LUIS-results` ve klavyede girin. Listede yalnızca, bu öğreticiyle eklenen LUIS sorgu sonuçlarına daraltıldığı.

    ![Bağımlılıklar için filtre](./media/luis-tutorial-appinsights/app-insights-filter.png)

3. En üstteki girişe seçin. Yeni bir pencerede, sağ uçta LUIS sorgu için özel veriler dahil olmak üzere daha ayrıntılı veriler görüntülenir. Üst amaç ve kendi puanı veriler içerir.

    ![Bağımlılık ayrıntıları](./media/luis-tutorial-appinsights/app-insights-detail.png)

    İşiniz bittiğinde, en sağdaki üst seçin **X** bağımlılık öğeler listesine dönün. 


> [!Tip]
> Bağımlılık listesi kaydedin ve daha sonra döndürmek isterseniz tıklayın **... Daha fazla** tıklatıp **Kaydet sık**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Amacını ve score utterance için Application Insights sorgu
Application Insights ile verileri sorgulamak için power size [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics) dışarı aktarma yanı dil [Power BI](https://powerbi.microsoft.com). 

1. Tıklayarak **Analytics** üst kısmında, filtre kutusuna listeleme bağımlılık. 

    ![Analytics düğmesi](./media/luis-tutorial-appinsights/analytics-button.png)

2. Bir sorgu penceresi üstündeki ve bir veri tablosu penceresi altındaki yeni bir pencere açılır. Veritabanları önce kullandıysanız, bu düzenleme tanıdık gelir. Sorgu adı ile başlayan son 24 saat tüm öğeleri içeren `LUIS-results`. **CustomDimensions** sütununun ad/değer çiftleri olarak LUIS sorgu sonuçları.

    ![Analytics sorgu penceresi](./media/luis-tutorial-appinsights/analytics-query-window.png)

3. Üst amacı, Puanlama ve utterance çıkarmak için sorgu penceresinde aşağıdaki son satırının hemen üstüne ekleyin:

    ```kusto
    | extend topIntent = tostring(customDimensions.LUIS_intent_intent)
    | extend score = todouble(customDimensions.LUIS_intent_score)
    | extend utterance = tostring(customDimensions.LUIS_text)
    ```

4. Sorguyu çalıştırın. Sağdaki veri tablosu için kaydırın. Yeni sütunlar topIntent, Puanlama ve utterance kullanılabilir. TopIntent sütunu sıralamak için tıklayın.

    ![Analytics üst hedefi](./media/luis-tutorial-appinsights/app-insights-top-intent.png)


Daha fazla bilgi edinin [Kusto sorgu dili](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) veya [verileri Power BI'a aktarma](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 

## <a name="next-steps"></a>Sonraki adımlar

Application ınsights veri eklemek isteyebileceğiniz diğer bilgileri içeren uygulama kimliği, sürüm kimliği, son model değişikliği tarihi, son tarih eğitmek, son yayımlama tarihi. Bu değerler ya da uç nokta URL'si (uygulama kimliği ve sürüm kimliği) veya aracılığıyla alınabilir bir [API geliştirme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d) çağırma daha sonra web app botu Ayarları'nda ve buradan çekilir.  

Birden fazla LUIS uygulaması için aynı uç nokta aboneliği kullanıyorsanız, abonelik kimliği ve paylaşılan anahtar olduğunu belirten bir özellik içermelidir. 

> [!div class="nextstepaction"]
> [Örnek konuşma hakkında daha fazla bilgi edinin](luis-how-to-add-example-utterances.md)
