---
title: Azure Application Insights'tan Power BI'a dışarı aktarma | Microsoft Docs
description: Analiz sorguları, Power BI'da görüntülenebilir.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/10/2018
ms.author: mbullwin
ms.openlocfilehash: a57393918992019844e2ff4ccc13d671f0b90ed5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60900434"
---
# <a name="feed-power-bi-from-application-insights"></a>Application Insights'tan Power BI akışı
[Power BI](https://www.powerbi.com/) verileri analiz etmek ve öngörüleri paylaşmak yardımcı olan bir iş araçları paketidir. Her cihazda kullanılabilen zengin panolar. Analytics sorguları da dahil olmak üzere pek çok kaynaktan veri birleştirebilir [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md).

Application Insights verilerini Power BI'a verme üç yöntem vardır:

* [**Analiz sorguları dışarı aktar**](#export-analytics-queries). Tercih edilen yöntem budur. Herhangi bir yazma sorgu istediğiniz ve Power BI'a aktarın. Bu sorgu, herhangi bir veri yanı sıra bir Pano üzerinde yerleştirebilirsiniz.
* [**Sürekli dışarı aktarma ve Azure Stream Analytics**](../../azure-monitor/app/export-stream-analytics.md). Bu yöntem, verilerinizi uzun sürelerle depolamak istiyorsanız kullanışlıdır. Bir genişletilmiş veri saklama gereksinimine sahip değilseniz, dışarı aktarma analytics sorgu yöntemini kullanın. Sürekli dışarı aktarma ve Stream Analytics, daha fazla ayarlamak için iş ve ek depolama ek yükü içerir.
* **Power BI bağdaştırıcısı**. Grafikler dizi önceden tanımlanmış, ancak diğer kaynaklardan gelen kendi sorgularınızı ekleyebilirsiniz.

> [!NOTE]
> Power BI artık bağdaştırıcısıdır **kullanım dışı**. Bu çözüm için önceden tanımlanmış grafikleri statik düzenlenemez sorgular tarafından doldurulur. Bu sorguları düzenleme olanağına sahip değildir ve verilerinizin belirli özelliklerini bağlı olarak Power BI'ın başarılı olması için bağlantı mümkündür, ancak hiçbir veri doldurulur. Bu, sabit kodlanmış sorgu içinde ayarlanan hariç tutma kriterleri kaynaklanır. Bu çözüm, yine de bazı müşteriler için çalışabilir ancak esneklik bağdaştırıcısının eksikliği nedeniyle kullanmak için önerilen çözüm olduğunu [ **Analytics sorgusu dışarı** ](#export-analytics-queries) işlevselliği.

## <a name="export-analytics-queries"></a>Analiz sorguları Dışarı Aktar
Bu yol, beğendiğiniz veya kullanım Huniler ' dışarı aktarma Analytics sorgusunu yazmak ve ardından, bir Power BI panosuna dışarı sağlar. (Bağdaştırıcı tarafından oluşturulan bir panoya ekleyebilirsiniz.)

### <a name="one-time-install-power-bi-desktop"></a>Bir kez: Power BI Desktop uygulamasını yükleme
Application Insights sorgunuzu almak için Power BI'ın Masaüstü sürümünü kullanın. Ardından, Web veya Power BI bulut çalışma yayımlayabilirsiniz. 

Yükleme [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Bir Analytics sorgusunu dışarı aktarma
1. [Analytics açın ve sorgunuzu yazma](../../azure-monitor/log-query/get-started-portal.md).
2. Test ve Sonuçlardan memnun kadar sorgu daraltın. Vermeden önce sorgu Analytics'te düzgün çalıştığından emin olun.
3. Üzerinde **dışarı** menüsünde seçin **Power BI (M)** . Metin dosyasını kaydedin.
   
    ![Ekran görüntüsü, analizi, vurgulanan dışarı aktarma menüsüyle](./media/export-power-bi/analytics-export-power-bi.png)
4. Power BI Desktop'ta seçin **Veri Al** > **boş sorgu**. Ardından, sorgu Düzenleyicisi'nde altında **görünümü**seçin **Gelişmiş Düzenleyici**.

    Dışarı aktarılan M Dil betiğini Gelişmiş düzenleyiciye yapıştırın.

    ![Ekran görüntüsü, Power BI Desktop ile vurgulanmış Gelişmiş Düzenleyici](./media/export-power-bi/power-bi-import-analytics-query.png)

5. Power BI'ın Azure erişmesine izin vermek için kimlik bilgilerini sağlamanız gerekebilir. Kullanım **kuruluş hesabı** için Microsoft hesabınızla oturum açın.
   
    ![Ekran Power BI sorgu Ayarları iletişim kutusu](./media/export-power-bi/power-bi-import-sign-in.png)

    Kimlik bilgilerini doğrulamak ihtiyacınız varsa **veri kaynağı ayarları** sorgu Düzenleyicisi'nde menü komutu. Power BI için kimlik bilgilerinizi farklı olabilecek, Azure için kullandığınız kimlik bilgilerini belirttiğinizden emin olun.
6. Sorgunuz için bir görselleştirmeyi seçin ve x eksenini, y ekseni ve boyut kesimlere için alanları seçin.
   
    ![Power BI Desktop ekran görselleştirme seçenekleri](./media/export-power-bi/power-bi-analytics-visualize.png)
7. Raporunuzu Power BI bulut çalışma alanınıza yayımlayın. Burada, diğer web sayfalarına eşitlenmiş sürüm ekleyebilir.
   
    ![Ekran görüntüsü, Power BI Desktop, Yayımla düğmesi](./media/export-power-bi/publish-power-bi.png)
8. Rapor aralıklarla el ile yenileme veya zamanlanmış yenileme seçenekleri sayfasında ayarlayın.

### <a name="export-a-funnel"></a>Bir huni dışarı aktarma
1. [Huni olun](../../azure-monitor/app/usage-funnels.md).
2. Seçin **Power BI**.

   ![Power BI ekran düğmesi](./media/export-power-bi/button.png)

3. Power BI Desktop'ta seçin **Veri Al** > **boş sorgu**. Ardından, sorgu Düzenleyicisi'nde altında **görünümü**seçin **Gelişmiş Düzenleyici**.

   ![Ekran görüntüsü, Power BI Desktop, boş sorgu düğmesi vurgulanan](./media/export-power-bi/blankquery.png)

   Dışarı aktarılan M Dil betiğini Gelişmiş düzenleyiciye yapıştırın. 

   ![Ekran görüntüsü, Power BI Desktop ile vurgulanmış Gelişmiş Düzenleyici](./media/export-power-bi/advancedquery.png)

4. Sorgudan öğeleri seçin ve bir huni görselleştirmeyi seçin.

   ![Power BI Desktop ekran görselleştirme seçenekleri](./media/export-power-bi/selectsequence.png)

5. Başlığı anlamlı olacak şekilde değiştirin ve raporunuzu Power BI bulut çalışma alanınıza yayımlayın. 

   ![Ekran görüntüsü, Power BI Desktop ile vurgulanmış Unvan değişikliği](./media/export-power-bi/changetitle.png)

## <a name="troubleshooting"></a>Sorun giderme

Kimlik bilgileri veya veri kümesi boyutu için ilgili hatalarla karşılaşabilirsiniz. Bu hatalar hakkında yapmanız gerekenler hakkında bazı bilgiler aşağıda verilmiştir.

### <a name="unauthorized-401-or-403"></a>Yetkisiz (401 veya 403)
Yenileme belirtecinizi güncelleştirilmediyse, bu durum oluşabilir. Hâlâ erişiminiz olduğundan emin olmak için aşağıdaki adımları deneyin:

1. Azure portalında oturum açın ve kaynak erişebildiğinden emin olun.
2. Pano için kimlik bilgilerini yenilemeyi deneyin.

   Erişiminiz ve kimlik bilgileri yenileme işe yaramazsa, Lütfen bir destek bileti açın.

### <a name="bad-gateway-502"></a>Hatalı ağ geçidi (502)
Bunun nedeni genellikle çok fazla veri döndüren bir Analytics sorgusunu tarafından. Daha küçük bir zaman aralığı için sorgu kullanmayı deneyin. 

Veri kümesini analiz sorgusundan gelen azaltma gereksinimlerinizi karşılamıyorsa kullanmayı [API](https://dev.applicationinsights.io/documentation/overview) daha büyük bir veri kümesini çekmek için. API'yi kullanmak için M sorgu verme dönüştürme açıklanmıştır.

1. Oluşturma bir [API anahtarı](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).
2. Azure Resource Manager URL'si ile Application Insights API değiştirerek Analytics'ten dışarı Power BI M betik güncelleştirin.
   * Değiştirin **https:\//management.azure.com/subscriptions/...**
   * ile **https:\//api.applicationinsights.io/beta/apps/...**
3. Son olarak, kimlik bilgilerini temel güncelleştirin ve API anahtarınızı kullanın.

**Var olan bir komut dosyası**
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
**Güncelleştirilmiş betiği**
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a>Örnekleme hakkında
Uygulamanız tarafından gönderilen verilerin miktarına bağlı olarak, telemetrinizi yalnızca bir yüzdesini gönderen Uyarlamalı örnekleme özelliği kullanmak isteyebilirsiniz. Aynı el ile SDK'sında ya da alımdan örnekleme ayarladıysanız, geçerlidir. [Örnekleme hakkında daha fazla bilgi](../../azure-monitor/app/sampling.md).

## <a name="power-bi-adapter-deprecated"></a>Power BI bağdaştırıcısı (kullanım dışı)
Bu yöntem telemetri tamamlanmış bir Pano oluşturur. İlk veri kümesi önceden ancak daha fazla veri ekleyebilirsiniz.

### <a name="get-the-adapter"></a>Bağdaştırıcısı alma
1. Oturum [Power BI](https://app.powerbi.com/).
2. Açık **Veri Al** ![sol alt köşedeki GetData simgesi ekran](./media/export-power-bi/001.png), **Hizmetleri**.

    ![Ekran görüntüleri faydalanmaya Application Insights veri kaynağından](./media/export-power-bi/002.png)

3. Seçin **şimdi edinin** Application Insights altında.

   ![Ekran görüntüleri faydalanmaya Application Insights veri kaynağından](./media/export-power-bi/003.png)
4. Application Insights kaynağınıza ayrıntılarını sağlayın ve ardından **oturum açma**.

    ![Ekran faydalanmaya Application Insights veri kaynağından](./media/export-power-bi/005.png)

     Bu bilgiler Application Insights'a genel bakış bölmesinde bulunabilir:

     ![Ekran faydalanmaya Application Insights veri kaynağından](./media/export-power-bi/004.png)

5. Yeni oluşturulan Application Insights Power BI uygulamasını açın.

6. Bir veya iki içeri aktarılacak veriler için dakika bekleyin.

    ![Power BI ekran bağdaştırıcısı](./media/export-power-bi/010.png)

Application Insights grafikleri değerlerle diğer kaynakları ve analiz sorguları ile birleştirerek panoyu düzenleyebilirsiniz. Daha fazla görselleştirme galeri grafiklerde alabilirsiniz ve her grafik sahiptir ayarlayabilirsiniz.

İlk içeri aktarmadan sonra Pano ve raporlar günlük olarak güncelleştirilmeye devam. Veri kümesindeki yenileme zamanlamasını denetleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Power BI - öğrenin](https://www.powerbi.com/learning/)
* [Analizi Öğreticisi](../../azure-monitor/log-query/get-started-portal.md)

