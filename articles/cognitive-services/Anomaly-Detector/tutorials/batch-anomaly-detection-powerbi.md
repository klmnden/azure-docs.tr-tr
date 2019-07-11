---
title: Anomalileri batch algılama ve Power BI kullanarak Görselleştirme
titleSuffix: Azure Cognitive Services
description: Zaman serisi verilerinizle boyunca anomalileri görselleştirmek için Power BI ve Anomali algılayıcısı API kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: tutorial
ms.date: 04/30/2019
ms.author: aahi
ms.openlocfilehash: 74b51d04f2706d890475c500e1e730cff75397c5
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721491"
---
# <a name="tutorial-visualize-anomalies-using-batch-detection-and-power-bi"></a>Öğretici: Anomalileri batch algılama ve Power BI kullanarak Görselleştirme

Zaman serisi veri kümesinde toplu olarak anormallikleri bulmak için bu öğreticiyi kullanın. Power BI desktop'ı kullanarak, bir Excel dosyasını ele, Anomali algılayıcısı API için verileri hazırlama ve bunu boyunca istatistiksel anomalileri görselleştirin.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * İçeri aktarma ve zaman serisi veri kümesini dönüştürmek için Power BI Desktop'ı kullanma
> * Power BI Desktop, batch anomali algılama için Anomali algılayıcısı API'si ile tümleştirme
> * Beklenen ve görülen değerleri ve anomali algılama sınırları dahil olmak üzere verilerinizi içinde bulunan anormallikleri görselleştirin.

## <a name="prerequisites"></a>Önkoşullar

* [Microsoft Power BI Desktop](https://powerbi.microsoft.com/get-started/), kullanılabilir ücretsiz.
* Bir excel dosyası (.xlsx) içeren zaman serisi verilerini işaret eder. Bu Hızlı Başlangıç için örnek veri çubuğunda bulunabilir [GitHub](https://go.microsoft.com/fwlink/?linkid=2090962)

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

## <a name="load-and-format-the-time-series-data"></a>Yük ve zaman serisi verileri biçimlendirme

Kullanmaya başlamak için Power BI Desktop'ı açın ve zaman serisi verilerini önkoşulları karşıdan yükleyin. Bu excel dosyasını bir dizi eşgüdümlü evrensel saat (UTC) zaman damgası ve değer çiftleri içerir.  

> [!NOTE]
> Power BI, veri kaynakları, .csv dosyalarını, SQL veritabanları, Azure blob depolama ve daha fazlası gibi çok sayıda farklı kullanabilirsiniz.  

Power BI Desktop ana penceresinde tıklayın **giriş** Şerit. İçinde **dış veri** Şerit, açık grup **Veri Al** açılır menüsüne ve ardından **Excel**.

![Power bı'da "Veri Al" düğmesine görüntüsü](../media/tutorials/power-bi-get-data-button.png)

İletişim kutusu görüntülendikten sonra örnek .xlsx dosyası karşıdan yüklediğiniz klasöre gidin ve seçin. Sonra **Gezgin** iletişim kutusu görüntülendikten sonra **Sayfa1**, ardından **Düzenle**.

![Power bı'da veri kaynağı "Gezgin" ekran görüntüsü](../media/tutorials/navigator-dialog-box.png)

Power BI tarafından dönüştürülür zaman damgaları için ilk sütunda bir `Date/Time` veri türü. Bu zaman damgaları metne Anomali algılayıcısı API'sine gönderilmek üzere dönüştürülmesi gerekir. Power Query Düzenleyicisi'ni otomatik olarak açılmazsa tıklayın **sorguları Düzenle** Giriş sekmesinde. 

Tıklayın **dönüştürme** güç sorgu Düzenleyicisi'ndeki Şerit. İçinde **herhangi bir sütun** açık grup **veri türü:** açılan menüsünde ' nı seçip **metin**.

![Power bı'da veri kaynağı "Gezgin" ekran görüntüsü](../media/tutorials/data-type-drop-down.png)

Sütun türü değiştirme konusunda bir uyarı aldığınızda, tıklayın **değiştirin geçerli**. Daha sonra tıklayın **Kapat & Uygula** veya **Uygula** içinde **giriş** Şerit. 

## <a name="create-a-function-to-send-the-data-and-format-the-response"></a>Veri göndermek ve yanıtın biçimlendirmek için bir işlev oluşturma

Biçimlendirme ve veri dosyası Anomali algılayıcısı API'sine göndermek için yukarıda oluşturulan tablo üzerinde bir sorgu çağırabilirsiniz. Güç sorgu Düzenleyicisi'nde, gelen **giriş** Şerit, açık **yeni kaynak** açılır menüsüne ve ardından **boş sorgu**.

Yeni sorgunuzu seçildiğinden emin olun, ardından tıklatın **Gelişmiş Düzenleyici**. 

![Power bı'da "Gelişmiş Düzenleyici" düğme görüntüsü](../media/tutorials/advanced-editor-screen.png)

Gelişmiş Düzenleyici içinde tablodan sütunları ayıklar ve API'ye göndermek için Power Query M aşağıdaki kod parçacığını kullanın. Daha sonra sorgu JSON yanıtı bir tablo oluşturun ve döndürün. Değiştirin `apiKey` değişken geçerli Anomali algılayıcısı API anahtarınızı ve `endpoint` uç noktanız ile. Gelişmiş Düzenleyici sorgu girdikten sonra tıklayın **Bitti**.

```M
(table as table) => let

    apikey      = "[Placeholder: Your Anomaly Detector resource access key]",
    endpoint    = "[Placeholder: Your Anomaly Detector resource endpoint]/anomalydetector/v1.0/timeseries/entire/detect",
    inputTable = Table.TransformColumnTypes(table,{{"Timestamp", type text},{"Value", type number}}),
    jsontext    = Text.FromBinary(Json.FromValue(inputTable)),
    jsonbody    = "{ ""Granularity"": ""daily"", ""Sensitivity"": 95, ""Series"": "& jsontext &" }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Content-Type" = "application/json", #"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),

    respTable = Table.FromColumns({
                    
                     Table.Column(inputTable, "Timestamp")
                     ,Table.Column(inputTable, "Value")
                     , Record.Field(jsonresp, "IsAnomaly") as list
                     , Record.Field(jsonresp, "ExpectedValues") as list
                     , Record.Field(jsonresp, "UpperMargins")as list
                     , Record.Field(jsonresp, "LowerMargins") as list
                     , Record.Field(jsonresp, "IsPositiveAnomaly") as list
                     , Record.Field(jsonresp, "IsNegativeAnomaly") as list

                  }, {"Timestamp", "Value", "IsAnomaly", "ExpectedValues", "UpperMargin", "LowerMargin", "IsPositiveAnomaly", "IsNegativeAnomaly"}
               ),
    
    respTable1 = Table.AddColumn(respTable , "UpperMargins", (row) => row[ExpectedValues] + row[UpperMargin]),
    respTable2 = Table.AddColumn(respTable1 , "LowerMargins", (row) => row[ExpectedValues] -  row[LowerMargin]),
    respTable3 = Table.RemoveColumns(respTable2, "UpperMargin"),
    respTable4 = Table.RemoveColumns(respTable3, "LowerMargin"),

    results = Table.TransformColumnTypes(

                respTable4,
                {{"Timestamp", type datetime}, {"Value", type number}, {"IsAnomaly", type logical}, {"IsPositiveAnomaly", type logical}, {"IsNegativeAnomaly", type logical},
                 {"ExpectedValues", type number}, {"UpperMargins", type number}, {"LowerMargins", type number}}
              )

 in results
```

Sorgu veri sayfanızdaki seçerek çağırma `Sheet1` aşağıda **parametre gir**, tıklatıp **Invoke**. 

!["Gelişmiş Düzenleyici" düğme görüntüsü](../media/tutorials/invoke-function-screenshot.png)

## <a name="data-source-privacy-and-authentication"></a>Veri kaynağı gizlilik ve kimlik doğrulaması

> [!NOTE]
> Veri gizliliği ve erişim için kuruluşunuzun ilkelerine farkında olun. Bkz: [Power BI Desktop gizlilik düzeyleri](https://docs.microsoft.com/power-bi/desktop-privacy-levels) daha fazla bilgi için.

Bir dış veri kaynağını kullanan bu yana sorgusunu çalıştırmayı denediğinizde bir uyarı iletisi alabilirsiniz. 

![Power BI tarafından oluşturulan bir uyarı gösteren görüntü](../media/tutorials/blocked-function.png)

Bu sorunu gidermek için tıklatın **dosya**, ve **seçenekler ve ayarlar**. Ardından **seçenekleri**. Aşağıda **geçerli dosya**seçin **gizlilik**, ve **gizlilik düzeylerini yoksayın ve potansiyel performansı geliştirin**. 

Ayrıca, API'ye bağlanmak nasıl istediğinizi belirtmek için isteyen bir ileti alabilirsiniz.

![Erişim kimlik bilgilerini belirtmek için bir istek gösteren görüntü](../media/tutorials/edit-credentials-message.png)

Bu sorunu gidermek için tıklatın **bilgilerini Düzenle** iletisi. İletişim kutusu görüntülendikten sonra seçin **anonim** API için anonim olarak bağlanmasına izin. Ardından **Bağlan**’a tıklayın. 

Daha sonra tıklayın **Kapat & Uygula** içinde **giriş** değişiklikleri uygulamak için Şerit.

## <a name="visualize-the-anomaly-detector-api-response"></a>Anomali algılayıcısı API yanıtı görselleştirin

Ana Power BI ekranında, verileri görselleştirmek için yukarıda oluşturduğunuz sorguları kullanmaya başlayın. İlk seçin **çizgi grafik** içinde **görselleştirmeler**. Zaman damgası çizgi grafiğin için çağrılan bir işlevden eklersiniz **eksen**. Sağ tıklayın ve seçin **zaman damgası**. 

![Zaman damgası değeri sağ](../media/tutorials/timestamp-right-click.png)

Aşağıdaki alanları ekleyin **çağrılan işlev** grafiğin için **değerleri** alan. Kullanım grafiği oluşturmanıza yardımcı olmak üzere ekran görüntüsü aşağıda.

    * Value
    * UpperMargins
    * LowerMargins
    * ExpectedValues

![Yeni Hızlı ölçü ekran görüntüsü](../media/tutorials/chart-settings.png)

Alanları ekledikten sonra grafiğe ve tüm veri noktalarının gösterecek şekilde yeniden boyutlandırın. Grafiğinizi benzer şekilde görünmelidir ekran görüntüsü aşağıda:

![Yeni Hızlı ölçü ekran görüntüsü](../media/tutorials/chart-visualization.png)

### <a name="display-anomaly-data-points"></a>Anomali veri noktalarını görüntüle

Power BI penceresinin sağ tarafındaki aşağıda **alanları** sağ bölmesinde **değer** altında **çağrılan işlev sorgusu**, tıklatıp **yeni hızlı Ölçü**.

![Yeni Hızlı ölçü ekran görüntüsü](../media/tutorials/new-quick-measure.png)

Görünen ekranda işaretleyin **filtre değeri** hesaplaması olarak. Ayarlama **temel değer** için `Sum of Value`. Ardından sürükleyin `IsAnomaly` gelen **çağrılan işlev** alanlarını **filtre**. Seçin `True` gelen **filtre** açılan menüsü.

![Yeni Hızlı ölçü ekran görüntüsü](../media/tutorials/new-quick-measure-2.png)

' I tıklattıktan sonra **Tamam**, olması bir `Value for True` alanlarınızı listesinin altındaki alan. Sağ tıklayın ve yeniden adlandırın **Anomali**. Grafiğin ekleme **değerleri**. Ardından **biçimi** aracını ve x ekseni türünü ayarlayın **kategorik**.

![Yeni Hızlı ölçü ekran görüntüsü](../media/tutorials/format-x-axis.png)

Renkleri tıklayarak grafiğinize uygulamak **biçimi** aracı ve **veri renkleri**. Grafiğiniz aşağıdakine benzer görünmelidir:

![Yeni Hızlı ölçü ekran görüntüsü](../media/tutorials/final-chart.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Azure Databricks ile akış anomali algılama](anomaly-detection-streaming-databricks.md)
