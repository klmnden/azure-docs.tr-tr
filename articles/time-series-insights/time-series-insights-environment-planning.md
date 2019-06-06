---
title: Azure Time Series Insights ortamınızı ölçek planlama | Microsoft Docs
description: Bu makalede, Azure zaman serisi görüşleri ortamı planlarken, en iyi uygulamaları izlemesi açıklar. Kapsanan alanları, depolama kapasitesi, veri saklama, giriş kapasite, izleme ve iş sürekliliği ve olağanüstü durum kurtarma (BCDR) içerir.
services: time-series-insights
ms.service: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: 9d9f9b7fe7aafa927f023e1d4e30e079a26f7fab
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431030"
---
# <a name="plan-your-azure-time-series-insights-ga-environment"></a>Azure zaman serisi öngörüleri GA ortamınızı planlama

Bu makalede, beklenen giriş oranınız ve veri koruma gereksinimlerinizi temel alan Azure Time Series Insights genel kullanıma (GA) ortamınızı planlama açıklar.

## <a name="video"></a>Video

**Azure zaman serisi görüşleri ve bunun için planlama hakkında veri saklama hakkında daha fazla bilgi için bu videoyu**:<br /><br />

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

## <a name="best-practices"></a>En iyi uygulamalar

Dakika ve ne kadar süreyle verilerinizi depolamak gereken anında iletme beklediğiniz ne kadar veri biliyorsanız Time Series Insights ile çalışmaya başlamak için en iyisidir.  

Her iki zaman serisi öngörüleri SKU'ları için kapasite ve saklama hakkında daha fazla bilgi için bkz. [Time Series Insights fiyatlandırması](https://azure.microsoft.com/pricing/details/time-series-insights/).

Time Series Insights ortamınızı uzun vadeli başarı için en iyi şekilde planlamak için aşağıdaki öznitelikleri göz önünde bulundurun:

- <a href="#storage-capacity">Depolama kapasitesi</a>
- <a href="#data-retention">Veri saklama süresi</a>
- <a href="#ingress-capacity">Alma kapasitesi</a>
- <a href="#shape-your-events">Olaylarınızı şekillendirme</a>
- <a href="#ensure-that-you-have-reference-data">Başvuru verilerini yerinde kullandığınızdan emin olun</a>

## <a name="storage-capacity"></a>Depolama kapasitesi

Varsayılan olarak, zaman serisi görüşleri depolama, sağlamanız miktarına göre veri korur. (birimleri &#215; birim başına depolama alanı miktarı) ve giriş.

## <a name="data-retention"></a>Veri saklama

Değiştirebileceğiniz **veri saklama zamanı** Time Series Insights ortamınızı ayarlama. En çok 400 gün saklama etkinleştirebilirsiniz. 

Time Series Insights, iki modu vardır. Ortamınızı en güncel verileri sahip olduğundan emin olmak için bir mod en iyi duruma getirir. Bu mod, varsayılan olarak açıktır. 

Diğer moda bekletme sınırları karşılandığından emin olunması için en iyi duruma getirir. Ortam toplam depolama kapasitesinin karşılanırsa ikinci modunda Giriş duraklatıldı. 

Saklama ve Azure portalında ortam yapılandırma sayfasında iki modlar arasında geçiş yapabilirsiniz.

En çok 400 gün veri saklama zaman serisi görüşleri ortamınıza yapılandırabilirsiniz.

### <a name="configure-data-retention"></a>Veri saklamayı yapılandırma

1. İçinde [Azure portalında](https://portal.azure.com), zaman serisi görüşleri ortamınızı seçin.

1. İçinde **zaman serisi görüşleri ortamına** bölmesi altında **ayarları**seçin **yapılandırma**.

1. İçinde **veri saklama süresi (gün cinsinden)** kutusuna, 1 ile 400 arasında bir değer girin.

   [![Bekletmeyi yapılandır](media/environment-mitigate-latency/configure-retention.png)](media/environment-mitigate-latency/configure-retention.png#lightbox)

> [!TIP]
> Bir uygun veri saklama politikası uygulamadan hakkında daha fazla bilgi için bkz: [saklamayı yapılandırmak nasıl](./time-series-insights-how-to-configure-retention.md).

## <a name="ingress-capacity"></a>Alma kapasitesi

Alma kapasitesi Time Series Insights ortamınızı planlama odaklanmak için ikinci alanıdır. Giriş, dakika başına ayırma türevi kapasitesidir.

Azaltma açısından bakıldığında, bir paket boyutu 32 KB'lık bir ingressed veri paketi her 1 KB boyutunda 32 olayları olarak kabul edilir. 32 KB izin verilen en uzun olay boyutudur. 32 KB'den büyük veri paketleri kesilir.

Birim başına alma kapasitesi her zaman serisi öngörüleri SKU için aşağıdaki tabloda özetlenmiştir:

|SKU  |Ay başına olay sayısı  |Aylık olay boyutu  |Dakika başına olay sayısı  |Dakika başına olay boyutu  |
|---------|---------|---------|---------|---------|
|S1     |   30 milyon     |  30 GB     |  720    |  720 KB   |
|S2     |   300 milyon    |   300 GB   | 7,200   | 7200 KB  |

Tek bir ortamda 10 birim için bir S1 ve S2 SKU kapasitesi artırabilirsiniz. Bir S1 ortamından bir S2 geçiremezsiniz. Bir S2 ortamından bir S1 ila geçiremezsiniz.

Alma kapasitesi için ilk ay başına temelinde gerekli toplam giriş belirleyin. Daha sonra dakika başına gereksinimleriniz nelerdir belirleyin. 

Azaltma ve gecikme süresi dakika başına kapasite bir rol oynar. Veri girişinizi 24 saatten daha kısa süren bir ani değişiklik varsa, zaman serisi görüşleri "Yukarıdaki tabloda listelenen iki kez ücretler giriş hızında yakalayabilir".

Örneğin, bir tek S1 SKU'SUNDA, giriş verileri dakika başına 720 olay hızında varsa ve bir saatten kısa bir süre 1.440 olayları veya daha küçük bir hızda için veri hızı ani yoktur hiçbir fark edilebilir gecikme süresi, ortamınızda. Ancak, bir saatten fazla için dakika başına 1.440 olayları aşarsanız, olasılıkla görselleştirilmiş ve ortamınızda bir sorgu için kullanılabilir veri gecikme süreleri yaşayabilirsiniz.

Göndermeye beklediğiniz önceden ne kadar veri etmeyebilirsiniz. Bu durumda, veri telemetri için bulabilirsiniz [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) ve [Azure Event Hubs](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) Azure portal aboneliğinizdeki. Telemetriyi nasıl sağlayacağınızı ortamınızı belirlemenize yardımcı olabilir. Kullanım **ölçümleri** bölmesi ve telemetrisini görüntülemek ilgili olay kaynağı Azure portalında. Olay kaynağı ölçümlerinizi anlarsanız, daha etkili bir şekilde planlamak ve zaman serisi görüşleri ortamınıza sağlayın.

### <a name="calculate-ingress-requirements"></a>Giriş gereksinimleri hesaplayın

Giriş gereksinimlerinizi hesaplamak için:

- Giriş kapasite, ortalama dakikalık ücretini olduğunu ve ortamınız için bir saatten daha az kapasite iki kez eşdeğer beklenen girişinizi işleyebilecek büyüklükte olduğundan emin olun.

- Giriş ani son daha uzun süre 1 saat, artışının oranı, ortalama kullanan gerçekleşirse. Artışının oranı işleme yeteneğine sahip bir ortam sağlayın.

### <a name="mitigate-throttling-and-latency"></a>Azaltma ve gecikme süresini azaltın

Gecikme süresi azaltma ve önleme hakkında daha fazla bilgi için bkz: [azaltmak gecikme süresi ve azaltma](time-series-insights-environment-mitigate-latency.md).

## <a name="shape-your-events"></a>Olaylarınızı Şekil

Olayları zaman serisi görüşleri ortamı, sağlama boyutunu destekler gönderdiğiniz yolu emin olmak önemlidir. (Tersine, zaman serisi görüşleri okur kaç tane olay ortama boyutunu ve her bir olay boyutu eşleyebilirsiniz.) Dilim ve filtrelemek için kullanmak isteyebilirsiniz öznitelikleri hakkında düşünmeniz önemlidir, verileri sorguladığınızda.

> [!TIP]
> JSON belgelerinde şekillendirme gözden [olay göndermeye](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-send-events).

## <a name="ensure-that-you-have-reference-data"></a>Başvuru verileri sahip olduğunuzdan emin olun

A *başvuru veri kümesi* olayları, olay kaynağınızdan büyütmek öğeleri oluşan bir koleksiyondur. Zaman serisi görüşleri giriş altyapısı, her olay, olay kaynağınızdan karşılık gelen veri satırı ile başvuru Veri kümenizi birleştirir. Genişletilmiş olay daha sonra sorgu için kullanılabilir. Birleştirme dayanır **birincil anahtar** başvuru Veri kümenizi tanımlanan sütunları.

> [!NOTE]
> Başvuru verileri geriye dönük olarak katılmayan. Yalnızca geçerli ve gelecekteki giriş verileri eşleşen ve başvuru veri kümesi için yapılandırılmış ve karşıya sonra katılmış. Time Series Insights için çok miktarda geçmiş veri göndermek ve ilk karşıya yükleme yok ya da zaman serisi öngörüleri başvuru verileri oluşturun planlıyorsanız, iş Yinele gerekebilir (İpucu: eğlenceli değildir).  

Oluşturun, karşıya yükleme ve zaman serisi görüşleri başvuru verilerinizi yönetme hakkında daha fazla bilgi edinmek için müşterilerimize [başvuru veri kümesi belgeleri](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Oluşturmaya başlama [Azure portalında yeni bir zaman serisi görüşleri ortamı](time-series-insights-get-started.md).

- Bilgi edinmek için nasıl [Event Hubs olay kaynağı ekleme](time-series-insights-how-to-add-an-event-source-eventhub.md) Time Series Insights için.

- Konusunu okuyun [bir IOT Hub olay kaynağı yapılandırmak](time-series-insights-how-to-add-an-event-source-iothub.md).
