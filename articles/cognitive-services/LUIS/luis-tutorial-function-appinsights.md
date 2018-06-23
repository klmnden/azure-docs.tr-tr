---
title: Node.js kullanarak Application Insights'a HALUK veri ekleme | Microsoft Docs
titleSuffix: Azure
description: HALUK uygulama ve Node.js kullanarak Application Insights ile tümleşik bir bot oluşturun.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 01/18/2018
ms.author: v-geberr
ms.openlocfilehash: 929b6e1cc980d7215f91a616820e257aed26bab7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355468"
---
# <a name="add-luis-results-to-application-insights-from-a-web-app-bot"></a>HALUK sonuçlar Application Insights'a web uygulaması bot ekleyin.
Bu öğretici için HALUK istek ve yanıt bilgileri ekler [Application Insights](https://azure.microsoft.com/services/application-insights/) telemetri verileri depolama. Verileri bulduktan sonra onu Kusto dil veya çözümlemek, toplama, Powerbı ile sorgulayabilir ve amaçları ve gerçek zamanlı utterance varlıklarının rapor. Bu analiz eklediğinizde veya hedefleri ve varlıkları HALUK uygulamanızın varsa saptamanıza yardımcı olur.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
* Bir web uygulaması bot Application Insights kitaplığı Ekle
* Yakalama ve HALUK sorgu sonuçlarını Application Insights'a gönderme
* Application Insights üst amacı, puan ve utterance için sorgu

## <a name="prerequisites"></a>Önkoşullar

* HALUK web uygulaması bot gelen **[önceki öğretici](luis-nodejs-tutorial-build-bot-framework-sample.md)** açık Application Insights ile. 

> [!Tip]
> Bir abonelik zaten yoksa için kaydedebilirsiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).

Bu öğreticideki kod tümünün edinilebilir [HALUK-Samples github deposunu](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/nodejs) ve Bu öğretici ile ilişkili her bir satır ile geçersiz kılınan `//APPINSIGHT:`. 

## <a name="web-app-bot-with-luis"></a>Web uygulaması bot HALUK ile
Bu öğretici, kodu aşağıdaki gibi görünüyor veya tamamladığınızdan emin varsayar [diğer öğretici](luis-nodejs-tutorial-build-bot-framework-sample.md): 

   [!code-javascript[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/nodejs/app.js "Web app bot with LUIS")]

## <a name="add-application-insights-library-to-web-app-bot"></a>Web uygulaması bot için Application Insights kitaplığı Ekle
Şu anda, bu web uygulaması bot kullanılan Application Insights hizmeti bot için genel durum telemetri toplar. Denetlemek ve amaçları ve varlıkları düzeltmek için gereken HALUK istek ve yanıt bilgi toplamaz. 

HALUK istek ve yanıt yakalamak için web app bot gerekir **[Application Insights](https://www.npmjs.com/package/applicationinsights)** NPM paket yüklenmiş ve yapılandırılmış **app.js** dosya. Ardından hedefi iletişim işleyicileri HALUK istek ve yanıt bilgileri Application Insights'a gönderme gerekir. 

1. Azure portalında web uygulama bot hizmeti seçin **yapı** altında **Bot Yönetim** bölümü. 

    ![App ınsights arayın](./media/luis-tutorial-appinsights/build.png)

2. Uygulama hizmeti Düzenleyicisi ile yeni bir tarayıcı sekmesi açar. Üst araç çubuğunda uygulama adını seçin ve ardından seçin **açık Kudu Konsolu**. 

    ![App ınsights arayın](./media/luis-tutorial-appinsights/kudu-console.png)

3. Konsolda, Application Insights ve alt çizgi paketleri yüklemek için aşağıdaki komutu girin:

    ```
    cd site\wwwroot && npm install applicationinsights && npm install underscore
    ```

    ![App ınsights arayın](./media/luis-tutorial-appinsights/npm-install.png)

    Yüklenecek paketleri bekle:

    ```
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

    Kudu konsol tarayıcı sekmesi ile yapılır.

## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Yakalama ve HALUK sorgu sonuçlarını Application Insights'a gönderme
1. Uygulama hizmeti Düzenleyicisi tarayıcısı sekmesi açın **app.js** dosya.

2. Varolan altında aşağıdaki NPM kitaplıkları ekleme `requires` satırlar:

   [!code-javascript[Add NPM packages to app.js](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=12-16 "Add NPM packages to app.js")]

3. Application Insights nesnesi oluşturma ve web uygulaması bot uygulama ayarı kullanma **BotDevInsightsKey**: 

   [!code-javascript[Create the Application Insights object](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=68-80 "Create the Application Insights object")]

4. Ekleme **appInsightsLog** işlevi:

   [!code-javascript[Add the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=82-109 "Add the appInsightsLog function")]

    İşlev son satırının burada veri Application Insights'a eklenir. Olayın adı **HALUK sonuçları**, bu web uygulaması bot tarafından toplanan herhangi bir telemetri verileri dışında benzersiz bir ad. 

5. Kullanım **appInsightsLog** işlevi. Her amaç iletişim kutusuna ekleyin:

   [!code-javascript[Use the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=117-118 "Use the appInsightsLog function")]

6. Web uygulaması bot sınamak için kullanın **Test Web sohbet** özelliği. Tüm iş Application Insights'ta bot yanıtlarında olmadığından fark görmeniz gerekir.

## <a name="view-luis-entries-in-application-insights"></a>Application ınsights'ta görünümü HALUK girişleri
Application Insights'ı HALUK girişleri görmek için açın. 

1. Portalı'nda seçin **tüm kaynakları** web uygulaması bot adına göre filtre. Kaynak türü ile tıklatın **Application Insights**. Application Insights için bir ampul simgedir. 

    ![App ınsights arayın](./media/luis-tutorial-appinsights/search-for-app-insights.png)



2. Kaynak açıldığında tıklayın **arama** en sağdaki panelinde Büyüteç simgesini. Yeni bir panel için doğru görüntüler. Ne kadar telemetri verilerini bağlı olarak bulunduğunda paneli görüntülemek için bir saniye sürebilir. Arama `LUIS-results` ve klavyede girin. Liste, yalnızca, Bu öğretici ile eklenen HALUK sorgu sonuçlarına daraltıldığı.

    ![Bağımlılıklar için filtre](./media/luis-tutorial-appinsights/app-insights-filter.png)

3. En üstteki girişe seçin. Yeni bir pencere sağ uçta HALUK sorgu için özel veriler de dahil olmak üzere daha ayrıntılı verileri görüntüler. Üst amacını ve onun puanı veriler içerir.

    ![Bağımlılık ayrıntıları](./media/luis-tutorial-appinsights/app-insights-detail.png)

    İşiniz bittiğinde, sağdaki üst seçin **X** bağımlılık öğeler listesine dönün. 


> [!Tip]
> Bağımlılık listesini kaydetmek ve daha sonra geri istiyorsanız, tıklayın **... Daha fazla** tıklatıp **Kaydet sık kullanılan**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Amacı, puan ve utterance için sorgu Application Insights
Application Insights ile verileri sorgulamak için güç size [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics) verme yanı dil [Powerbı](https://powerbi.microsoft.com). 

1. Tıklayın **Analytics** , filtre kutusunun üstüne listeleme bağımlılık üstünde. 

    ![Analytics düğmesi](./media/luis-tutorial-appinsights/analytics-button.png)

2. Bir sorgu penceresi üst ve veri tablosu penceresi, aşağıda ile yeni bir pencere açılır. Veritabanlarından önce kullandıysanız, bu düzenleme alışkın olduğu. Sorgu adı ile başlayan son 24 saat tüm öğeleri içerir `LUIS-results`. **CustomDimensions** sütununda ad/değer çiftleri olarak HALUK sorgu sonuçları.

    ![Analytics sorgu penceresi](./media/luis-tutorial-appinsights/analytics-query-window.png)

3. Üst amacı, puan ve utterance çıkarmak için sorgu penceresinde aşağıdaki son satırının hemen ekleyin:

    ```SQL
    | extend topIntent = tostring(customDimensions.LUIS_intent_intent)
    | extend score = todouble(customDimensions.LUIS_intent_score)
    | extend utterance = tostring(customDimensions.LUIS_text)
    ```

4. Sorguyu çalıştırın. Sağdaki veri tablosunda kaydırın. Yeni sütunlar topIntent, puan ve utterance kullanılabilir. Sıralamak için topIntent sütununa tıklayın.

    ![Analytics üst hedefi](./media/luis-tutorial-appinsights/app-insights-top-intent.png)


Daha fazla bilgi edinmek [Kusto sorgu dili](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-queries) veya [Powerbı için verileri dışa](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 

## <a name="next-steps"></a>Sonraki adımlar

Uygulama kimliği, sürüm kimliği, son model değişikliği tarihi için uygulama istatistikleri verilerine eklemek isteyebilirsiniz diğer bilgileri içerir, son tarih eğitmek, son tarih yayımlayın. Bu değerleri ya da uç nokta URL'si (uygulama kimliği ve sürüm kimliği), ya da alınabilir bir [API yazma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d) çağrısı sonra web uygulaması bot Ayarları'nda ve buradan çekilen.  

Birden fazla HALUK uygulaması için aynı uç nokta abonelik kullanıyorsanız, abonelik kimliği ve paylaşılan bir anahtar olduğunu belirten bir özellik içermelidir. 

> [!div class="nextstepaction"]
> [Örnek utterances hakkında daha fazla bilgi edinin](luis-how-to-add-example-utterances.md)
