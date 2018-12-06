---
title: Azure İzleyici ölçüm Gezgini
description: Azure İzleyici ölçüm Gezgini'nde yeni özellikler hakkında bilgi edinin
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2017
ms.author: vitaly.gorbenko
ms.component: metrics
ms.openlocfilehash: d1cfaadd06d20a0f57d75cd43d00040c9e44c429
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52966033"
---
# <a name="azure-monitor-metrics-explorer"></a>Azure İzleyici ölçüm Gezgini

Azure İzleyici ölçüm Gezgini'ni grafikler çizme, görsel olarak eğilimleri ilişkilendirme ve ani araştırma sağlar ve düşüşler ölçümleri değerleri Microsoft Azure portalının bir bileşenidir. Ölçüm Gezgini çeşitli performans ve Azure'da barındırılan veya Azure izleme hizmetleri tarafından izlenen altyapı ve uygulamalar ile kullanılabilirlik sorunları araştırma için bir temel başlangıç noktasıdır. 

## <a name="what-are-metrics-in-azure"></a>Azure'da ölçümler nelerdir?

Microsoft azure'da ölçümleri ölçülen değerleri ve toplanan ve zaman içinde depolanmış olan sayıları oluşur. Standart (veya "platformu") ölçüm ve özel ölçüm vardır. Standart ölçümler, Azure platformu tarafından kendisi sağlanır. Standart ölçümler, Azure kaynaklarınızın durumunu ve kullanım istatistikleri yansıtır. Özel ölçüm kullanarak uygulamalarınızı tarafından Azure'a gönderilir ancak [özel olaylar için Application Insights API](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics). Özel ölçümler Application Insights kaynakları diğer uygulama belirli ölçümleri birlikte depolanır.

## <a name="how-do-i-create-a-new-chart"></a>Yeni bir grafik nasıl oluşturulur?

1. Azure portalını açın
2. Yeni Git **İzleyici** sekmesine tıklayın ve ardından **ölçümleri**.

   ![Ölçümler görüntüsü](./media/monitoring-metric-charts/0001.png)

3. **Ölçüm Seçici** otomatik olarak sizin için açık olacaktır. Bir kaynağı kendi ilgili ölçümleri görüntülemek için listeden seçin. Yalnızca kaynakları ölçümlerle listesinde gösterilir.

   ![Ölçümler görüntüsü](./media/monitoring-metric-charts/0002.png)

   > [!NOTE]
   >Ölçüm Gezgini çeken kaynakları Portal ayarlarında seçili abonelikler genelinde kullanıma birden fazla Azure aboneliğiniz varsa, filtre tarafından abonelik listesi ->. Değiştirmek için ekranın en üstünde Portal ayarları dişli simgesine tıklayın ve kullanmak istediğiniz abonelikleri seçin.

4. Bir ölçüm seçmeden önce bazı kaynak türleri için (depolama hesabı ve sanal makineler), seçmelisiniz bir **Namespace**. Her ad alanı, bu ad alanına ve diğer ad alanları için ilgili ölçümleri kendi kümesini gerçekleştirir.

   Örneğin, her bir Azure depolama alt Servisleri "BLOB", "Files", "Kuyrukları" ve tüm bölümleri depolama hesabının "Tablo" için ölçüler vardır. Ancak, "kuyruk mesaj sayısı" ölçüm doğal olarak "Sırası" subservice ve tüm diğer depolama hesabı alt servisleri için geçerlidir.

   ![Ölçümler görüntüsü](./media/monitoring-metric-charts/0003.png)

5. Listeden bir ölçüm seçin. İstediğiniz ölçümü kısmi adını biliyorsanız, kullanılabilir ölçümler filtrelenmiş bir listesini görmek için yazmak başlayabilirsiniz:

   ![Ölçümler görüntüsü](./media/monitoring-metric-charts/0004.png)

6. Bir ölçüm seçildiğinde, grafik, seçilen ölçüm için varsayılan toplama ile işlenir. Bu noktada yalnızca liste kutusundan tıklayabilirsiniz **ölçümleri Seçici** kapatmak için. Ayrıca isteğe bağlı olarak farklı bir toplama için grafiği geçiş yapabilirsiniz. Bazı ölçümler için toplama geçişi grafikte görmek istediğiniz değer seçmenize olanak sağlar. Örneğin, ortalama, minimum ve maksimum değerleri arasında geçiş yapabilirsiniz. 

7. Ölçüm Ekle simgesine tıklayarak ![Ölçüm simgesi](./media/monitoring-metric-charts/icon001.png) ve adım 3-6 yinelenen daha fazla ölçümleri aynı grafikte ekleyebilirsiniz.

   > [!NOTE]
   > Genellikle, bir grafikte ölçümleri farklı ölçü (yani "milisaniye" ve "kilobayt") veya önemli ölçüde farklı ölçeklendirme ile sahip istemezsiniz. Bunun yerine, birden çok grafik kullanmayı düşünün. Ölçüm Gezgini'nde birden çok grafik oluşturmak için Hesap Ekle düğmesine tıklayın.

## <a name="how-do-i-apply-filters-to-the-charts"></a>Filtreler grafikleri nasıl uygulayabilirim?

Boyutlarla ölçümleri gösteren grafikler için filtre uygulayabilirsiniz. "İşlem sayısı" ölçüm boyut varsa, "işlem yanıtı başarılı veya daha sonra bu boyutu üzerinde filtreleme başarısız olduğunu gösteren yanıt türü", bir grafik çizgisi için örneğin, çizim yalnızca başarılı (veya yalnızca başarısız) işlemleri. 

### <a name="to-add-a-filter"></a>Filtre eklemek için:

1. Filtre Ekle simgesine tıklayın ![filtre simgesi](./media/monitoring-metric-charts/icon002.png) grafiğin üstünde

2. Filtre uygulamak istediğiniz boyutu (özellik) seçin

   ![Ölçüm görüntüsü](./media/monitoring-metric-charts/0006.png)

3. (Bu örnekte filtreleme başarılı depolama işlemleri gösterilmiştir) grafik çizim, dahil etmek istediğiniz hangi boyut değerleri seçin:

   ![Ölçüm görüntüsü](./media/monitoring-metric-charts/0007.png)

4. Filtre değerleri belirledikten sonra filtre Seçici uzağa kapatmak için tıklayın. Artık grafik kaç depolama işlemi başarısız olmuş gösterir:

   ![Ölçüm görüntüsü](./media/monitoring-metric-charts/0008.png)

5. Aynı grafikleri birden fazla filtre uygulamak için 1-4 arası adımları tekrarlayabilirsiniz.

## <a name="how-do-i-segment-a-chart"></a>Bir grafiğin nasıl segmentlere?

Bir ölçüm bölme ölçütü: ölçüm karşılaştırma birbirleriyle nasıl farklı parçalarını görselleştirmek için boyut ve boyutun harici segmentleriyle. 

### <a name="to-segment-a-chart"></a>Bir grafik segmentlere ayırmak için:

1. Gruplama ekleme simgesine tıklayın  ![Ölçüm görüntüsü](./media/monitoring-metric-charts/icon003.png) grafiğin üstünde.
 
   > [!NOTE]
   > Tek bir gruplandırma ancak birden çok filtre herhangi tek grafikte olabilir.

2. Grafiğinizi segmentlere ayırmak istediğiniz bir boyut seçin: 

   ![Ölçüm görüntüsü](./media/monitoring-metric-charts/0010.png)

   Şimdi grafik, artık her bir kesim boyutu için birden fazla satır gösterir:

   ![Ölçüm görüntüsü](./media/monitoring-metric-charts/0012.png)

3. Liste kutusundan tıklayın **gruplandırma Seçici** kapatmak için.

   > [!NOTE]
   > Filtreleme ve gruplandırma aynı boyutta hem senaryonuz için ilgisi olmayan ve grafikleri okunmalarını kolaylaştırmak segmentleri gizlemek için kullanın.

## <a name="how-do-i-lock-lower-and-upper-boundaries-of-the-chart-y-axis"></a>Grafik y ekseni alt ve üst sınırları nasıl kilitleme?

Grafiğin büyük değerler daha küçük dalgalanmaları gösterdiğinde y ekseni aralığını kilitleme önemli hale gelir. 

Örneğin, başarılı istek hacmi % %99,99 %99,5 açılır, hizmet kalitesi önemli azalmaya temsil edebilir. Ancak, bir küçük sayısal değeri dalgalanma tercihinize zor veya imkansız bile varsayılan hesap ayarlarından olacaktır. Bu durumda bu küçük bırakma daha belirgin hale getirir %99 grafiğinin en düşük sınır kilitlenemedi. 

Başka bir kullanılabilir bellek bir dalgalanma burada değeri teknik olarak hiçbir zaman 0 ulaşacak örnektir. Daha yüksek bir değer aralığına düzeltme düşme kullanılabilir bellek. nokta kolaylaştırmak. 

Y ekseni aralığını denetlemek için kullanma "..." Grafik menü ve seçin **grafiği Düzenle** grafiği ayarları Gelişmiş erişim için. Y ekseni aralığını bölümündeki değerleri değiştirin veya kullanın **otomatik** varsayılanlara geri düğmesi.

![Ölçüm görüntüsü](./media/monitoring-metric-charts/0013.png)

> [!WARNING]
> Çeşitli izlemek grafiklerin y ekseni sınırlarını kilitleme sayar veya toplayan bir dönem boyunca zaman (ve dolayısıyla kullanım sayısı, toplam, minimum veya maksimum toplamalar) genellikle otomatik varsayılanlara güvenmek yerine bir sabit zaman ayrıntı düzeyi belirterek gerektirir. Bu gereklidir çünkü zaman ayrıntı düzeyi tarayıcı penceresini yeniden boyutlandırma veya başka bir ekran çözünürlüğünü giden kullanıcı tarafından otomatik olarak değiştirildiğinde grafiklerde değerlerini değiştirin. Elde edilen grafiğin y ekseni aralığını oluşan geçerli seçimi geçersiz kılmalarını görünümünü zaman ayrıntı düzeyi efektleri değiştirin.

## <a name="how-do-i-pin-charts-to-dashboards"></a>Nasıl grafikleri panolara sabitleyebilirsiniz?

Grafikleri yapılandırdıktan sonra yeniden, büyük olasılıkla diğer izleme telemetri bağlamında görüntülemek veya takımınızla paylaşmak için Pano eklemek isteyebilirsiniz. 

Yapılandırılmış bir grafik bir panoya sabitlemek için:

Grafiğinizi yapılandırdıktan sonra tıklayarak **grafik Eylemler** menüsü sağ üst köşe grafiğin ve tıklayın **panoya Sabitle**.

![Ölçüm görüntüsü](./media/monitoring-metric-charts/0013.png)

## <a name="next-steps"></a>Sonraki adımlar

  Okuma [özel KPI panoları oluşturma](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards) Ölçümleriyle eyleme dönüştürülebilir panolar oluşturmak için en iyi uygulamalar hakkında bilgi edinmek için.