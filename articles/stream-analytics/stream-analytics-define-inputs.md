---
title: "Veri bağlantısı: veri akışı olay akışının girişlerinde | Microsoft Docs"
description: "Stream Analytics 'Girişleri' adlı bir veri bağlantısı kurma bilgi edinin. Giriş olayları veri akışından içerir ve ayrıca veri başvuru."
keywords: "veri akışı, veri bağlantısı, olay akışı"
services: stream-analytics
documentationcenter: 
author: SnehaGunda
manager: kfile
editor: cgronlun
ms.assetid: 8155823c-9dd8-4a6b-8393-34452d299b68
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 12/11/2017
ms.author: sngun
ms.openlocfilehash: e8b55269e861dc010c911491d52973b674dd50ca
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="data-connection-learn-about-data-stream-inputs-from-events-to-stream-analytics"></a>Veri bağlantısı: hakkındaki verileri Stream Analytics akış girişleri olaylardan öğrenin
Veri akış analizi işi, işin adlandırılan bir veri kaynağından alınan olayların bir akış bağlantısıdır *giriş*. Akış analizi dahil olmak üzere Azure veri kaynakları ile veri akışı, birinci sınıf tümleştirme sahip [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [Azure IOT Hub](https://azure.microsoft.com/services/iot-hub/), ve [Azure Blob Depolama](https://azure.microsoft.com/services/storage/blobs/). Bu giriş kaynaklarıyla analytics işiniz aynı Azure aboneliğinden veya farklı bir abonelik olabilir.

## <a name="data-input-types-data-stream-and-reference-data"></a>Veri türleri giriş: veri akış ve başvuru verileri
Verileri bir veri kaynağına itildiği gibi bu akış analizi işi tarafından tüketilen ve gerçek zamanlı olarak işlenen. Girişleri iki türlerine bölünen: veri akışı girişleri ve başvuru verileri girdi.

### <a name="data-stream-inputs"></a>Veri akışı girişleri
Veri akışı bir sınırsız olayları zamanla dizisidir. Akış analizi işleri, en az bir veri akış girişi eklemeniz gerekir. Olay hub'ları, IOT Hub ve Blob Depolama veri akışı giriş kaynağı olarak desteklenir. Olay hub'ları, birden çok cihazları ve Hizmetleri olay akışları toplayacak şekilde kullanılır. Bu akışlar, sosyal medya Etkinlik Akışları, hisse senedi ticareti bilgi veya algılayıcı verileri içerebilir. IOT hub'ları, nesnelerin interneti (IOT) senaryolarında bağlı aygıtlardan verileri toplamak için getirilmiştir.  BLOB Depolama günlük dosyaları gibi bir akış olarak toplu veri alma için bir giriş kaynağı kullanılabilir.  

### <a name="reference-data"></a>Başvuru verileri
Akış analizi, aynı zamanda giriş olarak bilinen destekler *başvuru verileri*. Bu, ya da statik yardımcı verilerdir veya değişen yavaş. Genellikle, bağıntı ve aramalar gerçekleştirmek için kullanılır. Örneğin, statik değerleri aramak için bir SQL birleştirme gerçekleştireceği kadar veri akış girişine başvuru verilerini de verileri birleştirebilirsiniz. Azure Blob Depolama şu anda yalnızca desteklenen giriş başvuru verileri kaynağıdır. Başvuru veri kaynağı BLOB'ları, 100 MB boyutunda sınırlıdır.

Başvuru veri girişleri oluşturmayı öğrenmek için bkz: [kullanım başvuru verileri](stream-analytics-use-reference-data.md).  

## <a name="compression"></a>Sıkıştırma

Azure Stream Analytics sıkıştırma tüm veri akışı giriş kaynakları arasında (olay hub'ları, IOT Hub ve Blob Depolama) destekler. Bu özellik için yeni bir açılır seçeneği ekler **yeni giriş** dikey isteğe bağlı olarak veri akışları sıkıştırılacak seçmenizi sağlayan Azure Portalı'nda. Şu anda desteklenen başvuru türleri are - None, GZip ve sıkıştırma Deflate. Sıkıştırma desteği başvuru verileri için kullanılabilir değil.

Avro serileştirilmesi ile sıkıştırma türünü belirtmeniz gerekmez. Giriş Avro verileri sıkıştırılmışsa saydam olarak işlenir. 

## <a name="create-data-stream-input-from-event-hubs"></a>Veri akış girişine olay hub'larından oluşturma

Azure Event Hubs sağlayan yüksek düzeyde ölçeklenebilir olay ingestors yayımlama-abonelik. Böylece işlemek ve veri bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki analiz bir event hub saniye başına milyonlarca olayı toplayabilirsiniz. Olay hub'ları ve akış analizi birlikte sağlayın, uçtan uca çözüm ile gerçek zamanlı analiz için — olay hub'ları olayları Azure'da gerçek zamanlı akış olanak tanır ve akış analizi işleri, bu olayları gerçek zamanlı işleyebilir. Örneğin, web tıklama, sensör okumaları veya çevrimiçi günlük olayları Event Hubs'a gönderebilirsiniz. Daha sonra gerçek zamanlı filtreleme, toplama ve bağıntı için giriş veri akışları olarak olay hub'ları kullanmak için akış analizi işleri oluşturabilirsiniz.

Varsayılan zaman damgası akış analizi, olay hub'ten gelen olayların olay Olay hub'ı, ama olan gelen zaman damgası olan `EventEnqueuedUtcTime`. Veri yükü, kullanmalısınız durumunda bir zaman damgası kullanarak bir akış işlemek için [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) anahtar sözcüğü.

### <a name="consumer-groups"></a>Tüketici grupları
Kendi tüketici grubu için giriş her akış analizi olay hub'ı yapılandırmanız gerekir. Bir işi kendi kendine birleşim içerdiğinde ya da birden fazla giriş varsa, bazı giriş aşağı birden fazla okuyucu tarafından okuyabilir. Bu durum, tek bir tüketici grubundaki okuyucu sayısını etkiler. Bölüm başına tüketici grubu başına beş okuyucuların olay hub'ları sınırını aşmamak için her akış analizi işi için bir tüketici grubu atamak için bir en iyi uygulamadır. Olay hub'ı başına 20 tüketici grupları sınırı yoktur. Daha fazla bilgi için bkz: [Event Hubs Programlama Kılavuzu](../event-hubs/event-hubs-programming-guide.md).

### <a name="configure-an-event-hub-as-a-data-stream-input"></a>Bir event hub giriş veri akışı yapılandırın
Aşağıdaki tabloda her bir özellik açıklanmaktadır **yeni giriş** giriş olarak bir olay hub'ı yapılandırırken Azure portaldaki dikey.

| Özellik | Açıklama |
| --- | --- |
| **Giriş diğer adı** |Bu giriş başvurmak için işin sorguda kullanma kolay adı. |
| **Hizmet veri yolu ad alanı** |Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır bir Azure hizmet veri yolu ad. Yeni bir olay hub'ı oluşturduğunuzda aynı zamanda bir hizmet veri yolu ad alanı oluşturun. |
| **Olay hub'ı adı** |Giriş olarak kullanmak için olay hub'ın adı. |
| **Olay hub'ı ilke adı** |Olay hub'ına erişimi sağlayan paylaşılan erişim ilkesi. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. |
| **Olay hub tüketici grubu** (isteğe bağlı) |Olay hub'ı verilerden alma için kullanılacak tüketici grubu. Stream Analytics işi hiç tüketici grubu belirtilmezse, varsayılan bir tüketici grubu kullanır. Her akış analizi işi için ayrı bir tüketici grubundaki kullanmanızı öneririz. |
| **Olayı seri hale getirme biçimi** |Seri hale getirme biçimi (JSON, CSV veya Avro) gelen veri akışının. |
| **Kodlama** | UTF-8 anda desteklenen tek kodlama biçimi değil. |
| **Sıkıştırma** (isteğe bağlı) | Sıkıştırma (None, GZip ve Deflate) gelen veri akışının türü. |

Verilerinizi bir event hub'ından geldiğinde, aşağıdaki meta veri alanları Stream Analytics sorgunuzda erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **EventProcessedUtcTime** |Olay akış analizi tarafından işlenen saat ve tarihi. |
| **EventEnqueuedUtcTime** |Olayın olay hub tarafından alınan saat ve tarihi. |
| **PartitionID** |Giriş bağdaştırıcısı sıfır tabanlı bölüm kimliği. |

Örneğin, bu alanları kullanarak, aşağıdaki örneğe benzer bir sorgu yazabilirsiniz:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

> [!NOTE]
> Olay hub'ı IOT Hub yollar için bir uç nokta kullanırken, IOT hub'ı kullanarak medadata erişebilir [GetMetadataPropertyValue işlevi](https://msdn.microsoft.com/en-us/library/azure/mt793845.aspx).
> 

## <a name="create-data-stream-input-from-iot-hub"></a>IOT Hub'ından veri akış girişi oluşturun
Azure IOT Hub, yüksek düzeyde ölçeklenebilir yayımlama-abone olma IOT senaryoları için en iyi duruma getirilmiş olay yutucu ' dir.

Stream Analytics bir IOT hub'ten gelen olayların varsayılan zaman damgası olan IOT hub olayı geldiği zaman damgası olan `EventEnqueuedUtcTime`. Veri yükü, kullanmalısınız durumunda bir zaman damgası kullanarak bir akış işlemek için [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) anahtar sözcüğü.

> [!NOTE]
> İle gönderilen iletileri bir `DeviceClient` özelliği işlenebilir.
> 
> 

### <a name="consumer-groups"></a>Tüketici grupları
Kendi tüketici grubu için giriş her akış analizi IOT hub'ı yapılandırmanız gerekir. Bir işi kendi kendine birleşim içerdiğinde ya da birden fazla giriş varsa, bazı giriş aşağı birden fazla okuyucu tarafından okuyabilir. Bu durum, tek bir tüketici grubundaki okuyucu sayısını etkiler. Her bölüm başına tüketici grubu beş okuyucuların Azure IOT Hub sınırını aşmamak için her akış analizi işi için bir tüketici grubu atamak için bir en iyi uygulamadır.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>Giriş veri akışı IOT hub'ı yapılandırma
Aşağıdaki tabloda her bir özellik açıklanmaktadır **yeni giriş** dikey penceresinde IOT hub'ı giriş olarak yapılandırdığınızda, Azure portalında.

| Özellik | Açıklama |
| --- | --- |
| **Giriş diğer adı** |Bu giriş başvurmak için işin sorguda kullanma kolay adı.|
| **IOT hub'ı** |Giriş olarak kullanmak için IOT hub'ın adı. |
| **Uç noktası** |IOT hub'ına yönelik uç noktası.|
| **Paylaşılan erişim ilkesi adı** |IOT hub'ına erişimi sağlayan paylaşılan erişim ilkesi. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. |
| **Paylaşılan Erişim İlkesi anahtarı** |IOT hub'ına erişim yetkisi vermek için kullanılan paylaşılan erişim anahtarı. |
| **Tüketici grubu** (isteğe bağlı) |IOT hub'ından veri alma için kullanılacak tüketici grubu. Herhangi bir tüketici grubu belirtilirse, akış analizi işi varsayılan bir tüketici grubu kullanır. Her akış analizi işi için farklı bir tüketici grubundaki kullanmanızı öneririz. |
| **Olayı seri hale getirme biçimi** |Seri hale getirme biçimi (JSON, CSV veya Avro) gelen veri akışının. |
| **Kodlama** |UTF-8 anda desteklenen tek kodlama biçimi değil. |
| **Sıkıştırma** (isteğe bağlı) | Sıkıştırma (None, GZip ve Deflate) gelen veri akışının türü. |

Verilerinizi bir IOT hub'ından geldiğinde, aşağıdaki meta veri alanları Stream Analytics sorgunuzda erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **EventProcessedUtcTime** |Olay işlendiği saati ve tarihi. |
| **EventEnqueuedUtcTime** |Olay IOT hub tarafından alınan saat ve tarihi. |
| **PartitionID** |Giriş bağdaştırıcısı sıfır tabanlı bölüm kimliği. |
| **IoTHub.MessageId** | IOT hub'ında iki yönlü iletişim ilişkilendirmek için kullanılan kimliği. |
| **IoTHub.CorrelationId** |İleti yanıtlarını ve geri bildirim IOT hub'ın kullanılan kimliği. |
| **IoTHub.ConnectionDeviceId** |Bu ileti göndermek için kullanılan kimlik doğrulama kimliği. Bu değer, IOT hub tarafından servicebound iletileri damgalandı. |
| **IoTHub.ConnectionDeviceGenerationId** |Bu ileti göndermek için kullanılan kimliği doğrulanmış cihaz oluşturma kimliği. Bu değer, IOT hub tarafından servicebound iletileri damgalandı. |
| **IoTHub.EnqueuedTime** |İletinin IOT hub tarafından ne zaman alındığı saati. |
| **IoTHub.StreamId** |Gönderen aygıt tarafından eklenen özel olay özelliği. |


## <a name="create-data-stream-input-from-blob-storage"></a>Blob depolama alanından veri akış girişi oluşturun
Çok miktarda bulutta depolamak için yapılandırılmamış veri ile senaryoları için Azure Blob Depolama uygun maliyetli ve ölçeklenebilir bir çözüm sunar. Blob Depolama birimindeki verileri genellikle kalan verileri olarak kabul edilir. Ancak, bu veri akışı akış analizi tarafından işlenebilir. Blob Depolama Stream Analytics girişle tipik bir senaryo günlük işlemesidir. Bu senaryoda, telemetri verilerini bir sisteminden yakalanan ve ayrıştırılması ve anlamlı veri ayıklamak için işlenen gerekiyor.

Stream Analytics içinde Blob Depolama olayların varsayılan zaman damgası olan blob en son değiştirildiği zaman damgası olan `BlobLastModifiedUtcTime`. Veri yükü, kullanmalısınız durumunda bir zaman damgası kullanarak bir akış işlemek için [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) anahtar sözcüğü.

CSV biçimlendirilmiş girişleri *gerektiren* veri kümesi alanlarını tanımlamak için bir başlık satırı. Ayrıca, tüm üstbilgisi satır alanları benzersiz olması gerekir.

> [!NOTE]
> Akış analizi, varolan bir blobu dosyaya ekleme içerikleri desteklemez. Akış analizi her dosya yalnızca bir kez görüntüler ve dosyayı iş veri okuma sonra gerçekleşen değişiklikleri işlenmez. En iyi bir blob dosya için tüm verileri aynı anda karşıya yüklemek ve ek yeni olaylar için farklı, yeni blob dosya eklemek için bir uygulamadır.
> 

### <a name="configure-blob-storage-as-a-data-stream-input"></a>BLOB Depolama giriş veri akışı yapılandırın

Aşağıdaki tabloda her bir özellik açıklanmaktadır **yeni giriş** giriş olarak Blob Depolama Birimi yapılandırırken Azure portaldaki dikey pencere.

| Özellik | Açıklama |
| --- | --- |
| **Giriş diğer adı** | Bu giriş başvurmak için işin sorguda kullanma kolay adı. |
| **Depolama hesabı** | Blob dosyalarının bulunduğu depolama hesabının adı. |
| **Depolama hesabı anahtarı** | Depolama hesabıyla ilişkili gizli anahtar. |
| **Kapsayıcı** | Giriş blob kapsayıcısı. Kapsayıcılar Microsoft Azure Blob hizmetinde depolanan BLOB'lar için mantıksal bir gruplandırmasını sağlar. Azure Blob Depolama hizmetine bir blob karşıya yüklediğinde, o blob için bir kapsayıcı belirtmeniz gerekir. |
| **Yol deseni** (isteğe bağlı) | Belirtilen kapsayıcı içinde BLOB'ları bulmak için kullanılan dosya yolu. Yol içinde şu üç değişkenin bir veya daha fazla örneğini belirtebilirsiniz: `{date}`, `{time}`, veya`{partition}`<br/><br/>Örnek 1:`cluster1/logs/{date}/{time}/{partition}`<br/><br/>Örnek 2:`cluster1/logs/{date}`<br/><br/>`*` Karakter yol öneki için izin verilen bir değer değil. Yalnızca geçerli <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure blob karakter</a> izin verilir. |
| **Tarih biçimi** (isteğe bağlı) | Yolu tarih değişken kullanırsanız, tarih biçimi dosyaları düzenlenir. Örnek:`YYYY/MM/DD` |
| **Saat biçimi** (isteğe bağlı) |  Yolu zaman değişken kullanırsanız, saat biçimi dosyaları düzenlenir. Şu anda desteklenen tek değerdir `HH`. |
| **Olayı seri hale getirme biçimi** | Seri hale getirme biçimi (JSON, CSV veya Avro) gelen veri akışları için. |
| **Kodlama** | CSV ve JSON, UTF-8 şu anda desteklenen tek kodlama biçimi içindir. |
| **Sıkıştırma** (isteğe bağlı) | Sıkıştırma (None, GZip ve Deflate) gelen veri akışının türü. |

Verilerinizi bir Blob Depolama kaynaktan geldiğinde, aşağıdaki meta veri alanları Stream Analytics sorgunuzda erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **BlobName** |Olay geldiği giriş blob adı. |
| **EventProcessedUtcTime** |Olay akış analizi tarafından işlenen saat ve tarihi. |
| **BlobLastModifiedUtcTime** |Blob son değiştirilme saati ve tarihi. |
| **PartitionID** |Giriş bağdaştırıcısı sıfır tabanlı bölüm kimliği. |

Örneğin, bu alanları kullanarak, aşağıdaki örneğe benzer bir sorgu yazabilirsiniz:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
Azure veri bağlantı seçenekleri hakkında Stream Analytics işleri öğrendiniz. Stream Analytics hakkında daha fazla bilgi için bkz:

* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
