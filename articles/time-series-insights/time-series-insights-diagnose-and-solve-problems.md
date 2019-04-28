---
title: Azure Time Series Insights, sorunları çözmek tanılayın ve sorunlarını giderme | Microsoft Docs
description: Bu makalede, Azure zaman serisi görüşleri ortamınıza hatalarla karşılaşabilirsiniz yaygın sorunları çözme tanılama ve sorun giderme açıklar.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 04/09/2018
ms.custom: seodec18
ms.openlocfilehash: ad739041ebd20f9940e305efb19807df4c73cb8e
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63759717"
---
# <a name="diagnose-and-solve-issues-in-your-time-series-insights-environment"></a>Time Series Insights ortamınızdaki sorunları tanılayıp

Bu makalede, Azure zaman serisi görüşleri ortamınıza karşılaşabileceğiniz bazı sorunlar açıklanmaktadır. Bu makalede, olası nedenler ve çözümler için çözüm sunar.

## <a name="video"></a>Video

### <a name="in-this-video-we-cover-common-time-series-insights-customer-challenges-and-mitigationsbr"></a>Bu videoda, ortak Time Series Insights müşteri sorunları ve riskleri azaltma kapsar:</br>

> [!VIDEO https://www.youtube.com/embed/7U0SwxAVSKw]

## <a name="problem-one-no-data-is-shown"></a>Bir sorun: veri gösterilir

Veri içermeyen [Azure Time Series Insights gezgininin](https://insights.timeseries.azure.com) ortak çeşitli nedenlerle oluşabilir:

### <a name="cause-a-event-source-data-isnt-in-json-format"></a>Neden y: olay kaynağı verileri JSON biçiminde değil

Azure Time Series Insights, yalnızca JSON verilerini destekler. JSON örnekleri için bkz [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

### <a name="cause-b-the-event-source-key-is-missing-a-required-permission"></a>Bir gerekli izni neden B: olay kaynağı anahtarı eksik

* Azure IOT hub'ında bir IOT hub için anahtar sağlamalıdır **hizmetini bağlama** izinleri. Aşağıdakilerden birini **iothubowner** veya **hizmet** ilkeleri, sahip oldukları her iki çalışır **hizmetini bağlama** izinleri.

   ![IOT Hub hizmeti bağlantı izinleri](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

* Azure Event Hubs, bir olay hub için anahtar sağlamalıdır **dinleme** izinleri. Aşağıdakilerden birini **okuma** veya **yönetme** ilkeleri, sahip oldukları her iki çalışır **dinleme** izinleri.

   ![Olay hub'ı dinleme izinleri](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

### <a name="cause-c-the-consumer-group-provided-isnt-exclusive-to-time-series-insights"></a>Neden C: sağlanan tüketici grubu için Time Series Insights özel değil

IOT hub'ı veya bir olay hub'ı kaydettiğinizde, verileri okumak için kullanmak istediğiniz bir tüketici grubu ayarlamak önemlidir. Bu tüketici grubunun *paylaşılamaz*. Tüketici grubu paylaşılıyorsa, temel alınan IOT hub'ı veya olay hub'ı otomatik olarak ve rastgele bir okuyucu keser. Time Series Insights'ın okumak için bir benzersiz bir tüketici grubu sağlayın.

## <a name="problem-two-some-data-is-shown-but-data-is-missing"></a>İki sorun: bazı veriler gösterilir, ancak veriler eksik

Verileri yalnızca kısmen ve geciken olması için verilerin görüntülendiği, birkaç olasılık düşünmelisiniz.

### <a name="cause-a-your-environment-is-being-throttled"></a>Neden y: ortamınızın kısıtlanıyor

Veriler içeren bir olay kaynağı oluşturduktan sonra ortam sağlandığında azaltma sık karşılaşılan bir sorundur. Azure IOT Hub ve Azure olay hub'ları yedi güne kadar verileri depolar. Zaman serisi görüşleri her zaman başlatın eski olay ile olay kaynağı (ilk giren ilk çıkar veya *FIFO*).

Örneğin, bir S1'e bağlanırken bir olay kaynağı 5 milyona kadar olay varsa, tek birimli zaman serisi görüşleri ortamı, zaman serisi görüşleri yaklaşık 1 milyona kadar olay günlük okur. Time Series Insights beş gün gecikme yaşanıyor gibi görünebilir. Ancak, neler olduğunu ortamı kısıtlanıyor olur.

Eski olayları, olay kaynağınızdaki varsa, iki yoldan biriyle azaltma yaklaşımını:

- Olay kaynağının bekletme sınırları zaman serisi Öngörülerinde görünmesini istemediğiniz eski olayları kaldırılmasına yardımcı olması için değiştirin.
- Eski olayları verimini artırmak için daha büyük bir ortamı boyutu (birim sayısı) sağlayın. Önceki örnekte, bir gün için beş birimleri aynı S1 ortama artırırsanız kullanarak, ortamın bir gün içinde Kaçırdığınız. Kararlı bir duruma olay üretim günde 1 milyon veya daha az olayları ise, bunu yakaladıktan sonra olay için bir birim kapasitesi azaltabilir.

Kısıtlama sınırı, ortamın SKU türüne ve kapasite göre zorlanır. Bu kapasiteyi ortamdaki tüm olay kaynakları paylaşır. IOT hub'ı veya olay hub'ı için olay kaynağı uygulanan sınırları aşan veri gönderir, azaltma ve bir gecikme bakın.

Aşağıdaki şekilde, bir SKU S1 ve 3 kapasiteye sahip bir zaman serisi görüşleri ortamı gösterilmektedir. Günde 3 milyona kadar olay giriş alabilir.

![Geçerli SKU kapasitesi, ortam](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Örneğin, bu ortam bir olay hub'ından iletiler alan varsayılır. Aşağıdaki şekilde giriş oranı gösterilmektedir:

![Bir olay hub'ına yönelik örnek giriş oranı](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Günlük giriş ~ 67,000 iletileri oranıdır. Bu oran, dakikada yaklaşık 46 iletileri çevirir. Her bir olay hub'ı ileti için tek bir zaman serisi görüşleri olay düzleştirilir, kısıtlama oluşmaz. Her bir olay hub'ı iletisi 100 Time Series Insights olaylarına düzleştirilir, 4,600 olayları dakikada alınan. 3 kapasiteye sahip bir S1 SKU'ya ortamda dakikada giriş yalnızca 2100 olayları kullanabilirsiniz (günde 1 milyon olaya üç birimlerde dakikada 700 olayları = dakika başına 2100 olayları =). Bu kurulum için bir gecikme kısıtlama nedeniyle bakın. 

Mantıksal düzleştirme nasıl çalıştığına ilişkin üst düzey anlamak için bkz. [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

#### <a name="recommended-resolutions-for-excessive-throttling"></a>Aşırı azaltma için önerilen çözümler

Lag düzeltmek için SKU kapasitesi ortamınızın artırın. Daha fazla bilgi için [Time Series Insights ortamınızı ölçeklendirme](time-series-insights-how-to-scale-your-environment.md).

### <a name="cause-b-initial-ingestion-of-historical-data-slows-ingress"></a>Neden B: geçmiş verilerin ilk alımı giriş yavaşlatır.

Mevcut bir olay kaynağı bağlarsanız, IOT hub'ı veya olay hub'ı zaten verileri içerdiğini olasıdır. Olay kaynağının ileti Bekletme dönemi başına verileri çekerek ortamı başlatır. Bu varsayılan işleme ve geçersiz kılınamaz. Azaltma görüşebilirsiniz. Azaltma, geçmiş verilerini alır gibi Kaçırdığınız biraz sürebilir.

#### <a name="recommended-resolutions-for-large-initial-ingestion"></a>Büyük ilk alımı için önerilen çözümler

Lag düzeltmek için:

1. (Bu durumda, 10) değeri izin verilen üst sınırı için SKU kapasitesi artırın. Kapasiteyi artırmak sonra çok daha çabuk yakalamak giriş işlemi başlatır. Artan kapasite için ücretlendirilir. Ne kadar hızlı yakalama görselleştirmek için kullanılabilirlik grafiği görüntüleyebileceğiniz [Time Series Insights gezgininin](https://insights.timeseries.azure.com). 

2. Lag yakalandı, normal giriş oranınız için SKU kapasitesi azalır.

## <a name="problem-three-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Sorun üç: benim olay kaynağının zaman damgası özellik adı ayarı çalışmaz

Değer ve zaman damgası özellik adı aşağıdaki kurallara uygun emin olun:

* Zaman damgası özellik adı büyük/küçük harfe duyarlıdır.
* Bir JSON dizesi biçimi olması gerektiği kadar olay kaynağından gelir zaman damgası özellik değerini _yyyy-MM-ddTHH. FFFFFFFK_. Bir örnek **2008-04-12T12:53Z**.

Zaman damgası özellik adı, yakalanan ve Time Series Insights gezginini kullanmak için düzgün bir şekilde çalışıyor olduğundan emin olmak için en kolay yolu. Grafiğini kullanarak Time Series Insights Gezgininde, zaman damgası özellik adı girildikten sonra süreyi seçin. Seçime sağ tıklayın ve ardından **olayları keşfet** seçeneği. 

İlk sütun başlığına, zaman damgası özellik adı olmalıdır. Word'ün yanındaki **zaman damgası**, görmelisiniz **($ts)**.

Aşağıdaki değerleri görmemeniz gerekir:

- *(abc)* : Time Series Insights veri değerleri dize olarak okuyor gösterir.
- *Takvim simgesine*: Time Series Insights veri değeri olarak okuyor gösterir *datetime*.
- *#*: Time Series Insights bir tamsayı veri değerleri okuma gösterir.

## <a name="next-steps"></a>Sonraki adımlar

- Yardım almak için bir konuşma başlatmak [MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) veya [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights).

- Yardım içeren destek seçeneklerini için [Azure Destek](https://azure.microsoft.com/support/options/).
