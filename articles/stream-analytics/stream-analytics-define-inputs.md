---
title: Azure Stream Analytics giriş olarak Stream veri
description: Azure Stream analytics'te bir veri bağlantısı kurma bilgi edinin. Giriş olayları veri akışından içerir ve ayrıca verilere başvurur.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 2a366a9030104c885adb1a4f773de04cdc439044
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61480502"
---
# <a name="stream-data-as-input-into-stream-analytics"></a>Stream Analytics giriş olarak Stream veri

Stream Analytics, Azure veri akışları ile birinci sınıf tümleştirme giriş kaynakları üç tür olarak sahiptir:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Depolama](https://azure.microsoft.com/services/storage/blobs/) 

Bu giriş kaynakları, Stream Analytics işinizi aynı Azure aboneliğinde veya farklı bir abonelik Canlı çalıştırabilirsiniz.

### <a name="compression"></a>Sıkıştırma
Stream Analytics, tüm veri akışı giriş kaynaklarında sıkıştırma destekler. Şu anda desteklenen sıkıştırma türleri şunlardır: Yok, GZip ve Deflate sıkıştırma. Sıkıştırma desteğine başvuru verileri için kullanılabilir değil. Giriş biçimi, sıkıştırılmış Avro veri olması durumunda saydam bir şekilde ele alınır. Avro serileştirme ile sıkıştırma türünü belirtmeniz gerekmez. 

## <a name="create-edit-or-test-inputs"></a>Oluşturma, düzenleme veya test girişleri
Kullanabileceğiniz [Azure portalı](https://portal.azure.com) için [yeni girdileri Oluştur](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-portal#configure-job-input) görüntüleyebilir veya var olan akış işinizin girişler düzenleyin. Ayrıca, giriş bağlantılarını test edebilirsiniz ve [test sorguları](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-manage-job#test-your-query) örnek verilerden. Bir sorgu yazdığınızda, giriş FROM yan tümcesi içinde listelenir. Kullanılabilir girişler listesini alabilirsiniz **sorgu** portalında sayfası. Birden çok giriş kullanmak istiyorsanız, aşağıdakileri yapabilirsiniz `JOIN` bunları veya birden çok yazma `SELECT` sorgular.


## <a name="stream-data-from-event-hubs"></a>Event Hubs’dan veri akışı sağlama

Azure Event Hubs yüksek oranda ölçeklenebilir sağlar yayımlama-abonelik olay ingestors. Böylece işleyebilir ve analiz veri uygulamanızın bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki bir olay hub'ı, saniyede milyonlarca toplayabilirsiniz. Birlikte, Event Hubs ve Stream Analytics bir uçtan uca çözüm için gerçek zamanlı analizler sağlar. Olay hub'ları sağlar, olayları Azure'da içine akış gerçek zamanlı olarak ve Stream Analytics işleri, gerçek zamanlı olayları işleyebilir. Örneğin, Event Hubs'a web tıklama, sensör okumaları veya çevrimiçi günlüğü olaylarını gönderebilir. Ardından, Event Hubs, gerçek zamanlı filtreleyerek, toplayarak ve bağıntı için giriş veri akışları olarak kullanmak için Stream Analytics işleri de oluşturabilirsiniz.

`EventEnqueuedUtcTime` bir olay hub'ındaki bir olayın geliş zaman damgasını ve varsayılan Stream Analytics'e Event Hubs'dan gelen olayların zaman damgası. Veri yükü kullanmalısınız olayda bir zaman damgası kullanarak bir akış olarak işlenecek [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) anahtar sözcüğü.

### <a name="consumer-groups"></a>Tüketici grupları
Kendi tüketici grubu için giriş her Stream Analytics olay hub'ı yapılandırmanız gerekir. Bir işin ne zaman kendi kendine birleşme içeriyor veya sahip birden fazla giriş, bazı girişler aşağı yönde birden fazla okuyucu tarafından okunmaması. Bu durum tek bir tüketici grubundaki okuyucu sayısını etkiler. Bölüm başına tüketici grubu başına beş okuyucular Event Hubs sınırını aşmamak için her bir Stream Analytics işine ilişkin bir tüketici grubu tanımlamak için en iyi uygulama olan. Olay hub'ı başına 20 tüketici grubu sınırı yoktur. Daha fazla bilgi için [Azure Stream Analytics sorunlarını giderme girişleri](stream-analytics-troubleshoot-input.md).

### <a name="stream-data-from-event-hubs"></a>Event Hubs’dan veri akışı sağlama
Aşağıdaki tabloda her bir özellik açıklanmaktadır **yeni giriş** sayfası akış veri girişi için bir olay hub'ı Azure Portalı'nda:

| Özellik | Açıklama |
| --- | --- |
| **Giriş diğer adı** |Bu giriş başvurmak için işin sorgusunda kullandığınız kolay ad. |
| **Abonelik** | Olay hub'ı kaynağını bulunduğu aboneliği seçin. | 
| **Olay hub'ı ad alanı** | Olay hub'ı ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, ad alanı oluşturabilir. |
| **Olay hub'ı adı** | Giriş olarak kullanmak için olay hub'ının adı. |
| **Olay hub'ı ilke adı** | Olay Hub'ına erişimi sağlayan paylaşılan erişim ilkesi. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. Olay hub'ı ayarlarını sağlama seçeneği seçmediğiniz sürece bu seçenek otomatik olarak, doldurulur el ile.|
| **Olay hub'ı tüketici grubu** (önerilir) | Her bir Stream Analytics işi için ayrı bir tüketici grubu kullanmak için önerilir. Bu dize, olay hub'ından veri alma işlemini tüketici grubu tanımlar. Henüz hiç tüketici grubu belirtilirse, Stream Analytics işi $Default tüketici grubu kullanır.  |
| **Olay serileştirme biçimi** | Serileştirme biçimini (JSON, CSV veya Avro) gelen veri akışı.  JSON biçimini belirtimiyle uyumludur ve ondalık sayılar için önde gelen 0 içermez emin olun. |
| **Kodlama** | UTF-8, şu anda desteklenen tek kodlama biçimi aşamasındadır. |
| **Olay sıkıştırma türü** | Hiçbiri (varsayılan), GZip veya Deflate gibi gelen veri akışını okumak için kullanılan sıkıştırma türü. |

Olay hub'ı stream girdi verilerinizi söz konusu olduğunda, aşağıdaki meta verileri alanları Stream Analytics sorgunuzda erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **EventProcessedUtcTime** |Stream Analytics tarafından işlendiğinde olay saat ve tarihi. |
| **EventEnqueuedUtcTime** |Olay, olay hub'ları tarafından alındı saat ve tarihi. |
| **PartitionID** |Giriş bağdaştırıcısı sıfır bölüm kimliği. |

Örneğin, bu alanları kullanarak, aşağıdaki örnekte olduğu gibi bir sorgu yazabilirsiniz:

```sql
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
```

> [!NOTE]
> Olay hub'ı, IOT hub'ı yollar için bir uç nokta kullanırken, IOT hub'ı kullanarak meta verileri erişebilir [GetMetadataPropertyValue işlevi](https://msdn.microsoft.com/library/azure/mt793845.aspx).
> 

## <a name="stream-data-from-iot-hub"></a>IOT hub'ı Stream verileri
Azure IOT Hub, yüksek düzeyde ölçeklenebilir Yayımla-abone ol olay yutucu IOT senaryoları için iyileştirilmiş ' dir.

Stream analytics'te bir IOT Hub'ından gelen olayların varsayılan zaman damgası olan IOT Hub olay geldiği zaman damgası olan `EventEnqueuedUtcTime`. Veri yükü kullanmalısınız olayda bir zaman damgası kullanarak bir akış olarak işlenecek [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) anahtar sözcüğü.

### <a name="consumer-groups"></a>Tüketici grupları
Her Stream Analytics IOT Hub'ı tüketici grubu için giriş yapılandırmanız gerekir. Bir işi kendi kendine birleşme içerdiğinde ya da birden fazla giriş varsa, bazı giriş aşağı yönde birden fazla okuyucu tarafından okunmaması. Bu durum tek bir tüketici grubundaki okuyucu sayısını etkiler. Bölüm başına tüketici grubu başına beş okuyucular Azure IOT Hub sınırını aşmamak için her bir Stream Analytics işine ilişkin bir tüketici grubu tanımlamak için en iyi uygulama olan.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>Giriş veri akışı IOT hub'ı yapılandırma
Aşağıdaki tabloda her bir özellik açıklanmaktadır **yeni giriş** Giriş akışı olarak IOT hub'ı yapılandırırken Azure portalında sayfası.

| Özellik | Açıklama |
| --- | --- |
| **Giriş diğer adı** | Bu giriş başvurmak için işin sorgusunda kullandığınız kolay ad.|
| **Abonelik** | IOT hub'ı kaynak bulunduğu aboneliği seçin. | 
| **IoT Hub’ı** | Giriş olarak kullanmak için IOT hub'ının adı. |
| **Uç noktası** | IOT hub'ının uç noktası.|
| **Paylaşılan erişim ilkesi adı** | IOT Hub'ına erişimi sağlayan paylaşılan erişim ilkesi. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. |
| **Paylaşılan Erişim İlkesi anahtarı** | IOT hub'ına erişim yetkisi vermek için kullanılan paylaşılan erişim anahtarı.  IOT hub'ının ayarlarını sağlama seçeneği seçmediğiniz sürece bu seçenek, otomatik olarak doldurulur el ile. |
| **Tüketici grubu** | Her bir Stream Analytics iş için farklı bir tüketici grubu kullanmanızı önemle tavsiye edilir. Tüketici grubu, IOT Hub'ından veri almak için kullanılır. Stream Analytics, aksi belirtilmedikçe $Default tüketici grubu kullanır.  |
| **Olay serileştirme biçimi** | Serileştirme biçimini (JSON, CSV veya Avro) gelen veri akışı.  JSON biçimini belirtimiyle uyumludur ve ondalık sayılar için önde gelen 0 içermez emin olun. |
| **Kodlama** | UTF-8, şu anda desteklenen tek kodlama biçimi aşamasındadır. |
| **Olay sıkıştırma türü** | Hiçbiri (varsayılan), GZip veya Deflate gibi gelen veri akışını okumak için kullanılan sıkıştırma türü. |


Bir IOT Hub'ından veri akışı kullandığınızda, aşağıdaki meta verileri alanları Stream Analytics sorgunuzda erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **EventProcessedUtcTime** | Olay işlendi saat ve tarihi. |
| **EventEnqueuedUtcTime** | IOT Hub tarafından olayı alındı saat ve tarihi. |
| **PartitionID** | Giriş bağdaştırıcısı sıfır bölüm kimliği. |
| **IoTHub.MessageId** | IOT hub'ında iki yönlü iletişim ilişkilendirmek için kullanılan bir kimliği. |
| **IoTHub.CorrelationId** | IOT hub'ında ileti yanıtlar ve kullanılan bir kimliği. |
| **IoTHub.ConnectionDeviceId** | Bu ileti göndermek için kullanılan kimlik doğrulama kimliği. Bu değer servicebound iletileri IOT Hub tarafından damgalandı. |
| **IoTHub.ConnectionDeviceGenerationId** | Bu ileti göndermek için kullanılan kimliği doğrulanmış cihaz oluşturma kimliği. Bu değer servicebound iletileri IOT Hub tarafından damgalandı. |
| **IoTHub.EnqueuedTime** | İleti IOT Hub tarafından ne zaman alındığı zamanı. |


## <a name="stream-data-from-blob-storage"></a>Blob depolama alanındaki verilerin Stream
Azure Blob Depolama, büyük miktarlarda yapılandırılmamış veriyi bulutta depolayabilir ile senaryoları için uygun maliyetli ve ölçeklenebilir bir çözüm sunar. Blob depolama alanındaki verilerin genellikle bekleyen veri olarak değerlendirilir. Ancak, blob verilerini bir veri akışı Stream Analytics tarafından işlenebilir. 

Günlük işleme, Stream Analytics ile Blob Depolama girişlerini kullanarak için yaygın olarak kullanılan bir senaryodur. Bu senaryoda, telemetri veri dosyaları bir sisteminden yakalanan ve ayrıştırılması ve anlamlı verileri ayıklamak için işlenen gerekir.

Blob Depolama olaylarını Stream analytics'te varsayılan zaman damgası olan blob son değiştirildiği zaman damgası olan `BlobLastModifiedUtcTime`. Veri yükü kullanmalısınız olayda bir zaman damgası kullanarak bir akış olarak işlenecek [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) anahtar sözcüğü. Blob dosya varsa bir Stream Analytics işi saniyede Azure Blob Depolama giriş veri çeker. Blob dosya kullanılamıyorsa bir üstel geri alma bir gecikme süresiyle en fazla 90 saniyelik yoktur.

CSV biçimlendirilmiş girişleri *gerektiren* alanlar için veri kümesi ve tüm üst bilgi satırı alanları tanımlamak için bir üst bilgi satırı benzersiz olması gerekir.

Stream Analytics, olay hub'ına yakalama veya IOT hub'ı Azure depolama kapsayıcısı özel uç nokta tarafından oluşturulan seri durumdan çıkarılırken AVRO iletileri şu anda desteklemiyor.

> [!NOTE]
> Stream Analytics, mevcut bir blob dosyasına ekleyerek içeriği desteklemiyor. Stream Analytics her dosyanın yalnızca bir kez görüntüleyeceği ve dosyayı iş verileri okuma sonra gerçekleşen değişikliklerin işlenmez. En iyi blob dosyası için tüm verileri tek seferde karşıya yükleme ve ardından ek yeni olaylar farklı, yeni blob dosyasını eklemektir.
> 

### <a name="configure-blob-storage-as-a-stream-input"></a>Giriş akışı olarak BLOB depolamayı yapılandırma 

Aşağıdaki tabloda her bir özellik açıklanmaktadır **yeni giriş** Blob depolamaya Giriş akışı olarak yapılandırdığınızda Azure portalında sayfası.

| Özellik | Açıklama |
| --- | --- |
| **Giriş diğer adı** | Bu giriş başvurmak için işin sorgusunda kullandığınız kolay ad. |
| **Abonelik** | IOT hub'ı kaynak bulunduğu aboneliği seçin. | 
| **Depolama hesabı** | Blob dosyaların bulunduğu depolama hesabının adıdır. |
| **Depolama hesabı anahtarı** | Depolama hesabı ile ilişkili gizli anahtar. Blob Depolama alanı ayarlarını sağlama seçeneği seçmediğiniz sürece bu seçenek, otomatik olarak doldurulur el ile. |
| **Kapsayıcı** | Giriş blob kapsayıcısı. Kapsayıcıları Microsoft Azure Blob hizmetinde depolanan bloblar için mantıksal bir gruplandırmasını sağlar. İçin Azure Blob Depolama hizmetinin blob karşıya yüklediğinizde, bu blob kapsayıcısı belirtmeniz gerekir. Ya da tercih edebilirsiniz **var olanı kullan** kapsayıcı veya **Yeni Oluştur** oluşturulan yeni bir kapsayıcı için.|
| **Yol deseni** (isteğe bağlı) | Belirtilen kapsayıcı içindeki blobları bulmak için kullanılan dosya yolu. Yol, aşağıdaki üç değişkenin bir veya daha fazla örneğini belirtebilirsiniz: `{date}`, `{time}`, veya `{partition}`<br/><br/>Örnek 1: `cluster1/logs/{date}/{time}/{partition}`<br/><br/>Örnek 2: `cluster1/logs/{date}`<br/><br/>`*` Karakter yol ön eki için izin verilen bir değer değil. Yalnızca geçerli <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure blob karakter</a> izin verilir. Kapsayıcı adları veya dosya adlarını dahil yok. |
| **Tarih biçimi** (isteğe bağlı) | Yolda bir tarih değişkeniyle kullanırsanız, tarihi biçimlendirmek dosyaları düzenlenir. Örnek: `YYYY/MM/DD` |
| **Saat biçimi** (isteğe bağlı) |  Yolda zaman değişken kullanırsanız, biçiminde zamanı dosyaları düzenlenir. Şu anda desteklenen tek değerdir `HH` saat. |
| **Olay serileştirme biçimi** | Serileştirme biçimini (JSON, CSV veya Avro) gelen veri akışı.  JSON biçimini belirtimiyle uyumludur ve ondalık sayılar için önde gelen 0 içermez emin olun. |
| **Kodlama** | CSV ve JSON, UTF-8, şu anda desteklenen tek kodlama biçimi içindir. |
| **Sıkıştırma** | Hiçbiri (varsayılan), GZip veya Deflate gibi gelen veri akışını okumak için kullanılan sıkıştırma türü. |

Verilerinizi bir Blob Depolama kaynaktan geldiğinde, aşağıdaki meta verileri alanları Stream Analytics sorgunuzda erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **BlobName** |Olay geldiği giriş blob adı. |
| **EventProcessedUtcTime** |Stream Analytics tarafından işlendiğinde olay saat ve tarihi. |
| **BlobLastModifiedUtcTime** |Blob, son değiştirilme zamanı saat ve tarihi. |
| **PartitionID** |Giriş bağdaştırıcısı sıfır bölüm kimliği. |

Örneğin, bu alanları kullanarak, aşağıdaki örnekte olduğu gibi bir sorgu yazabilirsiniz:

```sql
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
