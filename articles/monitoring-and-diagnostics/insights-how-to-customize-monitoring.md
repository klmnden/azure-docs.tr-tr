---
title: Azure İzleyicisi'nde ölçümlere genel bakış
description: Azure'da izleme grafiklerini özelleştirmeyi öğrenin.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/06/2017
ms.author: robb
ms.component: metrics
ms.openlocfilehash: 44daf6461a062e75435ec6f70fbc3cf10327e799
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213053"
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Microsoft azure'da ölçümlere genel bakış
Tüm Azure Hizmetleri, sistem durumu, performans, kullanılabilirlik ve hizmetlerinizin kullanımını izlemenizi ana ölçümleri izleyin. Azure portalında bu ölçümleri görüntüleyebilir ve ayrıca [REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx) veya [.NET SDK'sı](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) eksiksiz bir ölçüm programlı olarak erişmek için.

Bazı hizmetler için tüm ölçümler görmek için tanılamayı Aç gerekebilir. Sanal makineler gibi diğer kullanıcıların ölçümleri temel bir dizi alırsınız, ancak tam etkinleştirmek için yüksek sıklık düzeyindeki ölçümleri ayarlamanız gerekir. Bkz: [izleme ve tanılamayı etkinleştirme](insights-how-to-use-diagnostics.md) daha fazla bilgi için.

## <a name="using-monitoring-charts"></a>İzleme grafiklerini kullanma
Ölçümlerden herhangi grafik seçtiğiniz herhangi bir süre boyunca onları.

1. İçinde [Azure portalı](https://portal.azure.com/), tıklayın **Gözat**, ve ardından bir kaynak, izleme ilginizi çeken.
2. **İzleme** bölüm, her Azure kaynağı için en önemli ölçümleri içerir. Örneğin, bir web uygulamasına sahip **istekler ve hatalar**burada bir sanal makine de yaptığınız gibi **CPU yüzdesi** ve **Disk okuma ve yazma**: ![izleme odağı](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)
3. Herhangi bir grafiğe tıklayarak gösterilir **ölçüm** dikey penceresi. Grafiğin yanı sıra dikey penceresinde, toplamalar ölçüm (örneğin, ortalama, minimum ve maksimum, seçtiğiniz zaman aralığında) gösteren bir tablodur. Bunun altında kaynak için uyarı kurallardır.
    ![Ölçüm dikey penceresi](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)
4. Görünen satırları özelleştirmek için tıklatın **Düzenle** grafik düğmesine veya **grafiği Düzenle** ölçüm dikey penceresinde komutu.
5. Sorguyu Düzenle dikey penceresinde üç şey yapabilirsiniz:
   
   * Zaman aralığını değiştirme
   * Görünümü arasında geçiş çubuk ve çizgi
   * Farklı metics seçin ![sorguyu Düzenle](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)
6. Zaman aralığı değiştirerek farklı bir aralık seçmek kadar kolay (gibi **son bir saat**) tıklayıp **Kaydet** dikey pencerenin alt kısmındaki. Ayrıca seçebilirsiniz **özel**, son 2 hafta boyunca herhangi bir süre seçin sağlar. Örneğin, tüm iki hafta veya, yalnızca 1 saat dün görebilirsiniz. Farklı bir saat girmek için metin kutusuna yazın.
    ![Özel zaman aralığı](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)
7. Grafikte göstermek için Metrik herhangi bir sayıda zaman aralığı, bir kanal seçin.
8. Kaydet'e tıkladığınızda bu kaynağa yaptığınız değişiklikler kaydedilir. Örneğin, iki sanal makineye sahip, bir grafikte değiştirirseniz, bu diğer etkilemez.

## <a name="creating-side-by-side-charts"></a>Yan yana grafikler oluşturma
Portalda güçlü özelleştirme ile istediğiniz sayıda grafikleri ekleyebilirsiniz.

1. İçinde **...**  dikey tıklayarak üst kısmındaki menü **kutucukları ekleme**:  
    ![Menü ekleme](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Ardından, bir grafik seçebilirsiniz **galeri** ekranınızın sağ tarafında: ![Galerisi](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Ölçüm görmüyorsanız, her zaman ekleyebileceğiniz önceden belirlenmiş ölçümleri birini istediğiniz ve **Düzenle** ölçümü, gerektiğini göstermek için grafiğin.

## <a name="monitoring-usage-quotas"></a>Kullanım kotalarını izleme
Çoğu ölçümün zaman içindeki eğilimleri Göster, ancak kullanım kotalar gibi belirli veri bir eşik ile zaman içinde nokta bilgilerdir.

Kullanım kotaları, kotalarının kaynaklar dikey penceresinde de görebilirsiniz:

![Kullanım](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Ölçümler ile kullanabileceğiniz gibi [REST API](https://msdn.microsoft.com/library/azure/dn931963.aspx) veya [.NET SDK'sı](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) kullanım kotalarını tam kümesini programlı olarak erişmek için.

## <a name="next-steps"></a>Sonraki adımlar
* [Uyarı Bildirimleri Alma](insights-receive-alert-notifications.md) her bir ölçüm eşiği aştığında.
* [İzleme ve tanılamayı etkinleştirme](insights-how-to-use-diagnostics.md) hizmetinizde ayrıntılı yüksek sıklık düzeyindeki ölçümleri toplamak için.
* [Örnek sayısını otomatik olarak ölçeklendirme](insights-how-to-scale.md) hizmetinizin kullanılabilir ve yanıt verdiğinden emin olmak için.
* [Uygulama performansını izleme](../application-insights/app-insights-azure-web-apps.md) tam olarak nasıl kodunuzu bulutta performans gösterdiğini anlamak istiyorsanız.
* Kullanım [JavaScript uygulamaları ve web sayfaları için Application Insights](../application-insights/app-insights-web-track-usage.md) istemci analizi bir web sayfasını ziyaret edin tarayıcıları hakkında alınamıyor.
* [Kullanılabilirlik ve yanıt herhangi bir web sayfası izleme](../application-insights/app-insights-monitor-web-app-availability.md) ile Application ınsights'ı için bunu, sayfayı aşağı olup olmadığını öğrenebilirsiniz.

