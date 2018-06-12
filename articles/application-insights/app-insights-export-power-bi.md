---
title: Azure Application Insights Power BI verme | Microsoft Docs
description: Power BI'da analiz sorguları görüntülenebilir.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 10/18/2016
ms.author: mbullwin
ms.openlocfilehash: dee3313082fbe75d76bf27105979cf7e869fafad
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294131"
---
# <a name="feed-power-bi-from-application-insights"></a>Power BI Application Insights akış
[Power BI](http://www.powerbi.com/) , verileri çözümlemek ve paylaşmak Öngörüler yardımcı olan bir iş araçları paketidir. Zengin panolar her cihazda kullanılabilir. Analytics sorgularından dahil olmak üzere pek çok kaynaktan veri birleştirebilirsiniz [Azure Application Insights](app-insights-overview.md).

Power BI için Application Insights verileri dışarı aktarma, önerilen üç yöntem vardır. Bunları ayrı olarak veya birlikte kullanabilirsiniz.

* [**Power BI bağdaştırıcısı**](#power-pi-adapter). Uygulamanızdan telemetrinin bir tam Pano ayarlayın. Grafikler dizi önceden tanımlanmış, ancak diğer kaynaklardan gelen kendi sorgularınızı ekleyebilirsiniz.
* [**Analitik sorguları dışarı**](#export-analytics-queries). Herhangi bir yazma istediğiniz ve Power BI için dışarı aktarma sorgu. Analytics kullanarak sorgunuzu yazın ya da bunları kullanım Funnels yazma. Bu sorgu, diğer verilerin bir Panoda yerleştirebilirsiniz.
* [**Sürekli dışarı aktarma ve Azure akış analizi**](app-insights-export-stream-analytics.md). Bu yöntem, uzun süre boyunca verilerinizi korumak istiyorsanız kullanışlıdır. Bunu yapmazsanız, bunu ayarlamak için daha fazla iş içerdiğinden diğer yöntemlerden birini kullanın.

## <a name="power-bi-adapter"></a>Power BI bağdaştırıcısı
Bu yöntem telemetri tam bir Pano oluşturur. İlk veri kümesi önceden, ancak daha fazla veri ekleyebilirsiniz.

### <a name="get-the-adapter"></a>Bağdaştırıcı Al
1. Oturum [Power BI](https://app.powerbi.com/).
2. Açık **veri alma**, **Hizmetleri**ve ardından **Application Insights**.
   
    ![Ekran görüntüleri alma Application Insights veri kaynağından](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. Application Insights kaynağınıza ayrıntılarını sağlayın.
   
    ![Ekran görüntüsü alma Application Insights veri kaynağından](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. Bir veya verilerin alınması iki dakika bekleyin.
   
    ![Power BI ekran bağdaştırıcısı](./media/app-insights-export-power-bi/010.png)

Application Insights grafikler olanla diğer kaynakları ve analiz sorguları birleştirme panoyu düzenleyebilirsiniz. Daha fazla grafik görselleştirme galerisinde alabilirsiniz ve her bir grafik sahiptir ayarlayabilirsiniz.

İlk içeri aktardıktan sonra günlük güncelleştirmek panoyu ve raporları devam. Veri kümesi üzerinde yenileme zamanlamasını kontrol edebilirsiniz.

## <a name="export-analytics-queries"></a>Analitik sorguları dışarı aktarma
Bu yol, ister ya da kullanım Funnels dışarı herhangi Analytics sorgu yazın ve ardından, bir Power BI panosuna dışa olanak sağlar. (Bağdaştırıcı tarafından oluşturulan Panoda ekleyebilirsiniz.)

### <a name="one-time-install-power-bi-desktop"></a>Bir kez: Power BI Desktop yükleyin
Application Insights sorgunuzu içeri aktarmak için Power BI'ın Masaüstü sürümünü kullanın. Ardından, Web veya Power BI bulut çalışma alanınıza yayımlayabilirsiniz. 

Yükleme [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Analytics sorgusu dışarı aktarma
1. [Analytics açın ve sorgunuzu yazma](app-insights-analytics-tour.md).
2. Test ve Sonuçlardan memnun kaldıysanız kadar sorgu daraltın. Vermeden önce sorgu analizleri düzgün çalıştığından emin olun.
3. Üzerinde **verme** menüsünde seçin **Power BI (M)**. Metin dosyasını kaydedin.
   
    ![Ekran görüntüsü, Analytics, ile vurgulanan verme menüsü](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. Power BI Desktop'ta seçin **Veri Al** > **boş sorgu**. Ardından, sorgu Düzenleyicisi'nde altında **Görünüm**seçin **Gelişmiş Düzenleyici**.

    Dışarı aktarılan M Dil komut dosyasını Gelişmiş düzenleyiciye yapıştırın.

    ![Ekran görüntüsü, Power BI Desktop, Gelişmiş vurgulanmış düzenleyicisiyle](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. Power BI'ı Azure erişmesine izin vermek için kimlik bilgilerini sağlamanız gerekebilir. Kullanım **kuruluş hesabı** için Microsoft hesabınızla oturum açın.
   
    ![Ekran Power BI sorgu Ayarları iletişim kutusu](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    Kimlik bilgilerini doğrulamak gereksinim duyarsanız kullanın **veri kaynağı ayarları** sorgu Düzenleyicisi'nde menü komutu. Power BI için kimlik bilgilerinizi farklı olabilir Azure için kullandığınız kimlik bilgilerini belirttiğinizden emin olun.
2. Sorgunuz için bir görselleştirme seçmesi ve x ekseni, y ekseni ve boyut kesimlere alanları seçin.
   
    ![Power BI Desktop ekran vizualization seçenekleri](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. Raporunuzu Power BI bulut alanınıza yayımlayın. Buradan, eşitlenen sürüm diğer web sayfalarına eklenebilir.
   
    ![Ekran görüntüsü, Power BI Desktop, Yayımla düğmesi vurgulanan](./media/app-insights-export-power-bi/publish-power-bi.png)
4. Raporun aralıklarla el ile yenileme veya zamanlanan yenileme seçenekleri sayfasında ayarlayın.

### <a name="export-a-funnel"></a>Bir huni dışarı aktarma
1. [Huni olun](usage-funnels.md).
2. Seçin **Power BI**. 

   ![Power BI ekran düğmesi](./media/app-insights-export-power-bi/button.png)
   
3. Power BI Desktop'ta seçin **Veri Al** > **boş sorgu**. Ardından, sorgu Düzenleyicisi'nde altında **Görünüm**seçin **Gelişmiş Düzenleyici**.

   ![Ekran görüntüsü, Power BI Desktop, boş sorgu düğmesi vurgulanan](./media/app-insights-export-power-bi/blankquery.png)

   Dışarı aktarılan M Dil komut dosyasını Gelişmiş düzenleyiciye yapıştırın. 

   ![Ekran görüntüsü, Power BI Desktop, Gelişmiş vurgulanmış düzenleyicisiyle](./media/app-insights-export-power-bi/advancedquery.png)

4. Sorgudan öğeleri seçin ve bir huni görselleştirme seçin.

   ![Power BI Desktop ekran vizualization seçenekleri](./media/app-insights-export-power-bi/selectsequence.png)

5. Başlık anlamlı olacak şekilde değiştirin ve raporunuzu Power BI bulut alanınıza yayımlayın. 

   ![Ekran görüntüsü, Power BI Desktop, vurgulanmış başlık değişikliği](./media/app-insights-export-power-bi/changetitle.png)

## <a name="troubleshooting"></a>Sorun giderme

Kimlik bilgileri veya veri kümesi boyutunu ilgili hatalarla karşılaşabilirsiniz. Aşağıda, bu hatalar hakkında yapmanız gerekenler hakkında bazı bilgiler verilmiştir.

### <a name="unauthorized-401-or-403"></a>Yetkisiz (401 veya 403)
Yenileme belirteci güncelleştirilmez bu durum meydana gelebilir. Hala erişiminiz olduğundan emin olmak için aşağıdaki adımları deneyin:

1. Azure portalında oturum açın ve kaynak erişebildiğinden emin olun.
2. Pano için kimlik bilgilerini yenilemeyi deneyin.

 Erişiminiz ve kimlik bilgilerini yenileme işe yaramazsa, Lütfen bir destek bileti açın.

### <a name="bad-gateway-502"></a>Hatalı ağ geçidi (502)
Bunun nedeni genellikle çok fazla veri döndüren bir Analytics sorgusu. Küçük bir zaman aralığı için sorgu kullanmayı deneyin. 

Analytics sorgusundan gelen veri kümesi azaltma gereksinimlerinizi karşılamıyorsa, kullanmayı [API](https://dev.applicationinsights.io/documentation/overview) daha büyük bir veri kümesi çıkarmak için. API kullanmak için M sorgu verme dönüştürme bırakılır.

1. Oluşturma bir [API anahtarı](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).
2. Azure Resource Manager URL Application Insights API'si ile değiştirerek Analytics'ten dışarı Power BI M komut dosyasını güncelleştirin.
   * Değiştir  **https://management.azure.com/subscriptions/...**
   * ile  **https://api.applicationinsights.io/beta/apps/...**
3. Son olarak, kimlik bilgileri için temel güncelleştirin ve API anahtarınızı kullanın.
  

**Varolan komut dosyası**
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
**Güncelleştirilmiş bir komut dosyası**
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a>Örnekleme hakkında
Uygulamanız çok miktarda veri gönderiyorsa telemetrinizi yalnızca bir yüzdesini gönderir Uyarlamalı örnekleme özelliğini kullanmak isteyebilirsiniz. El ile SDK veya alım örnekleme ayarladıysanız, aynı geçerlidir. [Örnekleme hakkında daha fazla bilgi](app-insights-sampling.md).


## <a name="next-steps"></a>Sonraki adımlar
* [Power BI - öğrenin](http://www.powerbi.com/learning/)
* [Analytics Öğreticisi](app-insights-analytics-tour.md)

