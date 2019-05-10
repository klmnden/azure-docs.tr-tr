---
title: Azure Time Series Insights ortamınızı ölçek planlama | Microsoft Docs
description: Bu makalede, depolama kapasitesi, veri saklama, giriş kapasite, izleme ve iş olağanüstü durum kurtarma (BCDR) dahil olmak üzere bir Azure zaman serisi görüşleri ortamına planlarken en iyi uygulamaları izlemesi açıklar.
services: time-series-insights
ms.service: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: bf1f570319370fab99e2f52086bc81df259e3d35
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236520"
---
# <a name="plan-your-azure-time-series-insights-ga-environment"></a>Azure zaman serisi öngörüleri GA ortamınızı planlama

Bu makalede, beklenen giriş oranınız ve veri koruma gereksinimlerinizi temel alan Azure Time Series Insights genel kullanıma (GA) ortamınızı planlama açıklar.

## <a name="video"></a>Video

### <a name="learn-more-about-data-retention-in-azuretime-series-insights-and-how-to-plan-for-itbr"></a>Veri saklama AzureTime serisi öngörüleri ve bunun için planlama hakkında daha fazla bilgi edinin.</br>

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

## <a name="best-practices"></a>En iyi yöntemler

Dakikaya göre verilerinizi depolamak ne kadar süre ihtiyaç nasıl anında iletme beklediğiniz ne kadar veri biliyorsanız Time Series Insights ile çalışmaya başlamak için en iyisidir.  

Her iki zaman serisi öngörüleri SKU'ları için kapasite ve saklama hakkında daha fazla bilgi için bkz. [Time Series Insights fiyatlandırması](https://azure.microsoft.com/pricing/details/time-series-insights/).

Aşağıdaki öznitelikler için en uygun planı ortamı uzun vadeli başarı için göz önünde bulundurun:

- <a href="#understand-storage-capacity">Depolama kapasitesi</a>
- <a href="#understand-data-retention">Veri saklama süresi</a>
- <a href="#understand-ingress-capacity">Alma kapasitesi</a>
- <a href="#shape-your-events">Olaylarınızı şekillendirme</a>
- <a href="#ensure-you-have-reference-data">Başvuru verilerini yerinde kullandığınızdan emin olun</a>

## <a name="understand-storage-capacity"></a>Depolama kapasitesi anlama

Varsayılan olarak, Azure Time Series Insights, sağlanan depolama miktarı üzerinden veri (birim başına depolama birimleri kez miktarı) ve giriş korur.

## <a name="understand-data-retention"></a>Veri saklamayı anlama

Time Series Insights ortamınızın yapılandırabileceğiniz **veri saklama zamanı** en çok 400 gün saklama etkinleştirme ayarı. Time Series Insights, iki modu vardır, ortamınızı sağlamaktan iyileştiren varsa en güncel verileri (üzerinde varsayılan olarak) ve bekletme sınırları karşılanıyorsa, giriş varsa burada duraklatıldı sağlamak için en iyi duruma getirir, başka bir genel depolama kapasitesi ortam ulaşılır.  Saklama ve Azure portalında ortam yapılandırma sayfasında iki modlar arasında geçiş yapabilirsiniz.

En çok 400 gün veri saklama zaman serisi görüşleri ortamınıza yapılandırabilirsiniz.

### <a name="configure-data-retention"></a>Veri saklamayı yapılandırma

1. İçinde [Azure portalında](https://portal.azure.com), zaman serisi görüşleri ortamınızı seçin.

1. Üzerinde **zaman serisi görüşleri ortamı sayfa**altında **ayarları** başlığı seçin **yapılandırma**.

1. İçinde **veri saklama süresi (gün cinsinden)** kutusunda, 1. 400'e bir değer girin.

   [![Bekletmeyi yapılandır](media/environment-mitigate-latency/configure-retention.png)](media/environment-mitigate-latency/configure-retention.png#lightbox)

> [!TIP]
> Bir uygun veri bekletme ilkesi gözden geçirerek uygulama hakkında daha fazla bilgi [saklamayı yapılandırmak nasıl](./time-series-insights-how-to-configure-retention.md).

## <a name="understand-ingress-capacity"></a>Alma kapasitesi anlama

Diğer alan planlama odaklanmak için dakika başına ayırma türevi olan giriş kapasitesidir.

Azaltma açısından, paket boyutu 32 KB'lık bir ingressed veri paketi 32 olayları olarak kabul edilir, her 1 KB boyutlu. 32 KB izin verilen en uzun olay boyutudur; 32 KB'den büyük veri paketleri kesilir.

Aşağıdaki tabloda her SKU alma kapasitesi özetlenmektedir:

|SKU  |Olay sayısı / ay / birim  |Olay boyutu / ay / birim  |Olay sayısı / dakika / birim  | Boyut / dakika / birim   |
|---------|---------|---------|---------|---------|
|S1     |   30 milyon     |  30 GB     |  720    |  720 KB   |
|S2     |   300 milyon    |   300 GB   | 7,200   | 7200 KB  |

Tek bir ortamda 10 birim için bir S1 ve S2 SKU kapasitesi artırabilirsiniz. S2 için S1 ortamdan ya da bir S1 S2 ortamdan geçiremezsiniz.

Alma kapasitesi için ilk ay başına temelinde gerekli toplam giriş belirlemeniz gerekir. Daha sonra bu gecikme süresi azaltma ve burada bir rol oynar olduğu gibi dakika başına gereksinimleriniz nelerdir, belirleyin.

Time Series Insights "yakalama" veri girişinizi 24 saatten uzun bir depo varsa, yukarıda listelenen fiyatlar x 2 giriş hızında olabilir.

Örneğin, tek bir S1 SKU'ya ve giriş verileri 720 olay başına dakika ve ani bir hızda 1 saatten az 1440 olayları veya daha küçük bir hızda için varsa, olacaktır ortamınıza belirgin hiçbir gecikme süresi. Ancak, bir saatten fazla için dakika başına 1440 olayları aşarsanız, gecikme görselleştirilmiş ve ortamınızda bir sorgu için kullanılabilir olan veriler için büyük olasılıkla deneyimleyeceği.

Göndermeye beklediğiniz önceden ne kadar veri bilemeyebilirsiniz. Bu durumda, veri telemetri için bulabilirsiniz [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) ve [Azure Event Hubs](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) , Azure portalında. Bu telemetrinin ortamınızı sağlama belirlemenize yardımcı olabilir. Kullanım **ölçümleri** ve telemetrisini görüntülemek ilgili olay kaynağı Azure portalında sayfası. Olay kaynağı ölçümlerinizi anlarsanız, daha etkili bir şekilde planlamak ve zaman serisi görüşleri ortamınıza sağlayın.

### <a name="calculate-ingress-requirements"></a>Giriş gereksinimleri hesaplayın

- Giriş kapasitesidir, ortalama dakikalık ücretini ve ortamınızın kapasitenizi 1 saatten az x 2 eşdeğerdir, beklenen giriş işlemek için büyük olduğunu onaylayın.

- Giriş ani artışlar gerçekleşirse, son 1 saat, uzun artışının oranı, ortalama kullanın ve artışının oranı işleme yeteneğine sahip bir ortam sağlamak.

### <a name="mitigate-throttling-and-latency"></a>Azaltma ve gecikme süresini azaltın

Gecikme süresi azaltma ve önleme hakkında daha fazla bilgi için konusunu okuyun [azaltmak gecikme süresi ve azaltma](time-series-insights-environment-mitigate-latency.md).

## <a name="shape-your-events"></a>Olaylarınızı Şekil

TSI için olayları göndermek yolu sağlama ortamın boyutuna desteklediğinden emin olmak önemlidir (buna karşılık, ortama TSI okur kaç tane olay boyutu ve her bir olay boyutu eşleyebilirsiniz). Benzer şekilde, dilim ve verilerinizi sorgulanırken filtre isteyebilirsiniz öznitelikleri hakkında düşünmeniz önemlidir.

> [!TIP]
> JSON belgelerinde şekillendirme gözden [olay göndermeye](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-send-events).

## <a name="ensure-you-have-reference-data"></a>Başvuru verileri sahip olduğunuzdan emin olun

A **başvuru veri kümesi** olayları, olay kaynağınızdan büyütmek öğeleri oluşan bir koleksiyondur. Zaman serisi görüşleri giriş altyapısı, başvuru veri kümenizdeki her olay, olay kaynağınızdan karşılık gelen veri satırı ile birleştirir. Bu genişletilmiş olay daha sonra sorgu için kullanılabilir. Bu birleşim, başvuru veri kümenizde tanımlı birincil anahtar sütunları dayanır.

Başvuru verileri geriye dönük olarak katılmamış unutmayın. Bu, yalnızca mevcut ve gelecekteki giriş verileri eşleşen ve yapılandırılmış ve karşıya sonra başvuru tarih kümesine katılmış anlamına gelir.  TSI için çok fazla geçmiş veri göndermek ve yok karşıya yükleyin veya başvuru verileri TSI ilk olarak oluşturmak planlıyorsanız, gerekebilir sonra iş yeniden yapın (ipucu, eğlenceli değildir).  

TSI içindeki, başvuru verilerini yönetme oluşturma ve karşıya yükleme hakkında daha fazla bilgi edinmek için head bizim [başvuru veri kümesi belgeleri](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Oluşturmaya başlama [Azure portalında yeni bir zaman serisi görüşleri ortamı](time-series-insights-get-started.md).

- Bilgi edinmek için nasıl [Event Hub olay kaynağı ekleme](time-series-insights-how-to-add-an-event-source-eventhub.md) Time Series Insights için.

- Konusunu okuyun [bir IOT Hub olay kaynağı yapılandırmak](time-series-insights-how-to-add-an-event-source-iothub.md).
