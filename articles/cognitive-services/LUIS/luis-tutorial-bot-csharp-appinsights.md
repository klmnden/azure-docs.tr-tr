---
title: Application ınsights'ı kullanmaC#
titleSuffix: Azure Cognitive Services
description: LUIS uygulama ve C# kullanarak Application Insights ile tümleşik bir bot oluşturun.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/30/2019
ms.author: diberry
ms.openlocfilehash: 56ceb48be9d5cc9d1cdceed7505e2e3e918a7286
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399668"
---
# <a name="add-luis-results-to-application-insights-with-a-bot-in-c"></a>Application Insights içinde bir Bot ile LUIS sonuçları ekleyinC#

Bu öğreticide LUIS yanıt bilgileri ekler [Application Insights](https://azure.microsoft.com/services/application-insights/) telemetri veri depolama. Bu verileri aldıktan sonra bunu Kusto dil veya çözümlemek, toplama, Power BI ile sorgulayabilirsiniz ve hedefleri ve gerçek zamanlı utterance varlıklarının rapor. Bu analiz, eklediğinizde veya amaç ve varlıkları LUIS uygulamanızı düzenlemek, belirlemenize yardımcı olur.

Bot, Bot Framework ile derlenir 3.x ve Azure Web app botu. A [Bot Framework 4.x LUIS öğreticisiyle](luis-csharp-tutorial-bf-v4.md) de kullanılabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir web app botu için Application Insights ekleme
> * Yakalama ve LUIS sorgu sonuçları Application Insights'a gönderme
> * Üst amacı, Puanlama ve utterance için Application Insights sorgu

## <a name="prerequisites"></a>Önkoşullar

* Öğesinden, LUIS web app botu **[önceki öğreticide](luis-csharp-tutorial-build-bot-framework-sample.md)** açık Application Insights ile.
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) yerel olarak bilgisayarınızda yüklü.

> [!Tip]
> Zaten bir aboneliğiniz yoksa, kaydolabilirsiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).

Bu öğreticideki kod tüm kullanılabilir [Azure örnekleri GitHub deposunda](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/csharp) ve Bu öğretici ile ilişkili her bir satır ile geçersiz kılınan `//LUIS Tutorial:`.

## <a name="review-luis-web-app-bot"></a>LUIS web app botu gözden geçirin

Bu öğreticide kod aşağıdaki gibi görünüyor veya tamamladığınızdan emin olduğunuz varsayılır [diğer öğretici](luis-csharp-tutorial-build-bot-framework-sample.md):

   [!code-csharp[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs "Web app bot with LUIS")]

## <a name="application-insights-in-web-app-bot"></a>Application Insights'ta web app botu

Şu anda web app botu hizmeti oluşturma işleminin bir parçası olarak eklenen Application Insights hizmeti için robot genel durumu telemetri toplar. LUIS yanıt bilgi toplamaz. Analiz etmek ve LUIS geliştirmek için LUIS yanıt bilgileri gerekir.  

LUIS yanıt yakalamak için web app botu gereken **[Application Insights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/)** yüklü ve proje için yapılandırılmış.

## <a name="download-web-app-bot"></a>Web app botu indirin

Kullanım [Visual Studio 2017](https://www.visualstudio.com/downloads/) eklemek ve Application Insights için web app botu yapılandırmak için. Visual Studio'da web app botu kullanmak için web app botu kodunu indirin.

1. Web app botu için Azure portalında seçin **yapı**.

    ![Portalda derleme seçin](./media/luis-tutorial-bot-csharp-appinsights/download-build-menu.png)

2. Seçin **indirme zip dosyası** ve dosya hazırlanmasını bekleyin.

    ![Zip dosyasını indirin](./media/luis-tutorial-bot-csharp-appinsights/download-link.png)

3. Seçin **indirme zip dosyası** açılır pencerede. Bilgisayarınızdaki bir konuma unutmayın, sonraki bölümde ihtiyacınız olacak.

    ![Zip dosyası açılır indirin](./media/luis-tutorial-bot-csharp-appinsights/download-popup.png)

## <a name="open-solution-in-visual-studio-2017"></a>Visual Studio 2017'de çözümü açın

1. Dosyayı bir klasöre ayıklayın.

2. Visual Studio 2017'yi açın ve çözüm dosyasını açın `Microsoft.Bot.Sample.LuisBot.sln`. Güvenlik uyarısını açılırsa "Tamam"'ı seçin.

    ![Visual Studio 2017'de çözümü açın](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-security-warning.png)

3. Bağımlılıkları çözüme eklemek Visual Studio gerekir. İçinde **Çözüm Gezgini**, sağ **başvuruları**seçip **NuGet paketlerini Yönet...** .

    ![NuGet paketlerini yönetme](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-manage-nuget-packages.png)

4. NuGet Paket Yöneticisi yüklü paketler listesini gösterir. Seçin **geri** sarı çubuk. Geri yükleme işleminin tamamlanması için bekleyin.

    ![NuGet paketlerini geri yükleme](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-restore-packages.png)

## <a name="add-application-insights-to-the-project"></a>Application ınsights'ı projeye ekleyin.

Yükleyin ve Visual Studio'da Application Insights'ı yapılandırın.

1. Visual Studio 2017'de üst menüden **proje**, ardından **Application Insights Telemetrisi Ekle...** .

2. İçinde **Application Insights Yapılandırması** penceresinde **ücretsiz Başlat**

    ![Application Insights yapılandırma Başlat](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configure-app-insights.png)

3. Uygulamanızı Application Insights'a kaydedin. Azure portal kimlik bilgilerinizi girmeniz gerekebilir.

4. Visual Studio Application Insights, bunu yapar gibi durumu görüntüleme projeye ekler.

    ![Application Insights durumu](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-adding-application-insights-to-project.png)

    İşlem tamamlandığında **Application Insights Yapılandırması** ilerleme durumunu gösterir.

    ![Application Insights ilerleme durumu](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configured-application-insights-to-project.png)

    **İzlemesini etkinleştir** kırmızı, yani, etkin değil. Bu öğretici, özellik kullanmaz.

## <a name="build-and-resolve-errors"></a>Derleme ve hatalarını çözümleme

1. Çözüm seçerek yapı **derleme** menüsünde, ardından **çözümü yeniden derle**. Derlemenin tamamlanmasını bekleyin.

2. Yapı başarısız olursa `CS0104` hataları için gereksinim duyduğunuz bunları da onarabilir. İçinde `Controllers` klasörü içinde `MessagesController.cs file`, belirsiz kullanımını düzeltme `Activity` türe göre bağlayıcı türü ile etkinlik türü önek. Bunu yapmak için adı değiştirin `Activity` 22 ve 36'dan satırlarındaki `Activity` için `Connector.Activity`. Çözümü yeniden oluşturun. Daha fazla derleme hataları olması gerekir.

    Bu dosyanın tam kaynağıdır:

    [!code-csharp[MessagesController.cs file](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/MessagesController.cs "MessagesController.cs file")]

## <a name="publish-project-to-azure"></a>Projeyi Azure'da yayımlama

**Application Insights** paket artık projededir ve kimlik bilgileriniz Azure Portalı'nda doğru yapılandırılmış. Azure'da yeniden yayımlanabilmesi değişiklikler proje için gerekir.

1. İçinde **Çözüm Gezgini**, proje adına sağ tıklayın ve ardından seçin **Yayımla**.

    ![Proje portalında yayımlayın](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish.png)

2. İçinde **Yayımla** penceresinde **yeni profil oluşturma**.

    ![Yayımlama işleminin bir parçası olarak, yeni profili oluşturun.](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-1.png)

3. Seçin **profili içeri aktar**seçip **Tamam**.

    ![Yayımlama işleminin bir parçası olarak, profili içeri aktar](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-2.png)

4. İçinde **yayımlama ayarları dosyasını içeri aktar** windows gidin, proje klasörünüze gidin `PostDeployScripts` klasöründe biten dosyaları seçin `.PublishSettings`seçip `Open`. Bu proje için yayımlama yapılandırdınız.

5. Yerel kaynak kodunuz için robot hizmeti seçerek yayımlama **Yayımla** düğmesi. **Çıkış** penceresi durumunu gösterir. Azure portalında Bu öğreticinin geri kalanını tamamlandı. Visual Studio 2017'yi kapatın.

## <a name="open-three-browser-tabs"></a>Üç tarayıcı sekmeleri Aç

Azure portalında web app botu bulun ve açın. Web app botu üç farklı görünümlerini aşağıdaki adımları kullanın. Tarayıcıda açmak için üç ayrı sekmeler daha kolay olabilir:
  
>  * Test Web sohbeti
>  * Derleme/açık çevrimiçi Kod Düzenleyicisi -> App Service Düzenleyicisi
>  * App Service Düzenleyicisi/açık Kudu Konsolu -> tanılama Konsolu

## <a name="modify-basicluisdialogcs-code"></a>BasicLuisDialog.cs kodu değiştirin

1. İçinde **App Service Düzenleyicisi** açık tarayıcı sekmesinde `BasicLuisDialog.cs` dosya.

2. Varolan altında aşağıdaki NuGet bağımlılık ekleme `using` satırlar:

   [!code-csharp[Add using statement](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=11-12 "Add using statement")]

3. Ekleme `LogToApplicationInsights` işlevi:

   [!code-csharp[Add the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=61-92 "Add the LogToApplicationInsights function")]

    Application Insights izleme anahtarı zaten web app botu'nın uygulama adlandırılmış ayarı `BotDevInsightsKey`.

    İşlev son satırının Application Insights'a veri ekler. İzleme'nin adı `LUIS`, herhangi bir telemetri verisi dışında benzersiz bir ad, bu web app botu tarafından toplanır. Tüm özellikleri de ön eki `LUIS_` hangi verileri görebilmeniz için bu öğreticiyi karşılaştırılan web app botu tarafından verilen bilgi ekler.

4. Çağrı `LogToApplicationInsights` işlevi en üstündeki `ShowLuisResult` işlevi:

   [!code-csharp[Use the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=114-115 "Use the LogToApplicationInsights function")]

## <a name="build-web-app-bot"></a>Web app botu oluşturun

1. İki yoldan biriyle web app botu oluşturun. İlk yöntem sağ olmaktır `build.cmd` içinde **App Service Düzenleyicisi**, ardından **konsolundan çalıştırın**. Konsol çıktısını görüntüler ve ile tamamlar `Finished successfully.`

2. Bu başarıyla tamamlanmazsa konsolunu açın, komut dosyasına gidin ve aşağıdaki adımları kullanarak çalıştırmak gerekir. İçinde **App Service Düzenleyicisi**, üstteki mavi çubuğunda, botunuzun adını seçin ve ardından seçin **Kudu konsolu aç** aşağı açılan listesinde.

    ![Açık Kudu Konsolu](./media/luis-tutorial-bot-csharp-appinsights/open-kudu-console.png)

3. Konsol penceresinde aşağıdaki komutu girin:

    ```console
    cd site\wwwroot && build.cmd
    ```

    İle tamamlanmasını bekle `Finished successfully.`

## <a name="test-the-web-app-bot"></a>Web app botu test

1. Web app botu test etmek için açık **Test Web sohbeti içinde** portalında özelliği.

2. Tümcecik girin `Coffee bar on please`.  

    ![Web app botu Sohbeti test](./media/luis-tutorial-bot-csharp-appinsights/test-in-web-chat.png)

3. Fark sohbet botu yanıt görmeniz gerekir. Değişikliği veri Application Insights için robot değil yanıtları gönderiyor. Application Insights'ta biraz daha fazla veri için birkaç daha fazla konuşma girin:

|Konuşmalar|
|--|
|Lütfen bir pizza sunun|
|Tüm ışıkları Aç|
|Hall ışığını Aç|


## <a name="view-luis-entries-in-application-insights"></a>Application ınsights'ta görünümü LUIS girişleri

LUIS girişlerini görmek için Application ınsights'ı açın.

1. Portalında **tüm kaynakları** ardından web app botu adına göre filtreleyin. Kaynak türü ile tıklayın **Application Insights**. Application ınsights bir ampul simgedir.

    ![Azure portalında app ınsights arayın](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights.png)

2. Kaynak açıldığında tıklayarak **arama** Sağdaki panelde Büyüteç simgesi. Yeni bir panel için doğru görüntüler. Bağlı olarak ne kadar telemetri verilerini bulunduğunda paneli görüntülemek için birkaç saniye sürebilir. `LUIS` arayın. Listede yalnızca, bu öğreticiyle eklenen LUIS sorgu sonuçlarına daraltıldığı.

    ![İzlemeleri arayın](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace.png)

3. En üstteki girişe seçin. Yeni bir pencerede, sağ uçta LUIS sorgu için özel veriler dahil olmak üzere daha ayrıntılı veriler görüntülenir. Üst amaç ve kendi puanı veriler içerir.

    ![İzleme öğesini gözden geçirme](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace-item.png)

    İşiniz bittiğinde, en sağdaki üst seçin **X** bağımlılık öğeler listesine dönün.

> [!Tip]
> Bağımlılık listesi kaydedin ve daha sonra döndürmek isterseniz tıklayın **... Daha fazla** tıklatıp **Kaydet sık**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Amacını ve score utterance için Application Insights sorgu

Application Insights ile verileri sorgulamak için power size [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics) dışarı aktarma yanı dil [Power BI](https://powerbi.microsoft.com).

1. Tıklayarak **Analytics** üst kısmında, filtre kutusuna listeleme bağımlılık.

    ![Analytics düğmesi](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-analytics-button.png)

2. Bir sorgu penceresi üstündeki ve bir veri tablosu penceresi altındaki yeni bir pencere açılır. Veritabanları önce kullandıysanız, bu düzenleme tanıdık gelir. Sorgu adı ile başlayan son 24 saat tüm öğeleri içeren `LUIS`. **CustomDimensions** sütununun ad/değer çiftleri olarak LUIS sorgu sonuçları.

    ![Varsayılan analiz raporu](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-1.png)

3. Üst amacı, Puanlama ve utterance çıkarmak için sorgu penceresinde aşağıdaki son satırının hemen üstüne ekleyin:

    ```kusto
    | extend topIntent = tostring(customDimensions.LUIS_topScoringIntent)
    | extend score = todouble(customDimensions.LUIS_topScoringIntentScore)
    | extend utterance = tostring(customDimensions.LUIS_query)
    ```

4. Sorguyu çalıştırın. Sağdaki veri tablosu için kaydırın. Yeni sütunlar topIntent, Puanlama ve utterance kullanılabilir. TopIntent sütunu sıralamak için tıklayın.

    ![Özel analiz raporu](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-2.png)

Daha fazla bilgi edinin [Kusto sorgu dili](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) veya [verileri Power BI'a aktarma](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi).

## <a name="learn-more-about-bot-framework"></a>Bot Framework hakkında daha fazla bilgi edinin

Daha fazla bilgi edinin [Bot Framework](https://dev.botframework.com/).

## <a name="next-steps"></a>Sonraki adımlar

Application ınsights veri eklemek isteyebileceğiniz diğer bilgileri içeren uygulama kimliği, sürüm kimliği, son model değişikliği tarihi, son tarih eğitmek, son yayımlama tarihi. Bu değerler ya da uç nokta URL'si (uygulama kimliği ve sürüm kimliği) veya aracılığıyla alınabilir bir [API geliştirme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d) çağırma daha sonra web app botu Ayarları'nda ve buradan çekilir.  

Birden fazla LUIS uygulaması için aynı uç nokta aboneliği kullanıyorsanız, abonelik kimliği ve paylaşılan anahtar olduğunu belirten bir özellik içermelidir.

> [!div class="nextstepaction"]
> [Örnek konuşma hakkında daha fazla bilgi edinin](luis-how-to-add-example-utterances.md)
