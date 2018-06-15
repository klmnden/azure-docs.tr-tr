---
title: C# kullanarak Application Insights'a HALUK veri ekleme | Microsoft Docs
titleSuffix: Azure
description: HALUK uygulama ve C# kullanarak Application Insights ile tümleşik bir bot oluşturun.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/07/2018
ms.author: v-geberr
ms.openlocfilehash: 52b6ae224b0e8da12eb4903f5100a6e5cc39704d
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356125"
---
# <a name="add-luis-results-to-application-insights-from-a-web-app-bot"></a>HALUK sonuçlar Application Insights'a web uygulaması bot ekleyin.
Bu öğretici için HALUK yanıt bilgileri ekler [Application Insights](https://azure.microsoft.com/services/application-insights/) telemetri verileri depolama. Verileri bulduktan sonra onu Kusto dil veya çözümlemek, toplama, Powerbı ile sorgulayabilir ve amaçları ve gerçek zamanlı utterance varlıklarının rapor. Bu analiz eklediğinizde veya hedefleri ve varlıkları HALUK uygulamanızın varsa saptamanıza yardımcı olur.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
* Bir web uygulaması bot Application Insights Ekle
* Yakalama ve HALUK sorgu sonuçlarını Application Insights'a gönderme
* Application Insights üst amacı, puan ve utterance için sorgu

## <a name="prerequisites"></a>Önkoşullar

* HALUK web uygulaması bot gelen **[önceki öğretici](luis-csharp-tutorial-build-bot-framework-sample.md)** açık Application Insights ile. 
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) yerel olarak bilgisayarınızda yüklü.

> [!Tip]
> Bir abonelik zaten yoksa için kaydedebilirsiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).

Bu öğreticideki kod tümünün edinilebilir [HALUK-Samples github deposunu](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/csharp) ve Bu öğretici ile ilişkili her bir satır ile geçersiz kılınan `//LUIS Tutorial:`. 

## <a name="review-luis-web-app-bot"></a>HALUK web uygulaması bot gözden geçirin
Bu öğretici, kodu aşağıdaki gibi görünüyor veya tamamladığınızdan emin varsayar [diğer öğretici](luis-csharp-tutorial-build-bot-framework-sample.md): 

   [!code-csharp[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs "Web app bot with LUIS")]

## <a name="application-insights-in-web-app-bot"></a>Web uygulaması bot Application Insights'ta
Şu anda, web app bot hizmet oluşturmanın bir parçası eklenen Application Insights hizmeti, genel durumu telemetri bot için toplar. HALUK yanıt bilgi toplamaz. HALUK geliştirmek için HALUK yanıt bilgileri gerekir.  

HALUK yanıt yakalamak için web app bot gerekir **[Application Insights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/)** yüklü ve proje için yapılandırılmış. 

## <a name="download-web-app-bot"></a>Web uygulaması bot indirin
Kullanım [Visual Studio 2017](https://www.visualstudio.com/downloads/) eklemek ve Application Insights için web uygulaması bot yapılandırmak için. Visual Studio'da web uygulaması bot kullanabilmeniz için web uygulama bot kodu indirin.

1. Web uygulaması bot için Azure Portalı'nda seçin **yapı**.

    ![Portalda derleme seçme](./media/luis-tutorial-bot-csharp-appinsights/download-build-menu.png)

2. Seçin **indirme zip dosyası** ve dosyayı hazırlanmasını bekleyin.

    ![Zip dosyasını indirin](./media/luis-tutorial-bot-csharp-appinsights/download-link.png)

3. Seçin **indirme zip dosyası** açılır pencerede. Bilgisayarınızda konumu unutmayın, sonraki bölümde gerekir.

    ![Zip dosyası açılan indirin](./media/luis-tutorial-bot-csharp-appinsights/download-popup.png)

## <a name="open-solution-in-visual-studio-2017"></a>Visual Studio 2017 açık çözümde

1. Dosyayı bir klasöre ayıklayın. 

2. Visual Studio 2017'ni açın ve çözüm dosyasını açın `Microsoft.Bot.Sample.LuisBot.sln`. Güvenlik Uyarısı açılırsa "Tamam"'ı seçin.

    ![Visual Studio 2017 açık çözümde](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-security-warning.png)

3. Visual Studio çözümü bağımlılıkları eklemesi gerekir. İçinde **Çözüm Gezgini**, sağ tıklayın **başvuruları**seçip **NuGet paketlerini Yönet...** . 

    ![NuGet paketlerini yönetme](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-manage-nuget-packages.png)

4. NuGet Paket Yöneticisi yüklü olan paketlerin listesini gösterir. Seçin **geri** sarı çubuğunda. Geri yükleme işleminin tamamlanması için bekleyin.

    ![NuGet paketlerini geri yükleme](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-restore-packages.png)

## <a name="add-application-insights-to-the-project"></a>Application Insights projeye ekleyin
Yükleyin ve Visual Studio Application Insights yapılandırın. 

1. Visual Studio 2017 ' üst menüsünde seçin **proje**seçeneğini belirleyip **Application Insights Telemetrisi Ekle...** .

2. İçinde **uygulama Öngörüler Yapılandırması** penceresinde, seçin **Başlat boş**

    ![Application Insights yapılandırma Başlat](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configure-app-insights.png)

3. Uygulamanıza Application Insights ile kaydedin. Azure portal kimlik bilgilerinizi girmeniz gerekebilir. 

4. Visual Studio Application Insights projeye bu yaptığı gibi durumu görüntüleme ekler. 

    ![Uygulama Öngörüler durumu](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-adding-application-insights-to-project.png)

    İşlem tamamlandığında **uygulama Öngörüler Yapılandırması** ilerleme durumunu gösterir. 

    ![Uygulama Öngörüler ilerleme durumu](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configured-application-insights-to-project.png)

    **İzleme koleksiyonunu etkinleştir** kırmızı, etkin değil anlamına gelir. Bu öğretici, özellik kullanmaz. 

## <a name="build-and-resolve-errors"></a>Derleme ve hatalarını çözümleme

1. Seçerek çözümü derleme **yapı** menüsünde seçip **çözümü yeniden derle**. Yapının tamamlanmasını bekleyin. 

2. Yapı ile başarısız olursa `CS0104` hataları düzeltin vermeniz gerekir. İçinde `Controllers` klasörü içinde `MessagesController.cs file`, belirsiz kullanımını düzeltme `Activity` bağlayıcı türü ile etkinlik türü önek tarafından türü. Bunu yapmak için adı değiştirin `Activity` 22 ve gelen 36 satırlarındaki `Activity` için `Connector.Activity`. Çözümü yeniden oluşturun. Daha fazla hiçbir derleme hataları olması gerekir.

    Bu dosyanın tam kaynak şöyledir:

    [!code-csharp[MessagesController.cs file](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/MessagesController.cs "MessagesController.cs file")]

## <a name="publish-project-to-azure"></a>Projeyi Azure'a yayımlama
**Application Insights** paket projede sunulmuştur ve Azure portalında kimlik bilgilerinizi doğru şekilde yapılandırılmış. Proje için değişiklikleri geri Azure yayımlanması gerekir.

1. İçinde **Çözüm Gezgini**, proje adına sağ tıklayın ve sonra seçin **Yayımla**.

    ![Proje Portalı'na Yayımla](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish.png)

2. İçinde **Yayımla** penceresinde, seçin **yeni profil oluşturmak**.

    ![Proje Portalı'na Yayımla](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-1.png)

3. Seçin **içe profil**seçip **Tamam**.

    ![Proje Portalı'na Yayımla](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-2.png)

4. İçinde **alma yayımlama ayarları dosyası** windows, proje klasöre gidin, gitmek `PostDeployScripts` klasörü, biten dosyayı seçin `.PublishSettings`seçip `Open`. Bu proje için yayımlama artık yapılandırdınız. 

5. Yerel kaynak kodunuzu seçerek Bot hizmetinde yayımlamayı **Yayımla** düğmesi. **Çıkış** penceresi durumunu gösterir. Öğreticinin geri kalanını Azure portalında tamamlandı. Visual Studio 2017 kapatın. 

## <a name="open-three-browser-tabs"></a>Açık üç tarayıcı sekmeleri
Azure portalında web uygulaması bot bulun ve açın. Web uygulaması bot üç farklı görünümlerini aşağıdaki adımları kullanın. Üç ayrı sekme tarayıcıda açmak daha kolay olabilir: 
  
>  * Web sohbeti test
>  * Derleme/açık çevrimiçi Kod Düzenleyicisi -> uygulama hizmeti Düzenleyici
>  * Uygulama hizmeti Düzenleyicisi/açık Kudu konsol tanılama Konsolu ->

## <a name="modify-basicluisdialogcs-code"></a>BasicLuisDialog.cs kodu değiştirin

1. İçinde **App Service Düzenleyicisi** tarayıcı sekmesinde, açık `BasicLuisDialog.cs` dosya.

2. Varolan altında aşağıdaki NuGet bağımlılık ekleme `using` satırlar:

   [!code-csharp[Add using statement](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=11-12 "Add using statement")]

3. Ekleme `LogToApplicationInsights` işlevi:

   [!code-csharp[Add the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=61-92 "Add the LogToApplicationInsights function")]

    Web uygulaması bot'ın uygulama adlandırılmış ayarı Application Insights izleme anahtarı zaten `BotDevInsightsKey`. 

    İşlev son satırının verileri Application Insights'a ekler. İzleme'nin adı `LUIS`, bu web uygulaması bot tarafından toplanan herhangi bir telemetri verileri dışında benzersiz bir ad. Tüm özellikleri de ile önek `LUIS_` verileri görmek için Bu öğretici karşılaştırılan web uygulaması bot tarafından verilen bilgi ekler.

4. Çağrı `LogToApplicationInsights` işlevi en üstündeki `ShowLuisResult` işlevi:

   [!code-csharp[Use the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=114-115 "Use the LogToApplicationInsights function")]

## <a name="build-web-app-bot"></a>Web uygulaması bot derleme
1. Web uygulaması bot iki yoldan biriyle oluşturun. İlk sağ yöntemdir `build.cmd` içinde **App Service Düzenleyicisi**seçeneğini belirleyip **konsolundan çalıştırma**. Konsol çıktısını görüntüler ve ile tamamlar `Finished successfully.`

2. Bu başarıyla tamamlanmazsa, konsolunu açın, komut dosyasına gidin ve aşağıdaki adımları kullanarak çalıştırmak gerekir. İçinde **App Service Düzenleyicisi**, üst mavi çubuğunda, bot adını seçin ve ardından **açık Kudu Konsolu** aşağı açılan listesinde.

    ![Açık Kudu konsol](./media/luis-tutorial-bot-csharp-appinsights/open-kudu-console.png)

3. Konsol penceresinde aşağıdaki komutu girin:

    ```
    cd site\wwwroot && build.cmd
    ```

    Yapı ile tamamlanması için bekleyin `Finished successfully.`

## <a name="test-the-web-app-bot"></a>Web uygulaması bot test

1. Web uygulaması bot test etmek için açık **Test Web sohbet** portal özelliği. 

2. Tümcecik girin `Coffee bar on please`.  

    ![Web uygulaması bot Sohbeti test etme](./media/luis-tutorial-bot-csharp-appinsights/test-in-web-chat.png)

3. Fark chatbot yanıt görmeniz gerekir. Değişiklik verilerini Application Insights'a bot değil yanıtları gönderiyor. Application Insights'ta biraz daha fazla veri için birkaç daha fazla utterances girin:

```
Please deliver a pizza
Turn off all the lights
Turn on the hall light
```

## <a name="view-luis-entries-in-application-insights"></a>Application ınsights'ta görünümü HALUK girişleri
Application Insights'ı HALUK girişleri görmek için açın. 

1. Portalı'nda seçin **tüm kaynakları** web uygulaması bot adına göre filtre. Kaynak türü ile tıklatın **Application Insights**. Application Insights için bir ampul simgedir. 

    ![App ınsights arayın](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights.png)

2. Kaynak açıldığında tıklayın **arama** en sağdaki panelinde Büyüteç simgesini. Yeni bir panel için doğru görüntüler. Ne kadar telemetri verilerini bağlı olarak bulunduğunda paneli görüntülemek için bir saniye sürebilir. Arama `LUIS`. Liste, yalnızca, Bu öğretici ile eklenen HALUK sorgu sonuçlarına daraltıldığı.

    ![İzlemeler için arama](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace.png)

3. En üstteki girişe seçin. Yeni bir pencere sağ uçta HALUK sorgu için özel veriler de dahil olmak üzere daha ayrıntılı verileri görüntüler. Üst amacını ve onun puanı veriler içerir.

    ![İzleme öğesini gözden geçirme](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace-item.png)

    İşiniz bittiğinde, sağdaki üst seçin **X** bağımlılık öğeler listesine dönün. 


> [!Tip]
> Bağımlılık listesini kaydetmek ve daha sonra geri istiyorsanız, tıklayın **... Daha fazla** tıklatıp **Kaydet sık kullanılan**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Amacı, puan ve utterance için sorgu Application Insights
Application Insights ile verileri sorgulamak için güç size [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics) verme yanı dil [Powerbı](https://powerbi.microsoft.com). 

1. Tıklayın **Analytics** , filtre kutusunun üstüne listeleme bağımlılık üstünde. 

    ![Analytics düğmesi](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-analytics-button.png)

2. Bir sorgu penceresi üst ve veri tablosu penceresi, aşağıda ile yeni bir pencere açılır. Veritabanlarından önce kullandıysanız, bu düzenleme alışkın olduğu. Sorgu adı ile başlayan son 24 saat tüm öğeleri içerir `LUIS`. **CustomDimensions** sütununda ad/değer çiftleri olarak HALUK sorgu sonuçları.

    ![Varsayılan analizi raporu](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-1.png)

3. Üst amacı, puan ve utterance çıkarmak için sorgu penceresinde aşağıdaki son satırının hemen ekleyin:

    ```SQL
    | extend topIntent = tostring(customDimensions.LUIS_topScoringIntent)
    | extend score = todouble(customDimensions.LUIS_topScoringIntentScore)
    | extend utterance = tostring(customDimensions.LUIS_query)
    ```

4. Sorguyu çalıştırın. Sağdaki veri tablosunda kaydırın. Yeni sütunlar topIntent, puan ve utterance kullanılabilir. Sıralamak için topIntent sütununa tıklayın.

    ![Özel analizi raporu](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-2.png)


Daha fazla bilgi edinmek [Kusto sorgu dili](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-queries) veya [Powerbı için verileri dışa](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 


## <a name="learn-more-about-bot-framework"></a>Bot Framework hakkında daha fazla bilgi edinin
Daha fazla bilgi edinmek [Bot Framework](https://dev.botframework.com/).

## <a name="next-steps"></a>Sonraki adımlar

Uygulama kimliği, sürüm kimliği, son model değişikliği tarihi için uygulama istatistikleri verilerine eklemek isteyebilirsiniz diğer bilgileri içerir, son tarih eğitmek, son tarih yayımlayın. Bu değerleri ya da uç nokta URL'si (uygulama kimliği ve sürüm kimliği), ya da alınabilir bir [API yazma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d) çağrısı sonra web uygulaması bot Ayarları'nda ve buradan çekilen.  

Birden fazla HALUK uygulaması için aynı uç nokta abonelik kullanıyorsanız, abonelik kimliği ve paylaşılan bir anahtar olduğunu belirten bir özellik içermelidir. 

> [!div class="nextstepaction"]
> [Örnek utterances hakkında daha fazla bilgi edinin](luis-how-to-add-example-utterances.md)
