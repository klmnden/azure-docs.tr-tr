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
ms.date: 11/15/2017
ms.openlocfilehash: 2c06463d95467543a426079addf981aa42d53eb6
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39630645"
---
# <a name="plan-your-azure-time-series-insights-environment"></a>Azure Time Series Insights ortamınızı planlama

Bu makalede, beklenen giriş oranınız ve veri koruma gereksinimlerinizi temel alan Azure Time Series Insights ortamınızı planlama açıklar.

## <a name="best-practices"></a>En iyi uygulamalar

Dakikaya göre verilerinizi depolamak ne kadar süre ihtiyaç nasıl anında iletme beklediğiniz ne kadar veri biliyorsanız Time Series Insights ile çalışmaya başlamak için en iyisidir.  

Her iki zaman serisi öngörüleri SKU'ları için kapasite ve saklama hakkında daha fazla bilgi için bkz. [Time Series Insights fiyatlandırması](https://azure.microsoft.com/pricing/details/time-series-insights/).

Aşağıdaki öznitelikler için en uygun planı ortamı uzun vadeli başarı için göz önünde bulundurun: 
- Depolama kapasitesi
- Veri saklama süresi
- Alma kapasitesi 
- Olaylarınızı şekillendirme
- Başvuru verilerini yerinde kullandığınızdan emin olun

## <a name="understand-storage-capacity"></a>Depolama kapasitesi anlama
Varsayılan olarak, zaman serisi görüşleri, sağlanan depolama miktarı üzerinden veri (birim başına depolama birimleri kez miktarı) ve giriş korur.

## <a name="understand-data-retention"></a>Veri saklamayı anlama
Time Series Insights ortamınızın yapılandırabileceğiniz **veri saklama zamanı** en çok 400 gün saklama etkinleştirme ayarı.  Time Series Insights, iki modu vardır, ortamınızı sağlamaktan iyileştiren varsa en güncel verileri (üzerinde varsayılan olarak) ve bekletme sınırları karşılanıyorsa, giriş varsa burada duraklatıldı sağlamak için en iyi duruma getirir, başka bir genel depolama kapasitesi ortam ulaşılır.  Saklama ve Azure portalında ortam yapılandırma sayfasında iki modlar arasında geçiş yapabilirsiniz.

En çok 400 gün veri saklama zaman serisi görüşleri ortamınıza yapılandırabilirsiniz.

## <a name="configure-data-retention"></a>Veri saklamayı yapılandırma

1. İçinde [Azure portalında](https://portal.azure.com), zaman serisi görüşleri ortamınızı seçin.

2. Üzerinde **zaman serisi görüşleri ortamı sayfa**altında **ayarları** başlığı seçin **yapılandırma**. 

3. İçinde **veri saklama süresi (gün cinsinden)** kutusunda, 1. 400'e bir değer girin.

   ![Bekletmeyi yapılandır](media/environment-mitigate-latency/configure-retention.png)

## <a name="understand-ingress-capacity"></a>Alma kapasitesi anlama

Diğer alan planlama odaklanmak için dakika başına ayırma türevi olan giriş kapasitesidir. 

Azaltma açısından, paket boyutu 32 KB'lık bir ingressed veri paketi 32 olayları olarak kabul edilir, her 1 KB boyutlu. 32 KB izin verilen en uzun olay boyutudur; 32 KB'den büyük veri paketleri kesilir.

Aşağıdaki tabloda her SKU alma kapasitesi özetlenmektedir:

|SKU  |Ayda birim başına olay sayısı  |Her ay, birim başına olay boyutu  |Dakikada birim başına olay sayısı  | Birim başına dakikada boyutu   |
|---------|---------|---------|---------|---------|
|S1     |   30 milyon     |  30 GB     |  700    |  700 KB   |
|S2     |   300 milyon    |   300 GB   | 7,000   | 7000 KB  |

Tek bir ortamda 10 birim için bir S1 ve S2 SKU kapasitesi artırabilirsiniz. S2 için S1 ortamdan ya da bir S1 S2 ortamdan geçiremezsiniz. 

Alma kapasitesi için ilk ay başına temelinde gerekli toplam giriş belirlemeniz gerekir. Daha sonra bu gecikme süresi azaltma ve burada bir rol oynar olduğu gibi dakika başına gereksinimleriniz nelerdir, belirleyin.

Time Series Insights "yakalama" veri girişinizi 24 saatten uzun bir depo varsa, yukarıda listelenen fiyatlar x 2 giriş hızında olabilir. 

Örneğin, tek bir S1 SKU'ya ve giriş verileri 700 olay başına dakika ve ani bir hızda 1 saatten kısa bir süre bir hızda 1400 olayların ya da daha az olan, olacaktır ortamınıza belirgin hiçbir gecikme süresi. Ancak, bir saatten fazla için dakika başına 1400 olayları aşarsanız, gecikme görselleştirilmiş ve ortamınızda bir sorgu için kullanılabilir olan veriler için büyük olasılıkla deneyimleyeceği. 

Göndermeye beklediğiniz önceden ne kadar veri bilemeyebilirsiniz. Bu durumda, veri telemetri için bulabilirsiniz [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) ve [Azure Event Hubs](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) , Azure portalında. Bu telemetrinin ortamınızı sağlama belirlemenize yardımcı olabilir. Kullanım **ölçümleri** ve telemetrisini görüntülemek ilgili olay kaynağı Azure portalında sayfası. Olay kaynağı ölçümlerinizi anlarsanız, daha etkili bir şekilde planlamak ve zaman serisi görüşleri ortamınıza sağlayın.

### <a name="calculate-ingress-requirements"></a>Giriş gereksinimleri hesaplayın

- Giriş kapasitesidir, ortalama dakikalık ücretini ve ortamınızın kapasitenizi 1 saatten az x 2 eşdeğerdir, beklenen giriş işlemek için büyük olduğunu onaylayın.

- Giriş ani artışlar gerçekleşirse, son 1 saat, uzun artışının oranı, ortalama kullanın ve artışının oranı işleme yeteneğine sahip bir ortam sağlamak.
 
### <a name="mitigate-throttling-and-latency"></a>Azaltma ve gecikme süresini azaltın

Gecikme süresi azaltma ve önleme hakkında daha fazla bilgi için bkz: [azaltmak gecikme süresi ve azaltma](time-series-insights-environment-mitigate-latency.md). 

## <a name="shaping-your-events"></a>Olaylarınızı şekillendirme
TSI için olayları göndermek yolu sağlama ortamın boyutuna desteklediğinden emin olmak önemlidir (buna karşılık, ortama TSI okur kaç tane olay boyutu ve her bir olay boyutu eşleyebilirsiniz).  Benzer şekilde, dilim ve verilerinizi sorgulanırken filtre isteyebilirsiniz öznitelikleri hakkında düşünmeniz önemlidir.  Bunu aklınızda bölümünü şekillendirme JSON gözden geçirme öneririz bizim *olayları göndermek* belgeleri [belgeleri] (https://docs.microsoft.com/azure/time-series-insights/time-series-insights-send-events).  Bu, sayfasının en altına doğru olur.  

## <a name="ensuring-you-have-reference-data-in-place"></a>Başvuru verilerini yerinde kullandığınızdan emin olun
Başvuru veri kümesi, olaylar, olay kaynağınızdan büyütmek öğeleri koleksiyonudur. Zaman serisi görüşleri giriş altyapısı, başvuru veri kümenizdeki her olay, olay kaynağınızdan karşılık gelen veri satırı ile birleştirir. Bu genişletilmiş olay daha sonra sorgu için kullanılabilir. Bu birleşim, başvuru veri kümenizde tanımlı birincil anahtar sütunları dayanır.

Başvuru verileri geriye dönük olarak katılmamış unutmayın. Bu, yalnızca mevcut ve gelecekteki giriş verileri eşleşen ve yapılandırılmış ve karşıya sonra başvuru tarih kümesine katılmış anlamına gelir.  TSI için çok fazla geçmiş veri göndermek ve yok karşıya yükleyin veya başvuru verileri TSI ilk olarak oluşturmak planlıyorsanız, gerekebilir sonra iş yeniden yapın (ipucu, eğlenceli değildir).  

TSI içindeki, başvuru verilerini yönetme oluşturma ve karşıya yükleme hakkında daha fazla bilgi edinmek için head bizim *başvuru verileri* belgeleri [belgeleri](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

## <a name="business-disaster-recovery"></a>İş olağanüstü durum kurtarma
Bir Azure hizmeti olduğundan, zaman serisi görüşleri çözüm için gerekli olan herhangi bir ek çalışma yapmadan Azure bölgesi düzeyinde fazlalıkları kullanarak yüksek kullanılabilirlik (HA) sağlar. Microsoft Azure platformu, olağanüstü durum kurtarma (DR) özellikleri veya bölgeler arası kullanılabilirlik ile çözümler oluşturmanıza yardımcı olacak özellikler de içerir. Genel sağlamak istiyorsanız, bölgeler arası yüksek kullanılabilirlik için cihazlara veya kullanıcılara, bu Azure DR özelliklerden yararlanın. Makaleyi [Azure iş sürekliliği teknik rehberlik](../resiliency/resiliency-technical-guidance.md) iş sürekliliği ve olağanüstü durum kurtarma için azure'da yerleşik özellikler açıklanmaktadır. [Olağanüstü durum kurtarma ve Azure uygulamaları için yüksek kullanılabilirlik] [olağanüstü durum kurtarma ve Azure uygulamaları için yüksek kullanılabilirlik] kağıt, HA ve DR elde etmek, Azure uygulamaları için stratejileri hakkında mimari rehberlik sağlar.

Time Series Insights yerleşik iş olağanüstü durum kurtarma (BCDR) sahip değil.  Ancak, BCDR gerektiren müşteriler hala kurtarma stratejisi uygulayabilir. Yedekleme bir Azure bölgesinde ikinci bir zaman serisi görüşleri ortamı oluşturma ve ikinci bir adanmış bir tüketici grubu ve bu olay kaynağının BCDR yönergeleri yararlanarak birincil olay kaynağından ikincil bu ortam için olayları gönderirsiniz.  

1.  Ortam, ikinci bir bölgede oluşturun.  Zaman serisi görüşleri ortamı oluşturma hakkında daha fazla [burada](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-get-started).
2.  Olay kaynağınız için ikinci bir adanmış bir tüketici grubu oluşturun ve bu olay kaynak yeni ortama bağlanın.  İkinci ve adanmış bir tüketici grubu tanımlamak emin olun.  Ya da izleyerek bu konu hakkında daha fazla bilgi edinebilirsiniz [IOT Hub belgeleri](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) veya [Event hub belgeleri](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-data-access).
3.  Bir olağanüstü durum olayı sırasında Git, birincil bölge olsaydı, işlemlerinin yedekleme zaman serisi görüşleri ortamına geçiş yapın.  

IOT Hub'ın BCDR ilkeleri hakkında daha fazla bilgi edinmek için head [burada](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-ha-dr).  Olay hub'ın BCDR ilkeleri hakkında daha fazla bilgi edinmek için head [burada](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-geo-dr).  

## <a name="next-steps"></a>Sonraki adımlar
- [Bir Event Hub olay kaynağı ekleme](time-series-insights-how-to-add-an-event-source-eventhub.md)
- [IOT Hub olay kaynağı ekleme](time-series-insights-how-to-add-an-event-source-iothub.md)
