---
title: Azure Stream analytics'te zaman işleme anlama
description: Azure Stream Analytics'te işleme süresi hakkında bilgi edinin
author: jasonwhowell
ms.author: zhongc
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/05/2018
ms.openlocfilehash: 0eb4b77964aa3c07bac2af615a26c3a9199525de
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63760849"
---
# <a name="understand-time-handling-in-azure-stream-analytics"></a>Azure Stream analytics'te zaman işleme anlama

Bu makalede, pratik zaman Azure Stream Analytics hizmetinde sorunlarını çözmek için tasarım tercihlerinin nasıl kullanabileceğinizi ele alır. İşleme tasarım kararları Etkenler sıralama olaya yakından ilgili zaman.

## <a name="background-time-concepts"></a>Arka plan zamanı kavramları

Tartışma daha iyi çerçeve için bazı arka plan kavramları tanımlayalım:

- **Olay saati**: Ne zaman özgün olayın gerçekleştiği zaman. Örneğin, bir hareket ederken Otoyol üzerinde araba Ücretli standına yaklaşıyor.

- **İşlem süresi**: Olay işleme sistemi ulaştığında ve gözlemlenen süre. Örneğin, araba ve bilgisayar sistemi Ücretli standına algılayıcı gördüğünde veriyi işlemek için birkaç dakika sürer.

- **Filigran**: Hangi noktaya kadar olayları bir akış işlemciye ingressed edildiğini gösterir bir olay zaman işaretçisi. Filigranlar olayları almak Temizle ilerleme gösterir sistem sağlar. Belirli bir noktaya akışında ilerleme filigranlar göstermek için akışları gereği, gelen olay verilerini hiçbir zaman, durdurur.

   Filigran önemli bir kavramdır. Filigranlar sistem oluşturabilir, tam ve doğru belirlemek için Stream Analytics ve çekilmesini gerekmez tekrarlanabilir sonuçlar sağlar. İşleme, öngörülebilir ve tekrarlanabilir kesin bir şekilde gerçekleştirebilirsiniz. Örneğin, bir sayım bazı hata koşulu işleme için yapmanız gereken, filigranlar güvenli başlangıç ve bitiş noktaları demektir.

Bu konu ile ilgili ek kaynaklar Tyler Akidau'nın blog gönderilerini görün [akış 101](https://www.oreilly.com/ideas/the-world-beyond-batch-streaming-101) ve [akış 102](https://www.oreilly.com/ideas/the-world-beyond-batch-streaming-102).

## <a name="choosing-the-best-starting-time"></a>En iyi başlangıç saatini seçme

Stream Analytics, kullanıcılara olay saati seçmek için iki seçenek sağlar:

1. **Geliş saati**  

   Olay kaynağı ulaştığında geliş saati giriş kaynakta atanır. Geliş saati kullanarak erişebileceğiniz **EventEnqueuedUtcTime** özelliği için Event Hubs girişleri **IoTHub.EnqueuedTime** IOT hub'ı ve kullanmak için özelliği **BlobProperties.LastModified**  blob giriş özelliği.

   Geliş saati kullanarak varsayılan davranıştır ve en iyi senaryoları, arşivleme verileri için kullanılan gerekli hiçbir zamansal mantık olduğu.

2. **Uygulama zamanı** (Olay saati olarak da adlandırılan)

   Uygulama zamanı olayı oluşturulur ve bu atanan olay yükünün parçası. Uygulama zamanına göre olayları işlemek için **zaman damgası tarafından** yan tümcesinde select sorgusu. Varsa **zaman damgası tarafından** yan tümcesi eksik, olayları geliş saati tarafından işlenir.

   Zamansal mantık söz konusu olduğunda, bir zaman damgası yükteki kullanılması önemlidir. Böylece, kaynak sistemindeki veya ağ gecikmeleri olunması.

## <a name="how-time-progresses-in-azure-stream-analytics"></a>Azure Stream analytics'te ilerledikçe ne zaman

Uygulama zamanı kullanırken, saat ilerleme gelen olayları temel alır. Bu akış hiçbir olay varsa ya da olaylar gecikir bilmek sistem işleme için zordur. Azure Stream Analytics, bu nedenle, aşağıdaki şekilde giriş her bölüm için buluşsal filigranlar oluşturur:

1. Herhangi bir gelen olay olduğunda Filigran sıra dışı tolerans pencere boyutu kadar gördük en büyük olay zamanı gelmiştir.

2. Gelen bir olay olduğunda Filigran geçerli tahmini varış zamanı (geçen süre üzerinde arka planda bir giriş olayı görülen artı olayın geliş saati giriş son zamandan itibaren olayları işleme VM) geç varış toleransı penceresi olur.

   Geliş saati yalnızca, gerçek geliş saati Event Hubs gibi bir giriş olayı aracısı üzerinde oluşturulmuş olduğu için ve Azure Stream Analytics olayları işleme VM değil tahmin edilebilir.

Tasarım filigranlar oluşturma yanı sıra, iki ek amaca hizmet eder:

1. Sistem sonuçları isteğine ile veya olmadan gelen olayları oluşturur.

   Nasıl zamanında çıktı sonuçları görmek istedikleri kontrolüne sahip olursunuz. Azure portalında, üzerinde **olay sıralama** sayfasında, Stream Analytics işi yapılandırabileceğiniz **olan sıradışı olayları** ayarı. Bu ayarın yapılandırılması, olay akışının olayların sırası esneklikle dakikliğini dengelemeyi göz önünde bulundurun.

   Geç varış toleransı penceresi filigranlar gelen olayları olmaması durumunda bile oluşturma tutmak önemlidir. Bazen, olabilir bir süre nereden hiçbir gelen olayları, olay Giriş akışı seyrek olduğunda gibi gelir. Bu sorun, birden çok bölüm giriş olayı Aracısı kullanılarak exacerbated.

   Veri işleme sistemleri bir geç varış toleransı penceresi olmadan akış Gecikmeli çıkışları girişleri seyrek ve birden çok bölüm kullanılan düşebilir.

2. Sistem davranışı, tekrarlanabilir olmak zorundadır. Yinelenebilirliği, akış verilerini işleme sistemin önemli bir özelliktir.

   Filigran geliş saati ve uygulama zamanı elde edilir. Her ikisi de, olay Aracısı kalıcı ve bu nedenle tekrarlanabilir. Case geliş saati var olmayan olayları, Azure Stream Analytics günlüklerin tahmini varış süre için arıza kurtarma amacıyla yeniden yürütme sırasında yinelenebilirliği tahmin edilebilir var.

Kullanmayı seçerseniz, fark **geliş saati** olay saati sırası dayanıklılık ve geç varış toleransı yapılandırmak için gerek yoktur. Bu yana **geliş saati** giriş olayı Aracısı, Azure Stream Analytics yalnızca bölümüyseler yapılandırmaları tekdüze artırma garanti edilir.

## <a name="late-arriving-events"></a>Geç gelen olaylar

Geç varış toleransı penceresi gelen her olay için tanımı tarafından Azure Stream Analytics karşılaştırır **olay saati** ile **geliş saati**; olay saati toleransı penceresi dışında ise, Sistem olay bırakın ya da tolerans dahilinde olacak şekilde olayın süresini ayarlamak için yapılandırabilirsiniz.

Filigranlar oluşturulduktan sonra hizmet potansiyel olarak olayları Olay saati eşik düşük ile alabilir göz önünde bulundurun. Ya da hizmeti yapılandırabilirsiniz **bırak** bu olayları veya **ayarlamak** olayın süresi eşik değeri.

Ayarlama, olay bir parçası olarak **System.Timestamp** yeni değere ayarlanmış ancak **olay saati** alanın kendisi değiştirilmez. Bu ayarı yalnızca bir olay burada ait olduğu **System.Timestamp** olay Zamanı alanındaki değerden farklı olabilir ve oluşturulacak beklenmeyen sonuçlara neden olabilir.

## <a name="handling-time-variation-with-substreams"></a>Zaman değişim alt akışları ile işleme

Buluşsal Filigran oluşturma mekanizması, burada çoğunlukla çeşitli olay gönderenleri arasında eşitlendiğini genellikle iyi çalışır burada açıklanmıştır. Ancak gerçek hayatta, özellikle çok sayıda IOT senaryolarında, sistem olay gönderenleri saatin üzerinde çok az denetimde sahiptir. Olay gönderenleri cihazlar alanında her türlü farklı donanım ve yazılım sürümlerinde belki de olabilir.

Stream Analytics, filigran tüm olaylarına genel bir giriş bölümünde kullanmak yerine, yardımcı olacak alt akışları adlı başka bir mekanizma vardır. İşinizi alt akışları kullanan bir işi sorgu yazarak kullanabilir [ **TIMESTAMP BY** ](/stream-analytics-query/timestamp-by-azure-stream-analytics) yan tümcesi ve anahtar sözcüğü **üzerinden**. Alt akış belirlemek için bir anahtar sütunu adından sonra sağlamak **üzerinden** anahtar sözcüğü gibi bir `deviceid`, böylece sistem sütuna göre zaman ilkeleri uygular. Her alt akış kendi bağımsız Filigran alır. Bu mekanizma zamanında çıkış oluşturma, ne zaman ile büyük ilgilenme izin ver saat farklarından izin vermeyi veya olay gönderenleri arasında gecikmeler ağ yararlı olur.

Alt akışları Azure Stream Analytics tarafından sağlanan benzersiz bir çözümdür ve diğer akış veri işleme sistemleri tarafından sağlanmaz. Stream Analytics geç varış toleransı penceresi alt akışları kullanıldığında gelen olaylar için geçerlidir. Varsayılan ayar (5 saniye), büyük olasılıkla kaliteleri zaman damgalı cihazlar için çok küçük. 5 dakika ile başlamak ve kullanıcıların cihaz saat eğriltme deseni göre ayarlamalar öneririz.

## <a name="early-arriving-events"></a>Erken gelen olayları

Geç varış toleransı penceresi tersini gibi görünen erken varış penceresi adlı başka bir kavram fark etmiş olabilirsiniz. Bu pencere 5 dakika olarak sabit ve geç varış bir'dan farklı bir amaca hizmet eder.

Azure Stream Analytics, tüm sonuçların her zaman ürettiği garanti eder, çünkü yalnızca belirtebilirsiniz **iş başlangıç zamanı** ilk çıkış saati işinin giriş saati olarak. Tam pencere işlenir, pencerenin ortasından yalnızca iş başlangıç zamanı gereklidir.

Stream Analytics, başlangıç saatini sorgu belirtiminden sonra türetilir. Ancak, giriş olayı Aracısı yalnızca geliş saati sıralandığından, sistem geliş saati için başlayan olay saati çevirmek vardır. Sistem, olayları işlemeyi, o noktadan giriş olayı Aracısı başlayabilirsiniz. Erken gelen pencere sınırı ile çeviri oldukça basittir. Olay süresi 5 dakikalık erken gelen pencere eksi başlatılıyor. Bu hesaplama, sistem olay saati varış saatinden sonra 5 dakika görülen tüm olayları bırakır ayrıca anlamına gelir.

Bu kavram, işleme çıktısı başladığınız ne olursa olsun tekrarlanabilir olduğundan emin olmak için kullanılır. Akış birçok diğer sisteme yaptıkları talebi olarak mekanizmayı, Yinelenebilirlik, garanti mümkün olmayacaktır.

## <a name="side-effects-of-event-ordering-time-tolerances"></a>Olay saati toleranslar sıralama yan etkileri

Stream Analytics işleri birkaç sahip **olay sıralama** seçenekleri. İki Azure portalında yapılandırılabilir: **olan sıradışı olayları** ayarı (sırası dayanıklılık) ve **geç gelen olaylar** ayarı (geç varış toleransı). **Erken varış** dayanıklılık sabittir ve değiştirilemez. Bu zaman ilkeler güçlü garanti sağlamak için Stream Analytics tarafından kullanılır. Ancak, bu ayarları bazı bazen beklenmeyen etkiler:

1. Yanlışlıkla çok erken olayları gönderme.

   Erken olayları normalde yüzdelik değil. Gönderenin saat rağmen çok hızlı çalışıyorsa erken olayları çıkışa gönderilir mümkündür. Tüm erken gelen olayları bırakılır, bu şekilde çıktısından hiçbirini göremezsiniz.

2. Eski olayları, Azure Stream Analytics tarafından işlenmek üzere Event hubs'a gönderme.

   Eski olayları ilk olarak, geç varış toleransı uygulaması nedeniyle, zararsız görünebilir, ancak eski olaylar bırakılabilir. Olaylar çok eski ise **System.Timestamp** değer olay alımı sırasında değiştirilemez. Bu davranış nedeniyle, şu anda Azure Stream Analytics daha neredeyse gerçek zamanlı Olay işleme senaryolarına, yerine geçmiş olay işleme senaryoları için uygundur. Ayarlayabileceğiniz **geç gelen olaylar** zaman bazı durumlarda bu davranışı geçici olarak çözmek için en büyük olası değerle (20 gün).

3. Çıkışlar Gecikmeli görünmektedir.

   İlk Filigran hesaplanan sırasında oluşturulan: **en uzun olay süresi** sistem kadar sıra dışı tolerans pencere boyutunu gözlemleri. Varsayılan olarak, sırası tolerans sıfır (00 dakika 00 saniye için) olarak yapılandırılır. Yüksek, sıfır olmayan bir zaman değerine ayarlandığında, saat değeri tarafından Gecikmeli (veya üstü) ilk akış işin çıktısını hesaplanan eşik ilk nedeniyle.

4. Seyrek girişlerdir.

   Belirli bir bölümünde hiç girdi olduğunda Filigran saat olarak hesaplanır **geliş saati** eksi geç varış toleransı penceresi. Sonuç olarak, sık ve seyrek giriş olayları, çıkış bu süreyi tarafından ertelenebilir. Varsayılan **geç gelen olaylar** 5 saniyede bir değerdir. Giriş olayları bir anda, örneğin gönderirken bir gecikme görmeyi beklemelisiniz. Ayarladığınızda gecikmeleri yarışacağından, alabilirsiniz **geç gelen olaylar** penceresine büyük bir değer.

5. **System.Timestamp** değerdir sürede farklı **olay saati** alan.

   Daha önce açıklandığı gibi sistem sırası dayanıklılık ya da geç varış toleransı windows olay saati ayarlar. **System.Timestamp** olay değeri ayarlandı, ancak **olay saati** alan.

## <a name="metrics-to-observe"></a>Gözlemlemek için ölçümleri

Bir dizi olay aracılığıyla zaman dayanıklılık etkileri sıralama inceleyebileceğiniz [Stream Analytics iş ölçümleri](stream-analytics-monitoring.md). Aşağıdaki ölçümler ilgilidir:

|Ölçüm  | Açıklama  |
|---------|---------|
| **Sıra dışı olayları** | Bırakılmış veya ayarlanmış bir zaman damgası verilen sıralama dışında alınan olayların sayısını gösterir. Bu ölçüm doğrudan yapılandırması tarafından etkilenen **olan sıradışı olayları** ayarını **olay sıralama** Azure portalında işi sayfasında. |
| **Geç giriş olayları** | Geç kaynaktan gelen olayları sayısını gösterir. Bu ölçüm, bırakılmış olan veya damgasına sahip olayları içerdiği ayarlanmış. Bu ölçüm doğrudan yapılandırması tarafından etkilenen **geç gelen olaylar** ayarı **olay sıralama** Azure portalında işi sayfasında. |
| **Erken giriş olayları** | Erken ya da bırakıldı veya erken 5 dakikadan uzun olmaları durumunda damgasına ayarlandı kaynaktan gelen olayları sayısını gösterir. |
| **Eşik gecikmesi** | Veri işleme iş akışında gecikmesini gösterir. Daha fazla bilgi için aşağıdaki bölüme bakın.|

## <a name="watermark-delay-details"></a>Eşik gecikmesi ayrıntıları

**Eşik gecikmesi** ölçüm işleme düğümünün, görülen şu ana kadar en büyük Filigran eksi duvar saati süresi olarak hesaplanır. Daha fazla bilgi için [eşik gecikmesi blog gönderisi](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/).

Bu ölçüm değeri normal işlem altında 0'dan büyükse birkaç nedeni olabilir:

1. Akış işlem hattının devralınan bir işlem gecikmesi. Normalde bu gecikme çok düşük olur.

2. Filigran toleransı penceresi boyutuna düşürüldüğünden sırası toleransı penceresi gecikme kullanıma sunuldu.

3. Filigran boyutuyla düşürüldüğünden geç varış penceresi gecikme sunulan toleransı penceresi.

4. Ölçü oluşturma işlem düğümünü eğriltme saat.

Akış işlem hattının yavaşlamasına neden olabilecek diğer kaynak kısıtlamaları vardır. Eşik gecikmesi ölçüm nedeniyle artabilir:

1. Stream analytics'te birimin giriş olaylarını işlemek için yeterli işlem kaynakları. Kaynakları ölçeklendirmek için bkz: [anlayın ve akış birimi Ayarla](stream-analytics-streaming-unit-consumption.md).

2. Bunlar kısıtlanan için giriş olay aracıları içinde yeterli aktarım hızı. Olası çözümler için bkz: [Azure Event Hubs işleme birimleri otomatik olarak ölçeklendirme](../event-hubs/event-hubs-auto-inflate.md).

3. Bunlar kısıtlanan için yeterli kapasiteye sahip çıkış havuzlarının sağlanmadı. Olası çözümler yaygın olarak kullanılan çıkış hizmet örneğinizin göre değişiklik gösterir.

## <a name="output-event-frequency"></a>Çıktı olay sıklığı

Azure Stream Analytics Filigran ilerleme durumunu çıktı olaylar üretmek için tek bir tetikleyici olarak kullanır. Filigran gelen giriş verilerinin türetildiğinden hatadan kurtarma sırasında hem de kullanıcı tarafından başlatılan yeniden işlemeyerek tekrarlanabilir.

Kullanırken [pencereli toplamlar](stream-analytics-window-functions.md), hizmet yalnızca windows sonunda çıktılar üretir. Bazı durumlarda, kullanıcılar windows üretilen kısmi toplamlar görmek isteyebilirsiniz. Kısmi toplamlar, Azure Stream Analytics'te şu anda desteklenmemektedir.

Diğer akış çözümleri, çıkış olayları dış koşullara bağlı olarak çeşitli tetikleyici noktalarında gerçekleştirilmiş. Çıkış olayları belirtilen zaman penceresi için birden çok kez oluşturulabilir bazı çözümlere mümkündür. Giriş değerleri daraltılmış olarak toplama sonuçları daha doğru olur. Olayları ilk ve düzeltilmiş zamanla tahmin edilen. Örneğin, belirli bir cihaz, ağ bağlantısı çevrimdışı olduğunda, tahmini bir değer sistem tarafından kullanılabilir. Daha sonra aynı cihaz ağa çevrimiçine döner. Ardından giriş akışında gerçek olay verileri eklenecektir. O zaman penceresini işleme çıktı sonuçları daha doğru bir çıktı üretir.

## <a name="illustrated-example-of-watermarks"></a>Filigranlar Resimli örneği

Aşağıdaki resimlerde nasıl farklı durumlarda filigranlar ilerleme gösterir.

Bu tablo aşağıda grafiğinin örnek verileri gösterir. Bazen eşleşen olay saati ve geliş saati farklılık, dikkat edin ve bazen değil.

| Etkinlik saati | Geliş saati | DeviceId |
| --- | --- | --- |
| 12:07 | 12:07 | cihaz1
| 12:08 | 12:08 | cihaz2
| 12:17 | 12:11 | cihaz1
| 12:08 | 12:13 | device3
| 12:19 | 12:16 | cihaz1
| 12:12 | 12:17 | device3
| 12:17 | 12:18 | cihaz2
| 12:20 | 12:19 | cihaz2
| 12:16 | 12:21 | device3
| 12:23 | 12:22 | cihaz2
| 12:22 | 12:24 | cihaz2
| 12:21 | 12:27 | device3

Bu çizimde, aşağıdaki toleranslar kullanılır:

- Erken varış windows olan 5 dakika
- Geç gelen penceresi 5 dakikadır
- Yeniden sıralama penceresinin 2 dakikadır

1. Bu olayları Filigran ilerlediğini çizimi:

   ![Azure Stream Analytics Filigran çizim](media/stream-analytics-time-handling/WaterMark-graph-1.png)

   Önceki grafikte gösterilen önemli işlemler:

   1. İlk olay cihaz (1) ve ikinci olay (device2) kez hizalı ve düzeltmeleri işlenir. Her olay Filigran ilerler.

   2. Üçüncü olay (cihaz 1) işlendiğinde geliş saati (12:11) olay saati (12:17) önce gelir. 5 dakikalık erken varış toleransı nedeniyle bırakılan olay için olay erken 6 dakika geldi.

      Filigran erken bir olayın bu durumda ilerleme değil.

   3. Dördüncü olay (device3) ve beşinci olay (cihaz 1) kez hizalı ve ayarlama işlenir. Her olay Filigran ilerler.

   4. Altıncı olay (device3) işlendiğinde geliş saati (12:17) ve olay saati (12:12) Filigran düzeyleri verilmiştir. Olay saati (12:17) su işareti düzeyine ayarlanır.

   5. Dokuzuncu olay (device3) işlendiğinde geliş saati (12:27) 6 dakika (12:21) olay vaktinden ' dir. Geç varış ilke uygulanır. Olay zamandır ayarlanmış (12:22), başka hiçbir şekilde ayarlaması (12:21) eşiğin olduğu uygulanır.

2. Filigran olmadan bir erken varış İlkesi ilerlediğini ikinci çizimi:

   ![Azure Stream Analytics erken hiçbir ilke Filigran çizim](media/stream-analytics-time-handling/watermark-graph-2.png)

   Bu örnekte, hiçbir erken varış ilke uygulanır. Erken gelen aykırı Filigran önemli ölçüde olay. Üçüncü olay dikkat edin (deviceId1, saat 12: 11'de) Bu senaryoda atılmadı ve 12: 15'e Filigran tetiklenir. Dördüncü olay ayarlanmış iletme 7 dakika (12:08-12:15) sonucunda zamandır.

3. Son çizimde, alt akışları (DeviceID) kullanılır. Birden çok filigranlar izlenir, her akış. Sonuç olarak ayarlanmış süreleri ile daha az olaylar vardır.

   ![Azure Stream Analytics substreams Filigran çizim](media/stream-analytics-time-handling/watermark-graph-3.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stream Analytics olay sırası konuları](stream-analytics-out-of-order-and-late-events.md)
- [Stream Analytics işi ölçümleri](stream-analytics-monitoring.md)
