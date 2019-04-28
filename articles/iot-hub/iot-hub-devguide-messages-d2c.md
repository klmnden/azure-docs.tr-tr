---
title: Azure IOT Hub ileti yönlendirme anlama | Microsoft Docs
description: Geliştirici Kılavuzu - ileti yönlendirme CİHAZDAN buluta iletileri göndermeye kullanma. Telemetri hem olmayan telemetri verileri gönderme hakkında bilgi içerir.
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: asrastog
ms.openlocfilehash: dc5bfe6b431659b7b99140eb29a0e64922a42275
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61364510"
---
# <a name="use-iot-hub-message-routing-to-send-device-to-cloud-messages-to-different-endpoints"></a>Farklı uç noktalar için CİHAZDAN buluta iletileri göndermek için IOT Hub ileti yönlendirme kullanın

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

İleti yönlendirme, otomatik, ölçeklenebilir ve güvenilir bir şekilde bulut Hizmetleri için cihazlarınızdan alınan iletileri göndermesini sağlar. İleti yönlendirme için kullanılabilir: 

* **Cihazın telemetri iletilerini yanı sıra olayları gönderme** yani, cihaz yaşam döngüsü olayları ve değişiklik olayları uç yerleşik ve özel uç noktalar için cihaz ikizi. Hakkında bilgi edinin [yönlendirme uç noktaları](#routing-endpoints).

* **Çeşitli uç noktalar ile yönlendirme önce verileri filtreleme** zengin sorguların uygulayarak. İleti yönlendirme sorgu ileti özellikleri ve ileti gövdesi hem de cihaz ikizi etiketleri ve cihaz ikizi özelliklerini sağlar. Kullanma hakkında daha fazla bilgi edinin [sorguları içinde ileti yönlendirme](iot-hub-devguide-routing-query-syntax.md).

IOT Hub ileti yönlendirme çalışmak için bu hizmet uç noktaları yazma erişimi olmalıdır. Azure portalı üzerinden uç noktalarınızı yapılandırırsanız sizin için gerekli izinleri eklenir. Hizmetlerinizi, beklenen aktarım hızıyla destekleyecek şekilde yapılandırdığınızdan emin olun. IOT çözümünüzü ilk kez yapılandırırken, ek uç noktalar izleyin ve gerçek yükleme için gerekli ayarlamaları yapmak gerekebilir.

IOT hub'ı tanımlayan bir [ortak biçimi](iot-hub-devguide-messages-construct.md) CİHAZDAN buluta tüm protokoller üzerinde birlikte çalışabilirlik için Mesajlaşma için. IOT Hub iletisi için bu endpoint bir ileti aynı uç noktaya işaret eden yolların eşleşirse, yalnızca bir kez sunar. Bu nedenle, Service Bus kuyruğuna veya konusuna üzerinde yinelenenleri kaldırmayı yapılandırma gerekmez. Bölümlenmiş kuyruklar bölüm benzeşim mesaj sıralama garanti eder. Bilgi edinmek için bu öğreticiyi kullanmak için nasıl [ileti yönlendirmeyi yapılandırma](tutorial-routing.md).

## <a name="routing-endpoints"></a>Yönlendirme uç noktaları

IOT hub'ı varsayılan yerleşik-all-ın-bitiş noktası vardır (**iletiler/olaylar**) Event Hubs ile uyumlu. Oluşturabileceğiniz [özel uç noktalar](iot-hub-devguide-endpoints.md#custom-endpoints) rota iletileri IOT Hub'ına aboneliğinizde diğer hizmetler arasında bağlantı kurarak. IOT hub'ı, şu anda özel uç noktalar şu Hizmetleri destekler:

### <a name="built-in-endpoint"></a>Yerleşik uç noktası

Standart kullanabileceğiniz [Event Hubs tümleştirme ve SDK'ları](iot-hub-devguide-messages-read-builtin.md) CİHAZDAN buluta iletilerini yerleşik uç noktadan almak için (**iletiler/olaylar**). Bir rota oluşturulduktan sonra bir rota için bu endpoint oluşturulmadıkça dahili-all-ın-uç noktaya akan verileri durdurur.

### <a name="azure-blob-storage"></a>Azure Blob Depolama

IOT hub'ın desteklediği verileri Azure Blob depolama alanına yazma [Apache Avro](https://avro.apache.org/) JSON biçimine yanı sıra. IOT hub'ı Doğu ABD, Batı ABD ve Batı Avrupa, kullanılabilir tüm bölgelerde önizleme özelliği JSON biçiminde kodlamak için kullanılabilir. AVRO varsayılandır. Kodlama biçimi, yalnızca blob depolama uç noktası yapılandırıldığında ayarlanabilir. Biçim için mevcut bir uç nokta düzenlenemez. JSON encoding kullanıldığında, JSON ve UTF-8 contentEncoding iletisinde contentType ayarlamalısınız [Sistem Özellikleri](iot-hub-devguide-routing-query-syntax.md#system-properties). Özellikle IOT hub'ı oluşturma veya güncelleştirme REST API kullanarak kodlama biçimi seçebilirsiniz [RoutingStorageContainerProperties](https://docs.microsoft.com/rest/api/iothub/iothubresource/createorupdate#routingstoragecontainerproperties), Azure Portalı'nda, [Azure CLI](https://docs.microsoft.com/cli/azure/iot/hub/routing-endpoint?view=azure-cli-latest) veya [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.iothub/add-aziothubroutingendpoint?view=azps-1.3.0). Aşağıdaki diyagramda, kodlama biçimi seçin Azure Portalı'nda gösterilmektedir.

![BLOB Depolama uç noktası kodlama](./media/iot-hub-devguide-messages-d2c/blobencoding.png)

IOT Hub iletilerini toplu işlemleri ve her toplu işin belirli bir boyuta ulaştığında veya belirli bir süre geçtikten verileri bir blob olarak yazar. IOT hub'ı varsayılan olarak aşağıdaki dosya adlandırma kuralı:

```
{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}
```

Ancak, listelenen tüm belirteçleri kullanmanız gerekir, hiçbir dosya adlandırma kuralını kullanabilirsiniz. Yazılacak veri yoksa IOT hub'ı boş bir bloba yazılacaktır.

BLOB depolamaya yönlendirme, BLOB'ları kaydetme ve ardından tüm kapsayıcıları bölümünün varsayımlar yapmadan okunur emin olmak için bunları üzerinde yineleme öneririz. Bölüm aralığı sırasında olası değişebilir bir [Microsoft tarafından başlatılan bir yük devretme](iot-hub-ha-dr.md#microsoft-initiated-failover) veya IOT hub'ı [el ile yük devretme](iot-hub-ha-dr.md#manual-failover-preview). Kullanabileceğiniz [listesi Blobları API'sini](https://docs.microsoft.com/rest/api/storageservices/list-blobs) BLOB listesini numaralandırılamadı. Lütfen aşağıdaki örneği kılavuz bakın.

   ```csharp
        public void ListBlobsInContainer(string containerName, string iothub)
        {
            var storageAccount = CloudStorageAccount.Parse(this.blobConnectionString);
            var cloudBlobContainer = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
            if (cloudBlobContainer.Exists())
            {
                var results = cloudBlobContainer.ListBlobs(prefix: $"{iothub}/");
                foreach (IListBlobItem item in results)
                {
                    Console.WriteLine(item.Uri);
                }
            }
        }
   ```

### <a name="service-bus-queues-and-service-bus-topics"></a>Service Bus kuyrukları ve Service Bus konuları

Service Bus kuyrukları ve konuları IOT Hub uç noktaları değil olarak kullanılan **oturumları** veya **yinelenen algılama** etkin. Bu seçeneklerden birini etkinse, uç nokta olarak görünür **ulaşılamıyor** Azure portalında.

### <a name="event-hubs"></a>Event Hubs

Dahili Event hub uyumlu uç dışında veri Event Hubs türünde özel uç noktalar için de yönlendirebilirsiniz. 

## <a name="reading-data-that-has-been-routed"></a>Yönlendirilmiş verileri okuma

Bir yolu takip ederek yapılandırabileceğiniz [öğretici](tutorial-routing.md).

İleti bir uç noktasından okumak hakkında bilgi edinmek için aşağıdaki öğreticileri kullanın.

* Okuma [dahili uç noktası](quickstart-send-telemetry-node.md)

* Okuma [Blob Depolama](../storage/blobs/storage-blob-event-quickstart.md)

* Okuma [olay hub'ları](../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)

* Okuma [hizmet veri yolu kuyrukları](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md)

* Okuma [hizmet veri yolu konuları](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)

## <a name="fallback-route"></a>Temel yol

Geri dönüş rota herhangi bir yerleşik olay hub'ları için mevcut yolları sorgu koşulları karşılamayan tüm iletiler gönderir (**iletiler/olaylar**), diğer bir deyişle uyumlu [Event Hubs](/azure/event-hubs/). İleti yönlendirme açıksa, geri dönüş rota özelliğini etkinleştirebilirsiniz. Bir rota oluşturulduktan sonra bir rota için bu endpoint yapılandırılmadığı sürece veri dahili-all-ın-uç noktaya akan durdurur. Dahili-içeren uç noktası için yol yok ve bir geri dönüş yol etkin yollar üzerindeki sorgu koşulları eşleşmeyen iletiler dahili-all-ın-uç noktasına gönderilir. Ayrıca, mevcut tüm yolları silinirse, dahili-all-ın-uç noktasında tüm verileri almak için geri dönüş rota etkinleştirilmelidir. 

Etkinleştirebilir / Azure geri dönüş yolda devre dışı bırakabilir Portal dikey penceresinde ileti yönlendirme ->. Azure Resource Manager için de kullanabilirsiniz [FallbackRouteProperties](/rest/api/iothub/iothubresource/createorupdate#fallbackrouteproperties) özel uç nokta için geri dönüş yolu kullanmak için.

## <a name="non-telemetry-events"></a>Olmayan telemetri olayları

Cihaz telemetrisini yanı sıra ileti yönlendirme ayrıca gönderen cihazın ikiz değişiklik olayları ve cihaz yaşam döngüsü olayları sağlar. Örneğin, bir yol ayarlamak veri kaynağı ile oluşturulursa **cihaz ikiz değişiklik olayları**, IOT Hub cihaz ikizinde değişikliği içeren uç noktasına iletileri gönderir. Benzer şekilde, bir yol ayarlamak veri kaynağı ile oluşturulursa **cihaz yaşam döngüsü olaylarını**, IOT Hub cihaz oluşturulan veya silinmiş olup olmadığını belirten bir ileti gönderir. 

[IOT Hub ayrıca Azure Event Grid ile tümleşir](iot-hub-event-grid.md) gerçek zamanlı tümleştirmeler ve bu etkinliklere göre iş akışı Otomasyonu desteklemek için cihaz olaylarını yayımlamak için. Anahtar bkz [ileti yönlendirme ve Event Grid arasındaki farklar](iot-hub-event-grid-routing-comparison.md) senaryonuz için en iyi çalıştığı öğrenin.

## <a name="testing-routes"></a>Test yolları

Yeni bir rota oluşturduğunuzda veya var olan bir rota, örnek bir ileti ile rota sorgu test etmelisiniz. Tekil yollar test ya da aynı anda tüm yolları test edin ve ileti, test sırasında Uç noktalara yönlendirilir. Azure portalı, Azure Resource Manager, Azure PowerShell ve Azure CLI'yı test etmek için kullanılabilir. Sonuçlar örnek ileti sorguyla eşleşen, ileti sorguyu eşleşmedi veya örnek ileti veya sorgu söz dizimi yanlış olduğundan test çalıştırılamıyor olup olmadığını belirlemenize yardımcı olur. Daha fazla bilgi için bkz. [Test rota](/rest/api/iothub/iothubresource/testroute) ve [tüm yolları Test](/rest/api/iothub/iothubresource/testallroutes).

## <a name="latency"></a>Gecikme süresi

CİHAZDAN buluta telemetri iletilerini yerleşik uç noktalarını kullanarak yönlendirdiğinizde ilk yolun oluşturulduktan sonra artmasına uçtan uca gecikme yoktur.

Çoğu durumda, 500 MS'den az gecikme süresi ortalama artış olur. Gecikme süresi kullanarak izleyebileceğiniz **yönlendirme: ileti iletiler/olaylar için gecikme süresini** veya **d2c.endpoints.latency.builtIn.events** IOT hub'ı ölçümü. Uçtan uca gecikme süresi, ilk öğe sonra hiçbir yolu silmesini veya yaratmasını etkilemez.

## <a name="monitoring-and-troubleshooting"></a>İzleme ve sorun giderme

Uç nokta ilgili ölçümleri size gönderilen iletiler ve hub'a durumunu genel bakış sağlayacak ve çeşitli yönlendirme IOT hub'ı sağlar. Sorunların kök nedenini belirlemek için birden çok Ölçüm bilgilerini birleştirebilirsiniz. Örneğin, ölçümünü kullanın **yönlendirme: telemetri iletilerini bırakılan** veya **d2c.telemetry.egress.dropped** yolların herhangi birine sorgular ile eşleşmedi, bırakılan ileti sayısını belirlemek için ve geri dönüş rota devre dışı bırakıldı. [IOT hub'ı ölçümleri](iot-hub-metrics.md) IOT Hub'ınız için varsayılan olarak etkin olan tüm ölçümleri listeler.

REST API kullanabilirsiniz [uç nokta sistem durumu alma](https://docs.microsoft.com/de-de/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) almak için [sistem durumu](iot-hub-devguide-endpoints.md#custom-endpoints) uç nokta. Kullanmanızı öneririz [IOT hub'ı ölçümleri](iot-hub-metrics.md) tanımlamak ve uç nokta sistem durumu ölü ya da sistem durumu kötü olduğunda, hatalarını ayıklamanıza yönlendirme ileti gecikmesi için ilgili. Örneğin, uç nokta türü Event Hubs izleyebilirsiniz **d2c.endpoints.latency.eventHubs**. IOT Hub durumu sonunda tutarlı bir duruma olduğunda sağlıksız bir uç nokta durumunu iyi durumda olacak şekilde güncelleştirilir.

Kullanarak **yollar** tanılama günlüklerine yönelik Azure İzleyicisi'nde [tanılama ayarları](../iot-hub/iot-hub-monitor-resource-health.md), örneğin IOT Hub tarafından algılanan gibi yönlendirme sorgu ve uç nokta sistem durumu değerlendirmesi sırasında oluşan parçaları hataları olabilir ne zaman bir uç nokta etkin değil. Bu tanılama günlüklerini Azure İzleyici günlüklerine, Event Hubs veya Azure depolama için özel işleme gönderilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* İleti yollarını nasıl oluşturulacağını öğrenmek için bkz: [yolları kullanarak işlem IOT Hub CİHAZDAN buluta iletileri](tutorial-routing.md).

* [CİHAZDAN buluta iletiler gönderme](quickstart-send-telemetry-node.md)

* CİHAZDAN buluta iletileri göndermek için kullanabileceğiniz SDK'lar hakkında bilgi için bkz. [Azure IOT SDK'ları](iot-hub-devguide-sdks.md).
