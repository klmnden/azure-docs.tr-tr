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
ms.openlocfilehash: abc32b726eea55f08a052f29a12f1eb237d4f5d6
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2018
ms.locfileid: "49311329"
---
# <a name="use-message-routing-to-send-device-to-cloud-messages-to-different-endpoints"></a>Farklı uç noktalar için CİHAZDAN buluta iletileri göndermek için ileti yönlendirme kullanın

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

İleti yönlendirme, otomatik, ölçeklenebilir ve güvenilir bir şekilde bulut Hizmetleri için cihazlarınızdan alınan iletileri göndermesini sağlar. İleti yönlendirme için kullanılabilir: 

* **Cihazın telemetri iletileri ve bunun yanı sıra olayları gönderme** yani, cihaz yaşam döngüsü olayları ve değişiklik olayları uç yerleşik ve özel uç noktalar için cihaz ikizi. Hakkında bilgi edinin [yönlendirme uç noktaları](#routing-endpoints).

* **Çeşitli uç noktalar ile yönlendirme önce verileri filtreleme** zengin sorguların uygulayarak. İleti yönlendirme sorgu ileti özellikleri ve ileti gövdesi hem de cihaz ikizi etiketleri ve cihaz ikizi özelliklerini sağlar. Kullanma hakkında daha fazla bilgi edinin [sorguları içinde ileti yönlendirme](iot-hub-devguide-routing-query-syntax.md).

IOT Hub ileti yönlendirme çalışmak için bu hizmet uç noktaları yazma erişimi olmalıdır. Azure portalı üzerinden uç noktalarınızı yapılandırırsanız sizin için gerekli izinleri eklenir. Hizmetlerinizi, beklenen aktarım hızıyla destekleyecek şekilde yapılandırdığınızdan emin olun. IOT çözümünüzü ilk kez yapılandırırken, ek uç noktalar izleyin ve gerçek yükleme için gerekli ayarlamaları yapmak gerekebilir.

IOT hub'ı tanımlayan bir [ortak biçimi](iot-hub-devguide-messages-construct.md) için tüm cihaz-bulut Mesajlaşma için interoperatbility protokolleri. IOT Hub iletisi için bu endpoint bir ileti aynı uç noktaya işaret eden yolların eşleşirse, yalnızca bir kez sunar. Bu nedenle, Service Bus kuyruğuna veya konusuna üzerinde yinelenenleri kaldırmayı yapılandırma gerekmez. Bölümlenmiş kuyruklar bölüm benzeşim mesaj sıralama garanti eder. [İleti yönlendirme] nasıl yapılandırılacağını öğrenmek için bu öğreticiyi kullanın (öğretici routing.md).

## <a name="routing-endpoints"></a>Yönlendirme uç noktaları

IOT hub'ı varsayılan yerleşik-all-ın-bitiş noktası vardır (**iletiler/olaylar**) Event Hubs ile uyumlu. Oluşturabileceğiniz [özel uç noktalar](iot-hub-devguide-endpoints.md#custom-endpoints) rota iletileri IOT Hub'ına aboneliğinizde diğer hizmetler arasında bağlantı kurarak. IOT hub'ı, şu anda özel uç noktalar şu Hizmetleri destekler:

### <a name="built-in-endpoint"></a>Yerleşik uç noktası

Standart kullanabileceğiniz [Event Hubs tümleştirme ve SDK'ları](iot-hub-devguide-messages-read-builtin.md) CİHAZDAN buluta iletilerini yerleşik uç noktadan almak için (**iletiler/olaylar**). Bir rota oluşturulduktan sonra verileri bir rota için bu endpoint oluşturulmadıkça dahili-all-ın-uç noktaya akan durduracağını unutmayın.

### <a name="azure-blob-storage"></a>Azure Blob Depolama

IOT hub'ı yalnızca destekler verileri Azure Blob depolama alanına yazma [Apache Avro](http://avro.apache.org/) biçimi. IOT Hub iletilerini toplu işlemleri ve her toplu işin belirli bir boyuta ulaştığında veya belirli bir süre geçtikten verileri bir blob olarak yazar.

IOT hub'ı varsayılan olarak aşağıdaki dosya adlandırma kuralı:

```
{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}
```

Ancak, listelenen tüm belirteçleri kullanmanız gerekir, hiçbir dosya adlandırma kuralını kullanabilirsiniz. Yazılacak veri yoksa IOT hub'ı boş bir bloba yazılacaktır.

### <a name="service-bus-queues-and-service-bus-topics"></a>Service Bus kuyrukları ve Service Bus konuları

Service Bus kuyrukları ve konuları IOT Hub uç noktaları değil olarak kullanılan **oturumları** veya **yinelenen algılama** etkin. Bu seçeneklerden birini etkinse, uç nokta olarak görünür **ulaşılamıyor** Azure portalında.

### <a name="event-hubs"></a>Event Hubs

Dahili Event hub uyumlu uç dışında veri Event Hubs türünde özel uç noktalar için de yönlendirebilirsiniz. 

Yönlendirme ve özel uç noktaları kullandığınızda, tüm kuralları eşleşmiyorsa iletileri yalnızca yerleşik uç noktasına gönderilir. Özel uç noktalar ve yerleşik uç nokta iletileri ulaştırmak üzere olaylar uç noktasına iletileri gönderen bir rota ekleyin.

## <a name="reading-data-that-has-been-routed"></a>Yönlendirilmiş verileri okuma

Bir yolu takip ederek yapılandırabileceğiniz [öğretici](tutorial-routing.md).

İleti bir uç noktasından okumak hakkında bilgi edinmek için aşağıdaki öğreticileri kullanın.

* Okuma [dahili uç noktası](quickstart-send-telemetry-node.md)

* Okuma [Blob Depolama](../storage/blobs/storage-blob-event-quickstart.md)

* Okuma [olay hub'ları](../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)

* Okuma [hizmet veri yolu kuyrukları](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md)

* Okuma [hizmet veri yolu konuları](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)

## <a name="fallback-route"></a>Temel yol

Geri dönüş rota herhangi bir yerleşik olay hub'ları için mevcut yolları sorgu koşulları karşılamayan tüm iletiler gönderir (**iletiler/olaylar**), diğer bir deyişle uyumlu [Event Hubs](/azure/event-hubs/). İleti yönlendirme açıksa, geri dönüş rota özelliğini etkinleştirebilirsiniz. Bir rota için bu endpoint yapılandırılmadığı sürece bir rota oluşturulduktan sonra veri dahili-all-ın-uç noktaya akan durdurur unutmayın. Dahili-içeren uç noktası için yol yok ve bir geri dönüş yol etkin yollar üzerindeki sorgu koşulları eşleşmeyen iletiler dahili-all-ın-uç noktasına gönderilir. Ayrıca, mevcut tüm yolları silinirse, dahili-all-ın-uç noktasında tüm verileri almak için geri dönüş rota etkinleştirilmelidir. 

Etkinleştirebilir / Azure geri dönüş yolda devre dışı bırakabilir Portal dikey penceresinde ileti yönlendirme ->. Azure Resource Manager için de kullanabilirsiniz [FallbackRouteProperties](/rest/api/iothub/iothubresource/createorupdate#fallbackrouteproperties) özel uç nokta için geri dönüş yolu kullanmak için.

## <a name="non-telemetry-events"></a>Olmayan telemetri olayları

Cihaz telemetrisini yanı sıra ileti yönlendirme ayrıca gönderen cihazın ikiz değişiklik olayları ve cihaz yaşam döngüsü olayları sağlar. Örneğin, bir yol ayarlamak veri kaynağı ile oluşturulursa **cihaz ikiz değişiklik olayları**, IOT Hub cihaz ikizinde değişikliği içeren uç noktasına iletileri gönderir. Benzer şekilde, bir yol ayarlamak veri kaynağı ile oluşturulursa **cihaz yaşam döngüsü olaylarını**, IOT Hub cihaz oluşturulan veya silinmiş olup olmadığını belirten bir ileti gönderir. 

[IOT Hub ayrıca Azure Event Grid ile tümleşir](iot-hub-event-grid.md) gerçek zamanlı tümleştirmeler ve bu etkinliklere göre iş akışı Otomasyonu desteklemek için cihaz olaylarını yayımlamak için. Anahtar bkz [ileti yönlendirme ve Event Grid arasındaki farklar](iot-hub-event-grid-routing-comparison.md) senaryonuz için en iyi çalıştığı öğrenin.

## <a name="testing-routes"></a>Test yolları

Yeni bir rota oluşturduğunuzda veya var olan bir rota, örnek bir ileti ile rota sorgu test etmelisiniz. Tekil yollar test ya da aynı anda tüm yolları test edin ve ileti, test sırasında Uç noktalara yönlendirilir. Azure portalı, Azure Resource Manager, Azure PowerShell ve Azure CLI'yı test etmek için kullanılabilir. Sonuçlar örnek ileti sorguyla eşleşen, ileti sorguyu eşleşmedi veya örnek ileti veya sorgu söz dizimi yanlış olduğundan test çalıştırılamıyor olup olmadığını belirlemenize yardımcı olur. Daha fazla bilgi için bkz. [Test rota](/rest/api/iothub/iothubresource/testroute) ve [tüm yolları Test](/rest/api/iothub/iothubresource/testallroutes).

## <a name="latency"></a>Gecikme süresi

CİHAZDAN buluta telemetri iletilerini yerleşik uç noktalarını kullanarak yönlendirdiğinizde ilk yolun oluşturulduktan sonra artmasına uçtan uca gecikme yoktur.

Çoğu durumda, ortalama gecikme küçüktür 500ms artıştır. Gecikme süresi kullanarak izleyebileceğiniz **yönlendirme: ileti iletiler/olaylar için gecikme süresini** veya **d2c.endpoints.latency.builtIn.events** IOT hub'ı ölçümü. Uçtan uca gecikme süresi, ilk öğe sonra hiçbir yolu silmesini veya yaratmasını etkilemez.

## <a name="monitoring-and-troubleshooting"></a>İzleme ve sorun giderme

Uç nokta ilgili ölçümleri size gönderilen iletiler ve hub'a durumunu genel bakış sağlayacak ve çeşitli yönlendirme IOT hub'ı sağlar. Sorunların kök nedenini belirlemek için birden çok Ölçüm bilgilerini birleştirebilirsiniz. Örneğin ölçümünü kullanın **yönlendirme: telemetri iletilerini bırakılan** veya **d2c.telemetry.egress.dropped** yolların herhangi birine sorgular ile eşleşmedi, bırakılan ileti sayısını belirlemek için ve geri dönüş rota devre dışı bırakıldı. [IOT hub'ı ölçümleri](iot-hub-metrics.md) IOT Hub'ınız için varsayılan olarak etkin olan tüm ölçümleri listeler.

Kullanarak **yollar** tanılama günlüklerine yönelik Azure İzleyicisi'nde [tanılama ayarları](../iot-hub/iot-hub-monitor-resource-health.md), örneğin IOT Hub tarafından algılanan gibi yönlendirme sorgu ve uç nokta sistem durumu değerlendirmesi sırasında oluşan parçaları hataları olabilir ne zaman bir uç nokta etkin değil. Bu tanılama günlüklerini Log Analytics, Event Hubs veya Azure depolama için özel işleme gönderilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* İleti yollarını nasıl oluşturulacağını öğrenmek için bkz: [yolları kullanarak işlem IOT Hub CİHAZDAN buluta iletileri](tutorial-routing.md).

* [CİHAZDAN buluta iletiler gönderme](quickstart-send-telemetry-node.md)

* CİHAZDAN buluta iletileri göndermek için kullanabileceğiniz SDK'lar hakkında bilgi için bkz. [Azure IOT SDK'ları](iot-hub-devguide-sdks.md).
