---
title: "Power BI için Application Insights ' dışarı aktarma | Microsoft Docs"
description: "Power BI'da analiz sorguları görüntülenebilir."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: mbullwin
ms.openlocfilehash: ae204b79be228d55b30bb543dd25efdd9c3f0436
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="feed-power-bi-from-application-insights"></a>Power BI Application Insights akış
[Power BI](http://www.powerbi.com/) yardımcı iş analiz araçları dizisi verileri çözümlemek ve paylaşmak Öngörüler. Zengin panolar her cihazda kullanılabilir. Analytics sorgularından dahil olmak üzere pek çok kaynaktan veri birleştirebilirsiniz [Azure Application Insights](app-insights-overview.md).

Power BI için Application Insights verileri dışarı aktarma, önerilen üç yöntem vardır. Bunları ayrı olarak veya birlikte kullanabilirsiniz.

* [**Power BI bağdaştırıcısı** ](#power-pi-adapter) -uygulamanızdan telemetrinin bir tam Pano ayarlayın. Grafikler dizi önceden tanımlanmış, ancak diğer kaynaklardan gelen kendi sorgularınızı ekleyebilirsiniz.
* [**Analitik sorguları dışarı** ](#export-analytics-queries) -herhangi bir yazma analizi kullanarak istediğiniz sorgu veya kullanım Funnels ve Power BI için dışarı aktarma. Bu sorgu, diğer verilerin bir Panoda yerleştirebilirsiniz.
* [**Sürekli dışarı aktarma ve akış analizi** ](app-insights-export-stream-analytics.md) -bu ayarlamak için daha fazla iş içerir. Uzun süre boyunca verilerinizi korumak istiyorsanız kullanışlıdır. Aksi takdirde, diğer yöntemleri önerilir.

## <a name="power-bi-adapter"></a>Power BI bağdaştırıcısı
Bu yöntem telemetri tam bir Pano oluşturur. İlk veri kümesi önceden, ancak daha fazla veri ekleyebilirsiniz.

### <a name="get-the-adapter"></a>Bağdaştırıcı Al
1. Oturum [Power BI](https://app.powerbi.com/).
2. Açık **veri alma**, **Hizmetleri**, **Application Insights**
   
    ![Application Insights veri kaynağından veri alın](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. Application Insights kaynağınıza ayrıntılarını sağlayın.
   
    ![Application Insights veri kaynağından veri alın](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. Bir veya verilerin alınması iki dakika bekleyin.
   
    ![Power BI bağdaştırıcısı](./media/app-insights-export-power-bi/010.png)

Application Insights grafikler olanla diğer kaynakları ve analiz sorguları birleştirme panoyu düzenleyebilirsiniz. Burada, daha fazla grafik alabilirsiniz ve her bir grafik ayarlayabileceğiniz bir parametre yok görselleştirme galeri yoktur.

İlk içeri aktardıktan sonra günlük güncelleştirmek panoyu ve raporları devam. Veri kümesi üzerinde yenileme zamanlamasını kontrol edebilirsiniz.

## <a name="export-analytics-queries"></a>Analitik sorguları dışarı aktarma
Bu yol, kullanım Funnels ' dışarı aktarma veya gibi tüm Analytics sorgu yazın ve ardından, bir Power BI panosuna vermek sağlar. (Bağdaştırıcı tarafından oluşturulan Panoda ekleyebilirsiniz.)

### <a name="one-time-install-power-bi-desktop"></a>Bir kez: Power BI Desktop yükleyin
Application Insights sorgunuzu içeri aktarmak için Power BI'ın Masaüstü sürümünü kullanın. Ancak daha sonra onu Web veya Power BI bulut çalışma alanınızı yayımlayabilirsiniz. 

Yükleme [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Analytics sorgusu dışarı aktarma
1. [Analytics açın ve sorgunuzu yazma](app-insights-analytics-tour.md).
2. Test ve Sonuçlardan memnun kaldıysanız kadar sorgu daraltın.

   **Vermeden önce sorgu analizleri düzgün çalıştığından emin olun.**
3. Üzerinde **verme** menüsünde seçin **Power BI (M)**. Metin dosyasını kaydedin.
   
    ![Power BI sorgu verme](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. Power BI Desktop seçin **Veri Al, boş sorgu** ve ardından sorgu Düzenleyicisi'nde altında **Görünüm** seçin **Gelişmiş sorgu düzenleyici**.

    Dışarı aktarılan M Dil komut dosyası Gelişmiş sorgu düzenleyicisine yapıştırın.

    ![Gelişmiş Sorgu Düzenleyici](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. Power BI'ı Azure erişmesine izin vermek için kimlik bilgilerini sağlamanız gerekebilir. 'Kuruluş hesabı' kullanmak için Microsoft hesabınızla oturum açın.
   
    ![Power BI'ı, Application Insights sorguyu çalıştırmak etkinleştirmek üzere Azure kimlik bilgilerini sağlayın](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    (Kimlik bilgilerini doğrulamak gerekiyorsa, sorgu Düzenleyicisi'nde veri kaynağı ayarları menü komutunu kullanın. Power BI için kimlik bilgilerinizi farklı olabilir Azure için kullandığınız kimlik bilgileri belirtmek için dikkatli olun.)
2. Sorgunuz için bir görsel öğe seçin ve x ekseni, y ekseni ve boyut kesimlere alanları seçin.
   
    ![Görselleştirme seçin](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. Raporunuzu Power BI bulut alanınıza yayımlayın. Buradan, eşitlenen sürüm diğer web sayfalarına eklenebilir.
   
    ![Görselleştirme seçin](./media/app-insights-export-power-bi/publish-power-bi.png)
4. Raporun aralıklarla el ile yenileme veya zamanlanan yenileme seçenekleri sayfasında ayarlayın.

### <a name="export-a-funnel"></a>Bir huni dışarı aktarma
1. [Huni olun](usage-funnels.md)
2. Power BI düğmesini tıklatın 

   ![Powerbı düğmesi](./media/app-insights-export-power-bi/button.png)
   
3. Power BI Desktop seçin **Veri Al, boş sorgu** ve ardından sorgu Düzenleyicisi'nde altında **Görünüm** seçin **Gelişmiş sorgu düzenleyici**.

   ![Boş sorgu](./media/app-insights-export-power-bi/blankquery.png)

   Dışarı aktarılan M Dil komut dosyası Gelişmiş sorgu düzenleyicisine yapıştırın. 

   ![Gelişmiş Sorgu Düzenleyici](./media/app-insights-export-power-bi/advancedquery.png)

4. Sorgudan öğeleri seçin ve Huni görselleştirmeyi seçin

   ![Sıralı ve Huni seçin](./media/app-insights-export-power-bi/selectsequence.png)

5. Anlamlı ve Power BI bulut alanınıza raporunuzu yayımlamak için başlığını değiştirin. 

   ![Başlık değiştirme](./media/app-insights-export-power-bi/changetitle.png)

## <a name="troubleshooting"></a>Sorun giderme

### <a name="401-or-403-unauthorized"></a>401 veya 403 yetkisiz 
Yenileme belirteci güncelleştirilmez bu durum meydana gelebilir. Hala erişiminiz olduğundan emin olmak için aşağıdaki adımları deneyin. Erişiminiz ve kimlik bilgilerini yenileme işe yaramazsa, Lütfen bir destek bileti açın.

1. Azure portalında oturum açın ve kaynak erişebildiğinden emin olun
2. Kimlik bilgileri Pano yenilemeyi deneyin

### <a name="502-bad-gateway"></a>502 hatalı ağ geçidi
Bunun nedeni genellikle çok fazla veri döndüren bir Analytics sorgusu. Küçük bir zaman aralığı kullanarak denemelisiniz veya kullanarak [önce](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) veya [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) yalnızca işlevleri [proje](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) gereksinim alanları.

Analytics sorgusundan gelen veri kümesi azaltma gereksinimlerinizi karşılamıyorsa kullanmayı düşünmelisiniz [API](https://dev.applicationinsights.io/documentation/overview) daha büyük bir veri kümesi çıkarmak için. Burada, API kullanmak için M sorgu verme dönüştürme konusunda yönergeler bulunmaktadır.

1. Oluşturma bir [API anahtarı](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)
2. ARM URL AI API'si ile değiştirerek Analytics'ten dışarı Power BI M komut dosyasını güncelleştir (aşağıdaki örneğe bakın)
   * Değiştir **https://management.azure.com/subscriptions/...**
   * ile **https://api.applicationinsights.io/beta/apps/...**
3. Son olarak, kimlik bilgileri için temel güncelleştirin ve API anahtarınızı kullanın
  

**Varolan komut dosyası**
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
**Güncelleştirilmiş bir komut dosyası**
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a>Örnekleme hakkında
Uygulamanız çok miktarda veri gönderiyorsa, Uyarlamalı örnekleme özelliği çalışır ve yalnızca telemetrinizi yüzdesi gönderin. El ile SDK veya alım örnekleme ayarladıysanız, aynı geçerlidir. [Örnekleme hakkında daha fazla bilgi edinin.](app-insights-sampling.md)


## <a name="next-steps"></a>Sonraki adımlar
* [Power BI - öğrenin](http://www.powerbi.com/learning/)
* [Analytics Öğreticisi](app-insights-analytics-tour.md)

