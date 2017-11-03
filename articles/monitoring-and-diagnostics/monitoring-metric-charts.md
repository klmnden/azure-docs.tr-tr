---
title: "Azure İzleyici ölçüm Gezgini | Microsoft Docs"
description: "Azure İzleyici ölçüm Gezgini yeni özellikler hakkında bilgi edinin"
author: vgorbenko
manager: Victor.Mushkatin
editor: mrbullwinkle
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 2397596a-071f-4d49-8893-bec5f735bd7b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.author: vitaly.gorbenko
ms.openlocfilehash: 537dd6d64fe49093dd73d8040cde5a9153a7bd5c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-monitor-metrics-explorer"></a>Azure İzleyici ölçüm Gezgini

Bu nasıl yapılır genel önizlemede olan ileri nesil Azure İzleyici ölçümleri grafik deneyimi açıklanmıştır. Yeni deneyimi işleme grafikleri çok boyutlu ölçümleri hem temel ölçümleri hiçbir boyutlarla destekler. Ölçümleri farklı kaynak türleri, birden çok kaynak grupları ve abonelikleri kaplama grafikler çizebilirsiniz. Çok boyutlu ölçümleri grafikler, boyut filtreleri uygulayarak yanı sıra gruplandırma özelleştirilebilir. Özelleştirilmiş grafikler dahil olmak üzere herhangi bir grafiğin panolarına sabitlenmiş.

Yalnızca hiçbir boyutlarla temel ölçümleri destekleyen eski deneyimi hakkında bilgi arıyorsanız Lütfen bölüm başlıklı "ölçümleri portalı üzerinden erişim" bölümüne bakın [Microsoft Azure ölçümleri genel bakış Kılavuzu](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics).

## <a name="what-is-azure-monitor-metrics-explorer"></a>Azure İzleyici ölçüm Gezgini nedir?

Azure İzleyici ölçüm Gezgini grafikleri Çizdirmek, görsel olarak eğilimleri bağıntı ve ani araştırma sağlar ve ölçümleri değerleri dips Microsoft Azure portal'ın bir bileşenidir. Ölçüm Gezgini incelemektedir çeşitli performans ve kullanılabilirlik sorunları, uygulamalar ve Azure üzerinde barındırılan veya Azure İzleyici Hizmetleri tarafından izlenen altyapınız için bir temel başlangıç noktasıdır. 

## <a name="what-are-metrics-in-azure"></a>Azure ölçümlerini nelerdir?

Microsoft Azure ölçümlerini ölçülen değerleri ve zaman içinde depolanan ve toplanan sayılar oluşur. Standart (veya "platform") ölçümleri ve özel ölçümleri vardır. Standart ölçümleri size Azure platformu tarafından kendisini sağlanır. Standart ölçümleri Azure kaynaklarınızın sistem durumunu ve kullanım istatistiklerini yansıtır. Özel ölçümleri kullanarak uygulamalarınızı tarafından Azure'a gönderilir ancak [özel olaylar için Application Insights API'si](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics). Özel ölçümleri Application Insights kaynakları diğer uygulama belirli ölçümleri birlikte depolanır.

## <a name="what-are-multi-dimensional-metrics"></a>Çok boyutlu ölçümleri nelerdir?

Çoğu Azure'nın kaynakları artık çok boyutlu ölçümleri ortaya çıkarır. Bu ölçümler bir veya daha çok adlandırılmış boyutlar için değerleri birden fazla dizi izler. Örneğin, bir ölçüm "kullanılabilir disk alanı" değerler "C:", "Sürücü" adlı bir boyut ya da kullanılabilir disk alanı her sürücü için veya tüm sürücüler üzerinde tek tek görüntüleme belirleyebilmesini "D:" olabilir. 

Aşağıdaki örnekte, "Ağ verimliliği" adlı bir kuramsal ölçümü için iki veri kümesi gösterilmektedir. İlk veri kümesi herhangi bir boyutu vardır. İkinci veri kümesi, iki boyutları, "IP adresi" ve "Yönünü" değerlerle gösterir:

### <a name="network-throughput"></a>Ağ verimliliği
(Bu ölçüm hiç boyut yoktur)

 |zaman damgası        | Ölçüm değeri | 
   | ------------- |:-------------| 
   | 8/9/2017 8:14 | 1,331.8 KB/sn | 
   | 8/9/2017 8:15 | 1,141.4 KB/sn |
   | 8/9/2017 8:16 | 1,110.2 KB/sn |

Yanıt temel soru ister yalnızca "Benim ağ verimliliği belirli bir zamanda neydi?" Bu boyutlu olmayan ölçüm olabilir

### <a name="network-throughput--two-dimensions-ip-and-direction"></a>Ağ verimliliği + iki boyutu ("IP" ve "Yönünü")

| zaman damgası          | Boyut "IP" | Boyut "Yönünü" | Ölçüm değeri| 
   | ------------- |:-----------------|:------------------- |:-----------|  
   | 8/9/2017 8:14 | IP "192.168.5.2" = | Yön "Gönderme" =    | 646.5 KB/sn |
   | 8/9/2017 8:14 | IP "192.168.5.2" = | Yön "Alma" = | 420.1 KB/sn |
   | 8/9/2017 8:14 | IP "10.24.2.15" =  | Yön "Gönderme" =    | 150.0 KB/sn | 
   | 8/9/2017 8:14 | IP "10.24.2.15" =  | Yön "Alma" = | 115.2 KB/sn |
   | 8/9/2017 8:15 | IP "192.168.5.2" = | Yön "Gönderme" =    | 515.2 KB/sn |
   | 8/9/2017 8:15 | IP "192.168.5.2" = | Yön "Alma" = | 371.1 KB/sn |
   | 8/9/2017 8:15 | IP "10.24.2.15" =  | Yön "Gönderme" =    | 155.0 KB/sn |
   | 8/9/2017 8:15 | IP "10.24.2.15" =  | Yön "Alma" = | 100.1 KB/sn |

Bu ölçüm "her IP adresi için bir ağ verimliliği neydi?" ve "ne kadar veri karşı gönderildiği alınan?" gibi soruları yanıtlamak Çok boyutlu ölçümleri boyutlu olmayan ölçümleri karşılaştırıldığında ek Analitik ve tanılama değeri taşır. 

## <a name="how-do-i-create-a-new-chart"></a>Yeni bir grafik nasıl oluşturulur?

   > [!NOTE]
   > Bazı eski ölçümleri deneyimi özellikleri henüz yeni ölçümleri Explorer'ın kullanılabilir değil. Yeni deneyimi önizlemede olsa da, Azure İzleyicisi'nin eski (boyut olmayan) ölçümleri görünümü kullanmaya devam edebilirsiniz. 

1. Azure portalını açın
2. Yeni gidin **İzleyici** sekmesini tıklatın ve ardından **ölçümleri (Önizleme)**.

   ![Ölçümleri Önizleme görünümü](./media/monitoring-metric-charts/001.png)

3. **Ölçüm Seçici** otomatik olarak sizin için açık olacaktır. İlişkili ölçümleri görüntülemek için listeden bir kaynak seçin. Yalnızca ölçümleri kaynaklarla listesinde gösterilir.

   ![Ölçümleri Önizleme görünümü](./media/monitoring-metric-charts/002.png)

   > [!NOTE]
   >Birden fazla Azure aboneliğiniz varsa, ölçüm Gezgini çeken kaynakları Portal ayarlarında seçilmiş olan tüm abonelikler arasında çıkışı Filtresi tarafından abonelik listesi ->. Değiştirmek için ekranın üstünde Portalı Ayarları dişli simgesine tıklayın ve kullanmak istediğiniz abonelikleri seçin.

4. Ölçüm seçmeden önce bazı kaynak türleri için (yani depolama hesapları ve sanal makineler) seçmeniz gerekir bir **alt hizmet**. Her alt hizmet yalnızca bu alt hizmetine ve diğer alt hizmetlerine ilgili ölçümleri kendi kümesini taşır.

   Örneğin, her Azure Storage ölçümleri alt Servisleri "BLOB'lar", "Dosyalar", "Sıraları" ve depolama hesabı bölümlerdir "Tablo" için vardır. Ancak, "sıraya ileti sayısı" ölçüm doğal olarak "Sırası" subservice ve herhangi diğer depolama hesabı alt Servisleri geçerlidir.

   ![Ölçümleri Önizleme görünümü](./media/monitoring-metric-charts/003.png)

5. Bir ölçümü listeden seçin. İstediğiniz ölçüm kısmi adını biliyorsanız, içinde kullanılabilir ölçümler filtre uygulanmış bir listesini görmek için yazarak başlatabilirsiniz:

   ![Ölçümleri Önizleme görünümü](./media/monitoring-metric-charts/004.png)

6. Ölçüm seçtikten sonra grafik seçili ölçümü için varsayılan toplama ile işlemez. Bu noktada yalnızca merkezden tıklatabilirsiniz **ölçümleri Seçici** kapatmak için. Bu gibi durumlarda, grafik Ayrıca isteğe bağlı olarak farklı bir toplama geçebilirsiniz. Bazı ölçümleri toplama geçiş grafikte görmek istediğiniz hangi değerin seçmenize olanak sağlar. Örneğin, ortalama, minimum ve maksimum değerleri arasında geçiş yapabilirsiniz. 

7. Ölçüm Ekle simgesine tıklayarak ![Ölçüm simgesi](./media/monitoring-metric-charts/icon001.png) ve adımları 3-6 yinelenen aynı grafik üzerinde daha fazla ölçümleri ekleyebilirsiniz.

   > [!NOTE]
   > Genellikle bir grafikte ölçümleri farklı ölçü (yani "milisaniye" ve "kilobayt") veya önemli ölçüde farklı ölçek sahip istemezsiniz. Bunun yerine, birden çok grafik kullanmayı düşünün. Ölçümleri Gezgini'nde birden çok grafik oluşturmak için Grafik Ekle düğmesine tıklayın.

## <a name="how-do-i-apply-filters-to-the-charts"></a>Grafiklere nasıl filtreleri uygulansın mı?

Ölçümleri boyutlarla gösteren grafikleri için filtreler uygulayabilirsiniz. "İşlem sayısı" ölçüm boyut varsa, "işlemleri yanıttan başarılı veya bu boyuta göre filtreleme başarısız olup olmadığını gösteren yanıt türü", grafik satır için örneğin, çizim yalnızca başarılı (veya yalnızca başarısız) işlemleri. 

### <a name="to-add-a-filter"></a>Filtre eklemek için:

1. Filtre Ekle simgesine tıklayın ![filtre simgesi](./media/monitoring-metric-charts/icon002.png) grafiğin

2. Filtre uygulamak istediğiniz hangi boyut (özellik) seçin

   ![Ölçüm görüntüsü](./media/monitoring-metric-charts/006.png)

3. (Bu örnekte başarılı depolama işlemleri filtreleme gösterir) grafik Çizdirmek zaman dahil etmek istediğiniz hangi boyut değerleri seçin:

   ![Ölçüm görüntüsü](./media/monitoring-metric-charts/007.png)

4. Filtre değerleri seçtikten sonra kapatmak için filtre Seçici çıktığınızda tıklatın. Şimdi ne kadar depolama işlemleri başarısız olmuş grafik gösterir:

   ![Ölçüm görüntüsü](./media/monitoring-metric-charts/008.png)

5. Aynı grafiklere birden fazla filtre uygulamak için 1-4 arası adımları tekrar edebilirsiniz.

## <a name="how-do-i-segment-a-chart"></a>Bir grafik nasıl segmentlere?

Boyut ölçüm karşılaştırma birbirleriyle nasıl farklı parçalarını görselleştirmek için bir ölçüm bölme ve boyutun sınırdaki segmentleriyle. 

### <a name="to-segment-a-chart"></a>Bir grafik segmentlere ayırmak için:

1. Ekleme gruplandırma simgesine tıklayın  ![Ölçüm görüntüsü](./media/monitoring-metric-charts/icon003.png) grafiğin.
 
   > [!NOTE]
   > Tek bir grafikte yalnızca bir gruplama ancak birden fazla filtreye sahip olabilir.

2. Grafiğinizi segmentlere ayırmak istediğiniz boyutu seçin: 

   ![Ölçüm görüntüsü](./media/monitoring-metric-charts/010.png)

   Artık grafik şimdi boyutunun her segment için birden çok satırları gösterir:

   ![Ölçüm görüntüsü](./media/monitoring-metric-charts/012.png)

3. Merkezden tıklatın **gruplandırma Seçici** kapatmak için.

   > [!NOTE]
   > Filtreleme ve aynı boyutun gruplandırılması senaryonuz için önemli değildir ve grafikler okunmasını kolaylaştırmak segmentleri gizlemek için kullanın.

## <a name="how-do-i-pin-charts-to-dashboards"></a>Panolar grafiklere nasıl PIN?

Grafikler yapılandırdıktan sonra böylece yeniden, büyük olasılıkla diğer izleme telemetri bağlamında görüntülemek veya takımınızla paylaşmak için panolar eklemek isteyebilirsiniz. 

Yapılandırılmış bir grafik bir panoya sabitlemek için:

Grafiğinizi yapılandırdıktan sonra tıklayın **grafik Eylemler** menüsünde sağ üst köşe grafiğin ve tıklatın **panoya Sabitle**.

   ![Ölçüm görüntüsü](./media/monitoring-metric-charts/013.png)

## <a name="next-steps"></a>Sonraki adımlar

  Okuma [özel KPI panolar oluşturma](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards) Ölçümleriyle tıklatılabilir panoları oluşturmak için en iyi uygulamalar hakkında bilgi edinmek için.