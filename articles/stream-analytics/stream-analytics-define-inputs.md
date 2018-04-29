---
title: Azure Stream Analytics giriş olarak veri akışı
description: Azure Stream Analytics Veri bağlantısında ayarlama bilgi edinin. Giriş olayları veri akışından içerir ve ayrıca veri başvuru.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/25/2018
ms.openlocfilehash: 1fc1791d75355cc30f2ef43fc17e39a868e2c756
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="stream-data-as-input-into-stream-analytics"></a>Stream Analytics giriş olarak veri akışı

Akış analizi kaynaklarını üç tür girdi olarak Azure veri akışları ile birinci sınıf tümleştirme vardır:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Depolama](https://azure.microsoft.com/services/storage/blobs/) 

Bu giriş kaynakları, aynı abonelikte Azure Stream Analytics işi olarak veya farklı bir abonelik dinamik.

### <a name="compression"></a>Sıkıştırma
Akış analizi, tüm veri akışı giriş kaynaklarına arasında sıkıştırma destekler. Şu anda desteklenen başvuru türleri: None, GZip ve sıkıştırma Deflate. Sıkıştırma desteği başvuru verileri için kullanılabilir değil. Giriş biçimi sıkıştırılmış Avro verileri ise, saydam olarak işlenir. Avro serileştirilmesi ile sıkıştırma türünü belirtmeniz gerekmez. 

## <a name="create-or-edit-inputs"></a>Oluşturma veya girişleri düzenleme
Yeni girişleri oluşturmak ve liste veya varolan girişler için iş akışında düzenlemek için Azure portalını kullanabilirsiniz:
1. Açık [Azure portal](https://portal.azure.com) bulup, akış analizi işi seçin.
2. Seçin **girişleri** altında seçeneği **ayarları** başlığı. 
4. **Girişleri** sayfası varolan tüm girişleri listeler. 
5. Üzerinde **girişleri** sayfasında, **akış giriş Ekle** veya **giriş başvuru ekleme** Ayrıntılar sayfasını açın.
6. Seçin ve ayrıntıları düzenlemek için varolan bir girişi seçin **kaydetmek** varolan bir girişi düzenlemek için.
7. Seçin **Test** bağlantı seçenekleri geçerli ve çalışıyor olduğunu doğrulamak için Giriş Ayrıntıları sayfasında. 
8. Var olan bir giriş adını sağ tıklatın ve seçin **örnek giriş verilerinden** daha fazla test etmek için gerektiği gibi.


## <a name="stream-data-from-event-hubs"></a>Event hubs veri akışı

Azure Event Hubs sağlayan yüksek düzeyde ölçeklenebilir olay ingestors yayımlama-abonelik. Böylece işlemek ve veri bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki analiz bir event hub saniye başına milyonlarca olayı toplayabilirsiniz. Birlikte olay hub'ları ve akış analizi için gerçek zamanlı analiz ile bir uçtan uca çözümü sağlar. Olay hub'ları, olayları Azure'da gerçek zamanlı akış olanak tanır ve bu olayları gerçek zamanlı akış analizi işleri işleyebilir. Örneğin, web tıklama, sensör okumaları veya çevrimiçi günlük olayları Event Hubs'a gönderebilirsiniz. Daha sonra gerçek zamanlı filtreleme, toplama ve bağıntı için giriş veri akışları olarak olay hub'ları kullanmak için akış analizi işleri oluşturabilirsiniz.

Varsayılan zaman damgası akış analizi, olay hub'ten gelen olayların olay Olay hub'ı, ama olan gelen zaman damgası olan `EventEnqueuedUtcTime`. Veri yükü, kullanmalısınız durumunda bir zaman damgası kullanarak bir akış işlemek için [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) anahtar sözcüğü.

### <a name="consumer-groups"></a>Tüketici grupları
Kendi tüketici grubu için giriş her akış analizi olay hub'ı yapılandırmanız gerekir. Bir işi kendi kendine birleşim içerdiğinde ya da birden fazla giriş varsa, bazı giriş aşağı birden fazla okuyucu tarafından okuyabilir. Bu durum, tek bir tüketici grubundaki okuyucu sayısını etkiler. Bölüm başına tüketici grubu başına beş okuyucuların olay hub'ları sınırını aşmamak için her akış analizi işi için bir tüketici grubu atamak için bir en iyi uygulamadır. Olay hub'ı başına 20 tüketici grupları sınırı yoktur. Daha fazla bilgi için bkz: [Event Hubs Programlama Kılavuzu](../event-hubs/event-hubs-programming-guide.md).

### <a name="stream-data-from-event-hubs"></a>Event hubs veri akışı
Aşağıdaki tabloda her bir özellik açıklanmaktadır **yeni giriş** bir olay hub'ı Azure portalından akış veri giriş sayfası:

| Özellik | Açıklama |
| --- | --- |
| **Giriş diğer adı** |Bu giriş başvurmak için işin sorguda kullanma kolay adı. |
| **Abonelik** | Olay hub'ı kaynak bulunduğu abonelik seçin. | 
| **Olay Hub'ad alanı** | Olay hub'ı ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda aynı zamanda ad alanını oluşturun. |
| **Olay hub'ı adı** | Giriş olarak kullanmak için olay hub'ın adı. |
| **Olay hub'ı ilke adı** | Olay Hub'ına erişimi sağlayan paylaşılan erişim ilkesi. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. Olay hub'ı ayarlarını sağlama seçeneğine seçmediğiniz sürece bu seçenek, otomatik olarak doldurulur el ile.|
| **Olay Hub tüketici grubu** (önerilir) | Her akış analizi işi için ayrı bir tüketici grubundaki kullanmak için önerilir. Bu dize, olay hub'ı verilerden alma için kullanılacak bir tüketici grubundaki tanımlar. Herhangi bir tüketici grubu belirtilirse, Stream Analytics işi $Default tüketici grubu kullanır.  |
| **Olayı seri hale getirme biçimi** | Seri hale getirme biçimi (JSON, CSV veya Avro) gelen veri akışının.  JSON biçimi belirtimiyle hizalar ve ondalık sayılar önüne 0 içermeyen emin olun. |
| **Kodlama** | UTF-8 anda desteklenen tek kodlama biçimi değil. |
| **Olay sıkıştırma türü** | Hiçbiri (varsayılan), GZip ve Deflate gibi gelen veri akışını okumak için kullanılan sıkıştırma türü. |

Verilerinizi bir olay hub'ı akış girişten geldiğinde, aşağıdaki meta veri alanları Stream Analytics sorgunuzda erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **EventProcessedUtcTime** |Olay akış analizi tarafından işlenen saat ve tarihi. |
| **EventEnqueuedUtcTime** |Olayın olay hub tarafından alınan saat ve tarihi. |
| **PartitionID** |Giriş bağdaştırıcısı sıfır tabanlı bölüm kimliği. |

Örneğin, bu alanları kullanarak, aşağıdaki örneğe benzer bir sorgu yazabilirsiniz:

```sql
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
```

> [!NOTE]
> Olay hub'ı IOT Hub yollar için bir uç nokta kullanırken, IOT hub'ı kullanarak medadata erişebilir [GetMetadataPropertyValue işlevi](https://msdn.microsoft.com/library/azure/mt793845.aspx).
> 

## <a name="stream-data-from-iot-hub"></a>IOT hub'dan veri akışı
Azure IOT Hub, yüksek düzeyde ölçeklenebilir yayımlama-abone olma IOT senaryoları için en iyi duruma getirilmiş olay yutucu ' dir.

Stream Analytics bir IOT Hub'ten gelen olayların varsayılan zaman damgası olan IOT Hub olay geldiği zaman damgası olan `EventEnqueuedUtcTime`. Veri yükü, kullanmalısınız durumunda bir zaman damgası kullanarak bir akış işlemek için [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) anahtar sözcüğü.

> [!NOTE]
> İle gönderilen iletileri bir `DeviceClient` özelliği işlenebilir.
> 

### <a name="consumer-groups"></a>Tüketici grupları
Her akış analizi IOT Hub'ı giriş kendi tüketici grubunuz için yapılandırmanız gerekir. Bir işi kendi kendine birleşim içerdiğinde ya da birden fazla giriş varsa, bazı giriş aşağı birden fazla okuyucu tarafından okuyabilir. Bu durum, tek bir tüketici grubundaki okuyucu sayısını etkiler. Her bölüm başına tüketici grubu beş okuyucuların Azure IOT Hub sınırını aşmamak için her akış analizi işi için bir tüketici grubu atamak için bir en iyi uygulamadır.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>Giriş veri akışı IOT hub'ı yapılandırma
Aşağıdaki tabloda her bir özellik açıklanmaktadır **yeni giriş** giriş akış olarak IOT hub'ı yapılandırırken Azure portalında sayfası.

| Özellik | Açıklama |
| --- | --- |
| **Giriş diğer adı** | Bu giriş başvurmak için işin sorguda kullanma kolay adı.|
| **Abonelik** | IOT hub'ı kaynak bulunduğu abonelik seçin. | 
| **IoT Hub’ı** | Giriş olarak kullanmak için IOT Hub'ın adı. |
| **uç noktası** | IOT hub'ına yönelik uç noktası.|
| **Paylaşılan erişim ilkesi adı** | IOT Hub'ına erişimi sağlayan paylaşılan erişim ilkesi. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. |
| **Paylaşılan Erişim İlkesi anahtarı** | IOT hub'ına erişim yetkisi vermek için kullanılan paylaşılan erişim anahtarı.  IOT hub'ı ayarları sağlamak için seçeneği seçmediğiniz sürece bu seçenek, otomatik olarak doldurulur el ile. |
| **Tüketici grubu** | Farklı bir tüketici grubundaki her akış analizi işi için kullanmanız önerilir. IOT Hub'ından veri alma için tüketici grubu kullanılır. Aksi belirtilmedikçe akış analizi $Default tüketici grubu kullanır.  |
| **Olayı seri hale getirme biçimi** | Seri hale getirme biçimi (JSON, CSV veya Avro) gelen veri akışının.  JSON biçimi belirtimiyle hizalar ve ondalık sayılar önüne 0 içermeyen emin olun. |
| **Kodlama** | UTF-8 anda desteklenen tek kodlama biçimi değil. |
| **Olay sıkıştırma türü** | Hiçbiri (varsayılan), GZip ve Deflate gibi gelen veri akışını okumak için kullanılan sıkıştırma türü. |


Akış verilerinden bir IOT hub'ı kullandığınızda, aşağıdaki meta veri alanları Stream Analytics sorgunuzda erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **EventProcessedUtcTime** | Olay işlendiği saati ve tarihi. |
| **EventEnqueuedUtcTime** | Olay IOT Hub tarafından alınan saat ve tarihi. |
| **PartitionID** | Giriş bağdaştırıcısı sıfır tabanlı bölüm kimliği. |
| **IoTHub.MessageId** | IOT hub'ında iki yönlü iletişim ilişkilendirmek için kullanılan kimliği. |
| **IoTHub.CorrelationId** | İleti yanıtlarını ve geri bildirim IOT hub'ın kullanılan kimliği. |
| **IoTHub.ConnectionDeviceId** | Bu ileti göndermek için kullanılan kimlik doğrulama kimliği. Bu değer, IOT Hub tarafından servicebound iletileri damgalandı. |
| **IoTHub.ConnectionDeviceGenerationId** | Bu ileti göndermek için kullanılan kimliği doğrulanmış cihaz oluşturma kimliği. Bu değer, IOT Hub tarafından servicebound iletileri damgalandı. |
| **IoTHub.EnqueuedTime** | İletinin IOT Hub tarafından ne zaman alındığı saati. |
| **IoTHub.StreamId** | Gönderen aygıt tarafından eklenen özel olay özelliği. |


## <a name="stream-data-from-blob-storage"></a>Blob depolama biriminden veri akışı
Çok miktarda bulutta depolamak için yapılandırılmamış veri ile senaryoları için Azure Blob Depolama uygun maliyetli ve ölçeklenebilir bir çözüm sunar. Blob Depolama birimindeki verileri genellikle kalan verileri olarak kabul edilir. Ancak, blob verilerini bir veri akışı akış analizi tarafından işlenebilir. 

Günlük işlem Blob Depolama girişleri akış Analizi ile kullanmak için yaygın olarak kullanılan bir senaryodur. Bu senaryoda, telemetri veri dosyaları bir sisteminden yakalanan ve ayrıştırılması ve anlamlı veri ayıklamak için işlenen gerekiyor.

Stream Analytics içinde Blob Depolama olayların varsayılan zaman damgası olan blob en son değiştirildiği zaman damgası olan `BlobLastModifiedUtcTime`. Veri yükü, kullanmalısınız durumunda bir zaman damgası kullanarak bir akış işlemek için [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) anahtar sözcüğü.

CSV biçimlendirilmiş girişleri *gerektiren* veri kümesi için alanları ve tüm üst bilgi satırı alanları tanımlamak için bir başlık satırı benzersiz olması gerekir.

Akış analizi, olay hub'ı yakalama veya IOT hub'ı Azure depolama kapsayıcısının özel uç noktası tarafından oluşturulan kaldırırken AVRO iletileri şu anda desteklemiyor.

> [!NOTE]
> Akış analizi, varolan bir blobu dosyaya ekleme içerikleri desteklemez. Akış analizi her dosya yalnızca bir kez görüntüler ve dosyayı iş veri okuma sonra gerçekleşen değişiklikleri işlenmez. En iyi bir blob dosya için tüm verileri aynı anda karşıya yüklemek ve ek yeni olaylar için farklı, yeni blob dosya eklemek için bir uygulamadır.
> 

### <a name="configure-blob-storage-as-a-stream-input"></a>Giriş akış olarak BLOB depolamayı yapılandırma 

Aşağıdaki tabloda her bir özellik açıklanmaktadır **yeni giriş** Blob Depolama giriş akış olarak yapılandırdığınızda, Azure portalında sayfası.

| Özellik | Açıklama |
| --- | --- |
| **Giriş diğer adı** | Bu giriş başvurmak için işin sorguda kullanma kolay adı. |
| **Abonelik** | IOT hub'ı kaynak bulunduğu abonelik seçin. | 
| **Depolama hesabı** | Blob dosyalarının bulunduğu depolama hesabının adı. |
| **Depolama hesabı anahtarı** | Depolama hesabıyla ilişkili gizli anahtar. Blob Depolama ayarlarını sağlama seçeneğine seçmediğiniz sürece bu seçenek, otomatik olarak doldurulur el ile. |
| **kapsayıcı** | Giriş blob kapsayıcısı. Kapsayıcılar Microsoft Azure Blob hizmetinde depolanan BLOB'lar için mantıksal bir gruplandırmasını sağlar. Azure Blob Depolama hizmetine bir blob karşıya yüklediğinde, o blob için bir kapsayıcı belirtmeniz gerekir. Ya da seçebilirsiniz **kullanım varolan** kapsayıcı veya **Yeni Oluştur** oluşturulan yeni bir kapsayıcı sağlamak için.|
| **Yol deseni** (isteğe bağlı) | Belirtilen kapsayıcı içinde BLOB'ları bulmak için kullanılan dosya yolu. Yol içinde şu üç değişkenin bir veya daha fazla örneğini belirtebilirsiniz: `{date}`, `{time}`, veya `{partition}`<br/><br/>Örnek 1: `cluster1/logs/{date}/{time}/{partition}`<br/><br/>Örnek 2: `cluster1/logs/{date}`<br/><br/>`*` Karakter yol öneki için izin verilen bir değer değil. Yalnızca geçerli <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure blob karakter</a> izin verilir. |
| **Tarih biçimi** (isteğe bağlı) | Yolu tarih değişken kullanırsanız, tarih biçimi dosyaları düzenlenir. Örnek: `YYYY/MM/DD` |
| **Saat biçimi** (isteğe bağlı) |  Yolu zaman değişken kullanırsanız, saat biçimi dosyaları düzenlenir. Şu anda desteklenen tek değerdir `HH` saat. |
| **Olayı seri hale getirme biçimi** | Seri hale getirme biçimi (JSON, CSV veya Avro) gelen veri akışının.  JSON biçimi belirtimiyle hizalar ve ondalık sayılar önüne 0 içermeyen emin olun. |
| **Kodlama** | CSV ve JSON, UTF-8 şu anda desteklenen tek kodlama biçimi içindir. |
| **Sıkıştırma** | Hiçbiri (varsayılan), GZip ve Deflate gibi gelen veri akışını okumak için kullanılan sıkıştırma türü. |

Verilerinizi bir Blob Depolama kaynaktan geldiğinde, aşağıdaki meta veri alanları Stream Analytics sorgunuzda erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **BlobName** |Olay geldiği giriş blob adı. |
| **EventProcessedUtcTime** |Olay akış analizi tarafından işlenen saat ve tarihi. |
| **BlobLastModifiedUtcTime** |Blob son değiştirilme saati ve tarihi. |
| **PartitionID** |Giriş bağdaştırıcısı sıfır tabanlı bölüm kimliği. |

Örneğin, bu alanları kullanarak, aşağıdaki örneğe benzer bir sorgu yazabilirsiniz:

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
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
