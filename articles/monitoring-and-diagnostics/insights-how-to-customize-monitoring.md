---
title: Azure İzleyicisi'nde ölçümleri genel bakış
description: Azure'da izleme grafikleri özelleştirmeyi öğrenin.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/06/2017
ms.author: robb
ms.component: metrics
ms.openlocfilehash: 878ba004e7572ad78f574c15fd76c8868b281117
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35262265"
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Microsoft Azure ölçümlerini genel bakış
Tüm Azure Hizmetleri sistem durumu, performans, kullanılabilirlik ve hizmetlerinizi kullanımını izlemenizi mümkün kılan anahtar ölçümleri izleyin. Azure portalında bu ölçümleri görüntüleyebilir ve aynı zamanda [REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx) veya [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) ölçümleri, tamamını programlı olarak erişmek için.

Bazı hizmetler için hiçbir ölçümlerini görmek için tanılamayı açın gerekebilir. Sanal makineler gibi başkalarının ölçümleri temel bir dizi alırsınız, ancak tam etkinleştirmek için yüksek sıklıkta ölçümleri ayarlamanız. Bkz: [izleme ve tanılama](insights-how-to-use-diagnostics.md) daha fazla bilgi için.

## <a name="using-monitoring-charts"></a>İzleme grafikleri kullanma
Ölçümleri hiçbirini grafik seçtiğiniz herhangi bir süre boyunca onları.

1. İçinde [Azure Portal](https://portal.azure.com/), tıklatın **Gözat**, ve ardından kaynak izleme İlgilendiğiniz.
2. **İzleme** bölüm her Azure kaynağı için en önemli ölçümleri içerir. Örneğin, bir web uygulaması sahip **istekleri ve hataları**, burada bir sanal makine yaptığınız gibi **CPU yüzdesi** ve **Disk okuma ve yazma**: ![izleme Mercek](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)
3. Tüm grafik tıklatıldığında gösterir, **ölçüm** dikey. Grafik ek olarak dikey penceresinde toplamalar ölçümleri (örneğin, ortalama, minimum ve maksimum seçtiğiniz zaman aralığı içinde) gösteren bir tablo değil. , Kaynak için uyarı kurallardır.
    ![Ölçüm dikey penceresi](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)
4. Görünen satırları özelleştirmek için tıklatın **Düzenle** grafik düğmesine veya **grafiği Düzenle** ölçüm dikey komutu.
5. Sorgu Düzenle dikey penceresinde üç şeyler yapabilir:
   
   * Zaman aralığını değiştirme
   * Görünümü arasında geçiş çubuğu ve satırı
   * Farklı metics seçin ![Sorgu Düzenle](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)
6. Zaman aralığını değiştirmek, farklı bir aralık seçmek kadar kolay (gibi **son bir saat**) tıklayıp **kaydetmek** dikey pencerenin altındaki. Ayrıca seçebilirsiniz **özel**, geçen 2 haftada bir süre seçin sağlar. Örneğin, tüm iki hafta veya, yalnızca 1 saat dün görebilirsiniz. Farklı bir saat girmek için metin kutusuna yazın.
    ![Özel zaman aralığı](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)
7. Zaman aralığı, kanal herhangi bir sayıda grafikte gösterilecek ölçümleri seçin.
8. Kaydet tıklattığınızda değişikliklerinizi belirli bu kaynak için kaydedilir. Örneğin, iki sanal makineye sahip bir grafikte değiştirirseniz, bu diğer etkilemez.

## <a name="creating-side-by-side-charts"></a>Yan yana grafikleri oluşturma
Portalda güçlü özelleştirme ile istediğiniz sayıda grafikleri ekleyebilirsiniz.

1. İçinde **...**  dikey penceresinde üstündeki menü **eklemek döşeme**:  
    ![Menü ekleme](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Daha sonra Seç grafikten seçebilirsiniz **galeri** ekranınızın sağ tarafındaki: ![Galerisi](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Ölçüm görmüyorsanız, her zaman ekleyebilirsiniz hazır ölçümleri birini istediğiniz ve **Düzenle** ölçüm ihtiyacınız olduğunu göstermek için grafik.

## <a name="monitoring-usage-quotas"></a>Kullanım kotalarını izleme
Kullanım kotalarını gibi belirli veri bir eşik zaman içinde nokta bilgileriyle olan ancak çoğu ölçümleri zamanla eğilimlerini gösterir.

Kullanım kotalarını kotaları sahip kaynaklar için dikey penceresinde de görebilirsiniz:

![Kullanım](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Ölçümleri ile kullandığınız gibi [REST API](https://msdn.microsoft.com/library/azure/dn931963.aspx) veya [.NET SDK'sı](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) kullanım kotalarını kümesini programlı olarak erişmek için.

## <a name="next-steps"></a>Sonraki adımlar
* [Uyarı Bildirimleri Alma](insights-receive-alert-notifications.md) her bir ölçüm kestiği bir eşiği.
* [İzleme ve tanılama](insights-how-to-use-diagnostics.md) hizmetinizde ayrıntılı yüksek sıklıkta ölçümleri toplamak için.
* [Örnek sayısı otomatik olarak ölçeklendirme](insights-how-to-scale.md) hizmetinizi kullanılabilir ve yanıt verebilir durumda olduğundan emin olmak için.
* [Uygulama performansı izleme](../application-insights/app-insights-azure-web-apps.md) tam olarak kodunuzu bulutta nasıl gerçekleştiriyor anlamak istiyorsanız.
* Kullanım [JavaScript uygulamaları ve web sayfaları için Application Insights](../application-insights/app-insights-web-track-usage.md) istemci analytics bir web sayfasını ziyaret tarayıcılar hakkında alınamıyor.
* [Kullanılabilirlik ve yanıt hızını herhangi bir web sayfası izleme](../application-insights/app-insights-monitor-web-app-availability.md) , sayfanızın aşağı olup olmadığını öğrenmek için Application Insights ile.

