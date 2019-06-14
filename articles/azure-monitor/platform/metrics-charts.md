---
title: Gelişmiş özelliklerini Azure ölçüm Gezgini
description: Azure İzleyici ölçüm Gezgini'ni gelişmiş özellikler hakkında bilgi edinin
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: 67e4281b24a7489cf202d82bdddbe99992aac095
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60256913"
---
# <a name="advanced-features-of-azure-metrics-explorer"></a>Gelişmiş özelliklerini Azure ölçüm Gezgini

> [!NOTE]
> Bu makalede, ölçüm Gezgini temel özellikleri hakkında bilgi sahibi olduğunuz varsayılır. Yeni bir kullanıcıysanız ve ölçüm ilk grafiğinizi oluşturun, bkz öğrenmek istiyorsanız [Azure ölçüm Gezgini ile çalışmaya başlama](metrics-getting-started.md).

## <a name="metrics-in-azure"></a>Azure ölçümleri

[Azure İzleyicisi'nde ölçümler](data-platform-metrics.md) ölçülen değerleri ve toplanan ve zaman içinde depolanmış olan sayıları dizi. Standart (veya "platformu") ölçüm ve özel ölçüm vardır. Standart ölçümler, Azure platformu tarafından kendisi sağlanır. Standart ölçümler, Azure kaynaklarınızın durumunu ve kullanım istatistikleri yansıtır. Özel ölçüm kullanarak uygulamalarınızı tarafından Azure'a gönderilir ancak [özel olaylar ve ölçümler için Application Insights API](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics), [Windows Azure tanılama (WAD) uzantısı](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-overview), ya da [Azure REST API izleme](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-store-custom-rest-api).

## <a name="create-views-with-multiple-metrics-and-charts"></a>Birden çok ölçüm ve grafikler ile görünümlerini oluşturma

Birden çok ölçüm satırı çizim veya aynı anda birden çok ölçüm grafikleri Göster grafikler oluşturabilirsiniz. Bu işlev sağlar:

- başka bir performanstaki ilgili ölçümleri aynı grafikte bir değeri görmek için ilgili
- farklı ölçü yakın ölçümlerini görüntüle
- görsel olarak toplama ve birden çok kaynaklardan ölçümleri karşılaştırın

Örneğin, 5 depolama hesapları kullandığınız ve bunlar arasında tüketilen toplam ne kadar alan öğrenmek istiyorsanız, tek tek ve tüm değerlerin toplamını belirli noktalarda süresini gösterir (Yığılmış) alan grafiği oluşturabilirsiniz.

### <a name="multiple-metrics-on-the-same-chart"></a>Birden çok ölçümleri aynı grafikte

İlk olarak, [yeni bir grafik oluşturun](metrics-getting-started.md#create-your-first-metric-chart). Tıklayın **ölçüm Ekle** ve aynı grafiğe başka bir ölçüm eklemek için adımları yineleyin.

   > [!NOTE]
   > Genellikle, bir grafikte ölçümleri farklı ölçü (yani "milisaniye" ve "kilobayt") veya önemli ölçüde farklı ölçeklendirme ile sahip istemezsiniz. Bunun yerine, birden çok grafik kullanmayı düşünün. Ölçüm Gezgini'nde birden çok grafik oluşturmak için Hesap Ekle düğmesine tıklayın.

### <a name="multiple-charts"></a>Birden fazla grafiği

Tıklayın **Ekle grafik** ve farklı bir ölçümü ile başka bir grafik oluşturun.

### <a name="order-or-delete-multiple-charts"></a>Sipariş veya birden fazla Grafiği Sil

Sipariş ya da birden fazla grafiği silmek için üç noktayı tıklayın ( **...**  ) grafik menüsünü açın ve uygun menü öğesi, sembol **Yukarı Taşı**, **Aşağı Taşı**, veya **Sil**.

## <a name="apply-filters-to-charts"></a>Grafikler için filtre uygulayın

Boyutlarla ölçümleri gösteren grafikler için filtre uygulayabilirsiniz. "İşlem sayısı" ölçüm boyut varsa, "işlem yanıtı başarılı veya daha sonra bu boyutu üzerinde filtreleme başarısız olduğunu gösteren yanıt türü", bir grafik çizgisi için örneğin, çizim yalnızca başarılı (veya yalnızca başarısız) işlemleri. 

### <a name="to-add-a-filter"></a>Filtre eklemek için

1. Seçin **Filtre Ekle** grafiğin üstünde

2. Filtre uygulamak istediğiniz boyutu (özellik) seçin

   ![Ölçüm görüntüsü](./media/metrics-charts/00006.png)

3. (Bu örnekte filtreleme başarılı depolama işlemleri gösterilmiştir) grafik çizim, dahil etmek istediğiniz hangi boyut değerleri seçin:

   ![Ölçüm görüntüsü](./media/metrics-charts/00007.png)

4. Filtre değerleri belirledikten sonra filtre Seçici uzağa kapatmak için tıklayın. Artık grafik kaç depolama işlemi başarısız olmuş gösterir:

   ![Ölçüm görüntüsü](./media/metrics-charts/00008.png)

5. Aynı grafikleri birden fazla filtre uygulamak için 1-4 arası adımları tekrarlayabilirsiniz.



## <a name="apply-splitting-to-a-chart"></a>Bir grafiği bölme Uygula

Bir ölçüm bölme ölçütü: ölçüm karşılaştırma birbirleriyle nasıl farklı parçalarını görselleştirmek için boyut ve boyutun harici segmentleriyle.

### <a name="apply-splitting"></a>Bölme Uygula

1. Tıklayarak **uygulamak bölme** grafiğin üstünde.
 
   > [!NOTE]
   > Bölme sahip birden çok ölçüm grafikleri ile kullanılamaz. Ayrıca, birden çok filtre ancak yalnızca bir bölme boyutu tek bir grafiğe uygulanmış olabilir.

2. Grafiğinizi segmentlere ayırmak istediğiniz bir boyut seçin:

   ![Ölçüm görüntüsü](./media/metrics-charts/00010.png)

   Şimdi grafik, artık her bir kesim boyutu için birden fazla satır gösterir:

   ![Ölçüm görüntüsü](./media/metrics-charts/00012.png)

3. Liste kutusundan tıklayın **gruplandırma Seçici** kapatmak için.

   > [!NOTE]
   > Filtreleme hem de aynı boyutta bölme senaryonuz için ilgisi olmayan ve grafikleri okunmalarını kolaylaştırmak segmentleri gizlemek için kullanın.

## <a name="lock-boundaries-of-chart-y-axis"></a>Grafik y ekseni sınırlarını kilidi

Grafiğin büyük değerler daha küçük dalgalanmaları gösterdiğinde y ekseni aralığını kilitleme önemli hale gelir. 

Örneğin, başarılı istek hacmi % %99,99 %99,5 açılır, hizmet kalitesi önemli azalmaya temsil edebilir. Ancak, bir küçük sayısal değeri dalgalanma tercihinize zor veya imkansız bile varsayılan hesap ayarlarından olacaktır. Bu durumda bu küçük bırakma daha belirgin hale getirir %99 grafiğinin en düşük sınır kilitlenemedi. 

Başka bir kullanılabilir bellek bir dalgalanma burada değeri teknik olarak hiçbir zaman 0 ulaşacak örnektir. Daha yüksek bir değer aralığına düzeltme düşme kullanılabilir bellek. nokta kolaylaştırmak. 

Y ekseni aralığını denetlemek için kullanma "..." Grafik menü ve seçin **grafiği Düzenle** grafiği ayarları Gelişmiş erişim için. Y ekseni aralığını bölümündeki değerleri değiştirin veya kullanın **otomatik** varsayılanlara geri düğmesi.

![Ölçüm görüntüsü](./media/metrics-charts/00014-manually-set-granularity.png)

> [!WARNING]
> Çeşitli izlemek grafiklerin y ekseni sınırlarını kilitleme sayar veya toplayan bir dönem boyunca zaman (ve dolayısıyla kullanım sayısı, toplam, minimum veya maksimum toplamalar) genellikle otomatik varsayılanlara güvenmek yerine bir sabit zaman ayrıntı düzeyi belirterek gerektirir. Bu gereklidir çünkü zaman ayrıntı düzeyi tarayıcı penceresini yeniden boyutlandırma veya başka bir ekran çözünürlüğünü giden kullanıcı tarafından otomatik olarak değiştirildiğinde grafiklerde değerlerini değiştirin. Elde edilen grafiğin y ekseni aralığını oluşan geçerli seçimi geçersiz kılmalarını görünümünü zaman ayrıntı düzeyi efektleri değiştirin.

## <a name="pin-charts-to-dashboards"></a>Panolar için PIN grafikleri

Grafikleri yapılandırdıktan sonra yeniden, büyük olasılıkla diğer izleme telemetri bağlamında görüntülemek veya takımınızla paylaşmak için Pano eklemek isteyebilirsiniz.

Yapılandırılmış bir grafik bir panoya sabitlemek için:

Grafiğinizi yapılandırdıktan sonra tıklayarak **grafik Eylemler** menüsü sağ üst köşe grafiğin ve tıklayın **panoya Sabitle**.

![Ölçüm görüntüsü](./media/metrics-charts/00013.png)

## <a name="create-alert-rules"></a>Uyarı kuralları oluşturma

Uyarı kuralı için temel bir ölçüm tabanlı olarak ölçümlerinizi görselleştirmek için ayarlanan ölçütlerle kullanabilirsiniz. Yeni uyarı verme kuralı, hedef kaynak, ölçüm, bölme ve grafik filtresi boyutlardan içerir. Uyarı kuralı oluşturma bölmesi daha sonra bu ayarları değiştirmek mümkün olacaktır.

### <a name="to-create-a-new-alert-rule-click-new-alert-rule"></a>Yeni bir uyarı kuralı oluşturmak için tıklayın **yeni uyarı kuralı**

![Kırmızı renkte vurgulanmış yeni uyarı kuralı düğmesi](./media/metrics-charts/015.png)

Temel alınan ölçü boyutları ile uyarı kuralı oluşturma bölmesine özel uyarı kuralları oluşturma daha kolay hale getirmek için önceden doldurulmuş grafiğinizi alınır.

![Uyarı kuralı oluşturma](./media/metrics-charts/016.png)

Göz atın [makale](alerts-metric.md) ölçüm uyarılarını ayarlama hakkında daha fazla bilgi edinmek için.

## <a name="troubleshooting"></a>Sorun giderme

*Mychart üzerinde herhangi bir veri görmüyorum.*

* Filtreler bölmesinde tüm grafikler için geçerlidir. Bir grafikte odaklandığınız olsa da başka bir tüm verileri bırakan bir filtre ayarlanmış alamadık, emin olun.

* Farklı filtreler farklı grafikler üzerinde ayarlamak istiyorsanız bunları farklı dikey pencerelerinde oluşturma gibi ayrı sık kaydedin. İsterseniz, bunları birbirine yanı sıra görebilirsiniz, böylece bunları panoya sabitleyebilirsiniz.

* Bir grafik ölçüme göre tanımlı olmayan bir özelliğe göre segmentlere ayırmak, ardından olacaktır hiçbir şey grafiği. (Bölme) ayrılmasını temizlemeyi deneyin veya farklı bir özellik seçin.

## <a name="next-steps"></a>Sonraki adımlar

  Okuma [özel KPI panoları oluşturma](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards) Ölçümleriyle eyleme dönüştürülebilir panolar oluşturmak için en iyi uygulamalar hakkında bilgi edinmek için.

