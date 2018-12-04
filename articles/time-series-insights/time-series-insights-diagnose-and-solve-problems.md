---
title: Azure Time Series Insights, sorunları tanılama ve çözme | Microsoft Docs
description: Bu makalede, Azure zaman serisi görüşleri ortamınıza karşılaşabileceğiniz genel sorunları çözmesine tanılama ve sorun giderme açıklar.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 04/09/2018
ms.openlocfilehash: ef06e7b1abd66a2204ef982943fe24354bd7f122
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52837452"
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Time Series Insights ortamınızdaki sorunları tanılayın ve çözün

Bu makalede, zaman serisi görüşleri ortamınıza görebileceğiniz bazı sorunlar açıklanır. Olası nedenler ve çözümler için çözüm sunar.

## <a name="video"></a>Video: 

### <a name="in-this-video-we-cover-common-time-series-insights-customer-challenges-and-mitigationsbr"></a>Bu videoda, ortak Time Series Insights müşteri sorunları ve riskleri azaltma kapsar.</br>

> [!VIDEO https://www.youtube.com/embed/7U0SwxAVSKw]

## <a name="problem-1-no-data-is-shown"></a>Sorun 1: Veri gösterilir
Neden olmayan görebileceğiniz verilerinizi birkaç genel nedeni vardır [Azure Time Series Insights Gezgini](https://insights.timeseries.azure.com):

### <a name="possible-cause-a-event-source-data-is-not-in-json-format"></a>Olası nedeni y: olay kaynağı verileri JSON biçiminde değil.
Azure Time Series Insights, yalnızca JSON verilerini destekler. JSON örnekleri için bkz [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

### <a name="possible-cause-b-event-source-key-is-missing-a-required-permission"></a>Olası nedeni B: olay kaynağı anahtarı gerekli izni yok
* Bir IOT Hub için anahtar sağlamanız gereken **hizmetini bağlama** izni.

   ![IOT Hub hizmeti connect izni yok](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Yukarıdaki görüntüde, ilkelerin ya da gösterildiği gibi **iothubowner** ve **hizmet** çalışır, çünkü her ikisi de **hizmetini bağlama** izni.

* Bir event hub için anahtar sağlamanız gereken **dinleme** izni.

   ![Olay hub'ı dinleme izni](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Yukarıdaki görüntüde, ilkelerin ya da gösterildiği gibi **okuma** ve **yönetme** çalışır, çünkü her ikisi de **dinleme** izni.

### <a name="possible-cause-c-the-consumer-group-provided-is-not-exclusive-to-time-series-insights"></a>Olası neden C: Belirtilen tüketici grubu için Time Series Insights özel değil
IOT hub'ı kaydı veya bir olay hub'ı sırasında verileri okumak için kullanılması gereken tüketici grubu belirtin. Bu tüketici grubunun gerekir **değil** paylaşılabilir. Tüketici grubu paylaşılıyorsa, temel alınan olay hub'ı otomatik olarak bir okuyucu rastgele bağlantısını keser. Time Series Insights'ın okumak için bir benzersiz bir tüketici grubu sağlayın.

## <a name="problem-2-some-data-is-shown-but-some-is-missing"></a>Sorun 2: Bazı veriler gösterilir, ancak bazı eksik
Verileri kısmen görebilirsiniz, ancak veri geciken dikkate alınması gereken birkaç olasılık vardır:

### <a name="possible-cause-a-your-environment-is-getting-throttled"></a>Olası nedeni y: ortamınızı kısıtlanmış
Ortamları verilerle olay kaynağı oluşturulduktan sonra sağlandığında sık karşılaşılan bir sorundur.  Azure IOT hub'ları ve olay hub'ları veri yedi gün için depolama yapar.  TSI, her zaman içinde olay kaynağı eski bir etkinlikten (FIFO) başlar.  Bu nedenle tek birimli TSI ortamı, bir S1 ila bağlandığınızda beş milyon olayı olay kaynağı varsa TSI günde yaklaşık bir milyon olay okuyun.  Bu, TSI gecikme süresi, beş gün ilk bakışta yaşıyor gibi sorgulamanıza aramak için görünebilir.  Aslında neler olduğunu ortamı kısıtlanıyor olur.  Eski olayları, olay kaynağınızdaki varsa, iki yoldan biriyle yaklaşımını:

- Yardımcı olmak için olay kaynağının bekletme sınırları içinde TSI görünmesini istemediğiniz eski olayları kurtulun Değiştir
- Eski olayları verimini artırmak için daha büyük bir ortamı boyutu (açısından birim sayısı) sağlayın.  Yukarıdaki örnekte, bir gün için beş birimleri aynı söz konusu S1 ortama artırılmış kullanıyorsa, ortam yakalama için şimdi gün içinde olmalıdır.  Kararlı bir duruma olay üretim 1 milyon veya daha az olayları günde ise, bunu zachytila výjimku sonra olayına geri bir birim kapasitesi azaltabilir.  

Kısıtlama sınırı, ortamın SKU türüne ve kapasite göre zorlanır. Bu kapasiteyi ortamdaki tüm olay kaynakları paylaşır. IOT hub'ı veya olay hub'ı için olay kaynağı, uygulanan sınırları aşan veri gönderme, azaltma ve bir gecikme bakın.

Aşağıdaki diyagramda, bir SKU S1 ve 3 kapasiteye sahip bir zaman serisi görüşleri ortamı gösterilmektedir. Günde 3 milyona kadar olay giriş alabilir.

![Geçerli SKU kapasitesi, ortam](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Örneğin, bu ortam bir olay hub'ından iletiler başlayan kümeniz olduğunu varsayalım. Aşağıdaki diyagramda da görüldüğü giriş oranı inceleyin:

![Bir olay hub'ına yönelik örnek giriş oranı](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Diyagramda gösterildiği gibi günlük giriş ~ 67,000 iletileri hızıdır. Bu oran, kabaca dakikada 46 iletileri çevirir. Her bir olay hub'ı ileti için tek bir zaman serisi görüşleri olay düzleştirilir, bu ortamda herhangi bir kısıtlama görür. Ardından her bir olay hub'ı iletisi 100 Time Series Insights olaylarına düzleştirilir, 4,600 olayları dakikada alınan. 3 kapasiteye sahip bir S1 SKU'ya ortamda dakikada giriş yalnızca 2100 olayları kullanabilirsiniz (günde 1 milyon olaya = 3 birim dakika başına 700 olayları dakika başına 2100 olayları =). Bu nedenle, kısıtlama nedeniyle bir gecikme görürsünüz. 

Mantıksal düzleştirme nasıl çalıştığına ilişkin üst düzey anlamak için bkz. [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

### <a name="recommended-resolution-steps-for-excessive-throttling"></a>Aşırı azaltma için önerilen çözüm adımları
Lag düzeltmek için SKU kapasitesi ortamınızın artırın. Daha fazla bilgi için [Time Series Insights ortamınızı ölçeklendirme](time-series-insights-how-to-scale-your-environment.md).

### <a name="possible-cause-b-initial-ingestion-of-historical-data-is-causing-slow-ingress"></a>Olası nedeni B: ilk alımı geçmiş verilerin yavaş giriş neden oluyor
Mevcut bir olay kaynağı bağlanıyorsanız, IOT hub'ı veya olay hub'ınızı zaten veri içerdiğinden emin olasıdır. Olay kaynağının ileti Bekletme dönemi başına verileri çekerek ortamı başlatır.

Bu davranış varsayılan davranıştır ve geçersiz kılınamaz. Azaltma etkileşim kurun ve geçmiş veri alma hakkında bilgi edinmek için biraz zaman alabilir.

#### <a name="recommended-resolution-steps-of-large-initial-ingestion"></a>Büyük ilk alımı önerilen çözüm adımları
Lag düzeltmek için aşağıdaki adımları uygulayın:
1. (Bu örnekte 10) değeri izin verilen üst sınırı için SKU kapasitesi artırın. Kapasiteyi artırdık sonra çok daha hızlı yakalama giriş işlemi başlatır. Ne kadar hızlı, kullanılabilirlik grafiği aracılığıyla yakalama görselleştirebilirsiniz [Time Series Insights gezgininin](https://insights.timeseries.azure.com). Artan kapasite için ücretlendirilir.
2. Lag yakalandı sonra SKU kapasitesi normal giriş oranınız azaltın.

## <a name="problem-3-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>3. sorun: Benim olay kaynağının *zaman damgası özellik adı* ayarı çalışmaz
Ad ve değer şu kurallara uyar emin olun:
* Zaman damgası özellik adı _büyük/küçük harfe_.
* Bir JSON dizesi olarak olay kaynağınızı geldiği zaman damgası özellik değeri biçimi olmalıdır _yyyy-MM-ddTHH. FFFFFFFK_. Bu tür bir dize örneğidir "2008-04-12T12:53Z".

Emin olmak için en kolay yolu, *zaman damgası özellik adı* yakalanır ve düzgün çalışmasını TSI Gezgini kullanılacak.  TSI Gezgini içinde bir süre sonra size sağlanan seçin grafiğini kullanarak *zaman damgası özellik adı*.  Seçime sağ tıklayın ve seçin *olayları keşfet* seçeneği.  İlk sütun üst bilgisi olmalıdır, *zaman damgası özellik adı* ve olması gereken bir *($ts)* sözcüğünün yanındaki *zaman damgası*, yerine:
- *(abc)* , TSI belirtmek veri değerleri dize olarak okuma
- *Takvim simgesine*, TSI gösterir, veri değeri olarak okuyor *tarih/saat*
- *#*, TSI belirtmek veri değerlerini bir tamsayı olarak okuyor


## <a name="next-steps"></a>Sonraki adımlar
- Ek Yardım için üzerinde bir konuşma Başlat [MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) veya [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). 
- Ayrıca [Azure Destek](https://azure.microsoft.com/support/options/) yardımlı destek seçenekleri için.
