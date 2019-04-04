---
title: Azure İzleyici ölçüm Gezgini
description: Azure İzleyici ölçüm Gezgini'nde yeni özellikler hakkında bilgi edinin
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: 08ae74bcd9ee0a7cf5e0fb6d38758b1429c39145
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916351"
---
# <a name="azure-monitor-metrics-explorer"></a>Azure İzleyici ölçüm Gezgini

Azure İzleyici ölçüm Gezgini'ni çizim grafikleri, görsel olarak eğilimleri ilişkilendirme ve ani araştırma sağlar ve düşüşler ölçümleri değerleri Microsoft Azure portalının bir bileşenidir. Ölçüm Gezgini çeşitli performans ve Azure'da barındırılan veya Azure izleme hizmetleri tarafından izlenen altyapı ve uygulamalar ile kullanılabilirlik sorunları araştırma için bir temel başlangıç noktasıdır.

## <a name="metrics-in-azure"></a>Azure ölçümleri

[Azure İzleyicisi'nde ölçümler](data-platform-metrics.md) ölçülen değerleri ve toplanan ve zaman içinde depolanmış olan sayıları dizi. Standart (veya "platformu") ölçüm ve özel ölçüm vardır. Standart ölçümler, Azure platformu tarafından kendisi sağlanır. Standart ölçümler, Azure kaynaklarınızın durumunu ve kullanım istatistikleri yansıtır. Özel ölçüm kullanarak uygulamalarınızı tarafından Azure'a gönderilir ancak [özel olaylar ve ölçümler için Application Insights API](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics), [Windows Azure tanılama (WAD) uzantısı](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-overview), ya da [Azure REST API izleme](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-store-custom-rest-api).

## <a name="create-a-new-chart"></a>Yeni bir grafik oluşturun

1. Azure portalını açın
2. Yeni Git **İzleyici** sekmesine tıklayın ve ardından **ölçümleri**.

   ![Ölçümler görüntüsü](./media/metrics-charts/00001.png)

3. **Ölçüm Seçici** otomatik olarak sizin için açık olacaktır. Bir kaynağı kendi ilgili ölçümleri görüntülemek için listeden seçin. Yalnızca kaynakları ölçümlerle listesinde gösterilir.

   ![Ölçümler görüntüsü](./media/metrics-charts/00002.png)

   > [!NOTE]
   >Ölçüm Gezgini çeken kaynakları Portal ayarlarında seçili abonelikler genelinde kullanıma birden fazla Azure aboneliğiniz varsa, filtre tarafından abonelik listesi ->. Değiştirmek için ekranın en üstünde Portal ayarları dişli simgesine tıklayın ve kullanmak istediğiniz abonelikleri seçin.

4. Bir ölçüm seçmeden önce bazı kaynak türleri için (depolama hesabı ve sanal makineler), seçmelisiniz bir **Namespace**. Her ad alanı, bu ad alanına ve diğer ad alanları için ilgili ölçümleri kendi kümesini gerçekleştirir.

   Örneğin, her bir Azure depolama alt Servisleri "BLOB", "Files", "Kuyrukları" ve tüm bölümleri depolama hesabının "Tablo" için ölçüler vardır. Ancak, "kuyruk mesaj sayısı" ölçüm doğal olarak "Sırası" subservice ve tüm diğer depolama hesabı alt servisleri için geçerlidir.

   ![Ölçümler görüntüsü](./media/metrics-charts/00003.png)

5. Listeden bir ölçüm seçin. İstediğiniz ölçümü kısmi adını biliyorsanız, kullanılabilir ölçümler filtrelenmiş bir listesini görmek için yazmak başlayabilirsiniz:

   ![Ölçümler görüntüsü](./media/metrics-charts/00004.png)

6. Bir ölçüm seçildiğinde, grafik, seçilen ölçüm için varsayılan toplama ile işlenir. Bu noktada yalnızca liste kutusundan tıklayabilirsiniz **ölçümleri Seçici** kapatmak için. Ayrıca isteğe bağlı olarak farklı bir toplama için grafiği geçiş yapabilirsiniz. Bazı ölçümler için toplama geçişi grafikte görmek istediğiniz değer seçmenize olanak sağlar. Örneğin, ortalama, minimum ve maksimum değerleri arasında geçiş yapabilirsiniz. 

7. Tıklayarak **ölçüm Ekle** ve 3-6 adımları yinelemekten daha fazla ölçümleri aynı grafikte ekleyebilirsiniz.

   > [!NOTE]
   > Genellikle, bir grafikte ölçümleri farklı ölçü (yani "milisaniye" ve "kilobayt") veya önemli ölçüde farklı ölçeklendirme ile sahip istemezsiniz. Bunun yerine, birden çok grafik kullanmayı düşünün. Ölçüm Gezgini'nde birden çok grafik oluşturmak için Hesap Ekle düğmesine tıklayın.

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

## <a name="multiple-metrics-and-charts"></a>Birden çok ölçüm ve grafikler

Çoklu ölçümler çizim veya aynı anda birden çok ölçüm grafikleri Göster grafikleri de oluşturabilirsiniz. Bu işlev sağlar:

- başka bir performanstaki ilgili ölçümleri aynı grafikte bir değeri görmek için ilgili
- farklı ölçü yakın ölçümlerini görüntüle
- görsel olarak toplama ve birden çok kaynaklardan ölçümleri karşılaştırın

Örneğin, 5 depolama hesapları kullandığınız ve bunlar arasında tüketilen toplam ne kadar alan öğrenmek istiyorsanız, tek tek ve tüm değerlerin toplamını belirli noktalarda süresini gösterir (Yığılmış) alan grafiği oluşturabilirsiniz.

### <a name="multiple-metrics-on-a-chart"></a>Grafikte birden çok ölçümleri

İlk olarak, [yeni bir grafik oluşturun](#create-a-new-chart). Tıklayın **ölçüm Ekle** ve aynı grafiğe başka bir ölçüm eklemek için adımları yineleyin.

### <a name="multiple-charts"></a>Birden fazla grafiği

Tıklayın **Ekle grafik** ve farklı bir ölçümü ile başka bir grafik oluşturun.

### <a name="order-or-delete-multiple-charts"></a>Sipariş veya birden fazla Grafiği Sil

Sipariş ya da birden fazla grafiği silmek için üç noktayı tıklayın ( **...**  ) grafik menüsünü açın ve uygun menü öğesi, sembol **Yukarı Taşı**, **Aşağı Taşı**, veya **Sil**.

## <a name="apply-splitting-to-a-chart"></a>Bir grafiği bölme Uygula

Bir ölçüm bölme ölçütü: ölçüm karşılaştırma birbirleriyle nasıl farklı parçalarını görselleştirmek için boyut ve boyutun harici segmentleriyle.

### <a name="apply-splitting"></a>Bölme uygula

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

![Uyarı kuralı oluştur](./media/metrics-charts/016.png)

Göz atın [makale](alerts-metric.md) ölçüm uyarılarını ayarlama hakkında daha fazla bilgi edinmek için.

## <a name="troubleshooting"></a>Sorun giderme

*Mychart üzerinde herhangi bir veri görmüyorum.*

* Filtreler bölmesinde tüm grafikler için geçerlidir. Bir grafikte odaklandığınız olsa da başka bir tüm verileri bırakan bir filtre ayarlanmış alamadık, emin olun.

* Farklı filtreler farklı grafikler üzerinde ayarlamak istiyorsanız bunları farklı dikey pencerelerinde oluşturma gibi ayrı sık kaydedin. İsterseniz, bunları birbirine yanı sıra görebilirsiniz, böylece bunları panoya sabitleyebilirsiniz.

* Bir grafik ölçüme göre tanımlı olmayan bir özelliğe göre segmentlere ayırmak, ardından olacaktır hiçbir şey grafiği. (Bölme) ayrılmasını temizlemeyi deneyin veya farklı bir özellik seçin.

## <a name="next-steps"></a>Sonraki adımlar

  Okuma [özel KPI panoları oluşturma](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards) Ölçümleriyle eyleme dönüştürülebilir panolar oluşturmak için en iyi uygulamalar hakkında bilgi edinmek için.

