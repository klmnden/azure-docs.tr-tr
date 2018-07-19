---
title: Azure IOT hub'ı yerleşik uç anlama | Microsoft Docs
description: Geliştirici Kılavuzu - yerleşik, Event Hub ile uyumlu uç noktası o CİHAZDAN buluta iletileri kullanmayı açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: 912bb0dd3e48e53134ad848119ae7428b380b88d
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39124945"
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>CİHAZDAN buluta iletilerini yerleşik uç noktadan okuma

Varsayılan olarak, iletiler yerleşik hizmet'e yönelik uç noktaya yönlendirilir (**iletiler/olaylar**) ile uyumlu [Event Hubs][lnk-event-hubs]. Bu uç noktası şu anda yalnızca sunulan kullanmaktır [AMQP] [ lnk-amqp] 5671 numaralı bağlantı noktasında protokolü. IOT hub'ı yerleşik Event Hub ile uyumlu Mesajlaşma uç denetim sağlamak için aşağıdaki özellikleri sunan **iletiler/olaylar**.

| Özellik            | Açıklama |
| ------------------- | ----------- |
| **Bölüm sayısı** | Oluşturma sırasında sayısını tanımlamak için bu özelliği ayarlayın [bölümleri] [ lnk-event-hub-partitions] CİHAZDAN buluta olay alımı için. |
| **Elde tutma süresi**  | Bu özellik ne kadar süreyle iletileri IOT Hub tarafından korunur gün cinsinden belirtir. Varsayılan bir gündür, ancak yedi gün olarak artırılabilir. |

IOT Hub ayrıca yönetmenize imkan sağlar tüketici grupları yerleşik cihaz-bulut uç noktası alırsınız.

Varsayılan olarak, ileti yönlendirme kuralı açıkça eşleşmeyen tüm iletilerini yerleşik uç noktasına yazılır. Bu geri dönüş rota devre dışı bırakırsanız, açıkça herhangi bir ileti yönlendirme kuralları eşleşmeyen iletiler bırakılır.

Elde tutma süresi aşağıdakilerden birini kullanarak program aracılığıyla değiştirebilirsiniz [IOT hub'ı kaynak sağlayıcısı REST API'leri][lnk-resource-provider-apis], veya [Azure portalında] [ lnk-management-portal].

IOT hub'ı sunan **iletiler/olaylar** , hub tarafından alınan CİHAZDAN buluta iletileri okumak, arka uç Hizmetleri için yerleşik uç nokta. Bu uç nokta olaydır mekanizmalarının kullanmak için Event Hubs hizmeti sağlayan Hub ile uyumlu iletileri okumak için destekler.

## <a name="read-from-the-built-in-endpoint"></a>Yerleşik uç noktadan okuma

Kullanırken [.NET için Azure hizmet veri yolu SDK'sı] [ lnk-servicebus-sdk] veya [Event Hubs - olay işleyicisi ana bilgisayarı][lnk-eventprocessorhost], herhangi bir IOT Hub bağlantı kullanabilirsiniz. dizeleri doğru izinlere sahip. Ardından **iletiler/olaylar** olay hub'ı adı.

SDK'ları (veya ürün tümleştirmeleri) kullandığınızda, IOT hub'farkında olan, bir Event Hub ile uyumlu uç nokta ve Event Hub ile uyumlu adı almanız gerekir:

1. Oturum [Azure portalında] [ lnk-management-portal] ve IOT hub'ınıza gidin.
1. **Uç Noktalar**’a tıklayın.
1. İçinde **yerleşik uç noktaları** bölümünde **olayları**. 
1. Aşağıdaki değerleri içeren bir özellik sayfasını açar: **Event Hub ile uyumlu uç nokta**, **Event Hub ile uyumlu adı**, **bölümleri**,  **Elde tutma süresi**, ve **tüketici grupları**.

    ![Cihazdan buluta ayarları][img-eventhubcompatible]

IOT Hub SDK'sı, IOT Hub uç nokta adı gerektirir **iletiler/olaylar** altında gösterildiği **uç noktaları**.

Kullanmakta olduğunuz SDK'sı gerektiriyorsa bir **Hostname** veya **Namespace** değeri, düzeninden Kaldır **Event Hub ile uyumlu uç nokta**. Örneğin, Event Hub ile uyumlu uç noktanızı ise **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, **Hostname** olacaktır  **ıothub ns myiothub 1234.servicebus.windows.net**. **Namespace** olacaktır **iothub-ns-myiothub-1234**.

Daha sonra olan herhangi bir paylaşılan erişim ilkesinin kullanabilirsiniz **ServiceConnect** belirtilen olay Hub'ına bağlanmak için izinleri.

Önceki bilgileri kullanarak bir olay hub'ı bağlantı dizesi oluşturmak gerekiyorsa şu biçimi kullanın:

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

SDK'ları ve IOT hub'ı sunan Event Hub ile uyumlu uç noktalar ile kullanabileceğiniz tümleştirmeler, aşağıdaki listedeki öğeleri içerir:

* [Event Hubs Java istemci](https://github.com/Azure/azure-event-hubs-java).
* [Apache Storm spout](../hdinsight/storm/apache-storm-develop-csharp-event-hub-topology.md). Görüntüleyebileceğiniz [kaynak spout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) GitHub üzerinde.
* [Apache Spark tümleştirme](../hdinsight/spark/apache-spark-eventhub-streaming.md).

## <a name="next-steps"></a>Sonraki adımlar

IOT Hub uç noktaları hakkında daha fazla bilgi için bkz: [IOT Hub uç noktaları][lnk-endpoints].

[Hızlı Başlangıçlar] [ lnk-get-started] sanal cihazlardan CİHAZDAN buluta iletiler gönderir ve iletilerini yerleşik uç noktadan okuma işlemini gösterir. Daha fazla ayrıntı için [yolları kullanarak işlem IOT Hub CİHAZDAN buluta iletileri] [ lnk-d2c-tutorial] öğretici.

Özel uç noktalar için CİHAZDAN buluta iletileri yönlendirmek istiyorsanız, bkz. [CİHAZDAN buluta iletiler için ileti yollarını ve özel uç noktaları kullanma][lnk-custom].

[img-eventhubcompatible]: ./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png

[lnk-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-get-started]: quickstart-send-telemetry-node.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-management-portal]: https://portal.azure.com
[lnk-d2c-tutorial]: tutorial-routing.md
[lnk-event-hub-partitions]: ../event-hubs/event-hubs-features.md#partitions
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx
[lnk-amqp]: https://www.amqp.org/
