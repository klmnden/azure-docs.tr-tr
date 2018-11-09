---
title: Azure IOT hub'ı yerleşik uç anlama | Microsoft Docs
description: Geliştirici Kılavuzu - CİHAZDAN buluta iletileri okumak için yerleşik, Event Hub ile uyumlu uç noktası kullanma işlemini açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: 02ea4b94f8d1442360bebb36fdbba13d973f8555
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242424"
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Cihazdan buluta iletilerini yerleşik uç noktadan okuma

Varsayılan olarak, iletiler yerleşik hizmet'e yönelik uç noktaya yönlendirilir (**iletiler/olaylar**) ile uyumlu [Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/
). Bu uç noktası şu anda yalnızca sunulan kullanmaktır [AMQP](https://www.amqp.org/) 5671 numaralı bağlantı noktasında protokolü. IOT hub'ı yerleşik Event Hub ile uyumlu Mesajlaşma uç denetim sağlamak için aşağıdaki özellikleri sunan **iletiler/olaylar**.

| Özellik            | Açıklama |
| ------------------- | ----------- |
| **Bölüm sayısı** | Oluşturma sırasında sayısını tanımlamak için bu özelliği ayarlayın [bölümleri](../event-hubs/event-hubs-features.md#partitions) CİHAZDAN buluta olay alımı için. |
| **Elde tutma süresi**  | Bu özellik ne kadar süreyle iletileri IOT Hub tarafından korunur gün cinsinden belirtir. Varsayılan bir gündür, ancak yedi gün olarak artırılabilir. |

IOT Hub ayrıca yönetmenize imkan sağlar tüketici grupları yerleşik cihaz-bulut uç noktası alırsınız.

Kullanıyorsanız [ileti yönlendirme](iot-hub-devguide-messages-d2c.md) ve [geri dönüş rota](iot-hub-devguide-messages-d2c.md#fallback-route) olan etkin bir sorgu tüm yönlendiricilerin eşleşmeyen tüm iletileri yerleşik uç noktasına yazılır. Bu geri dönüş rota devre dışı bırakırsanız, herhangi bir sorgu eşleşmeyen iletiler bırakılır.

Elde tutma süresi aşağıdakilerden birini kullanarak program aracılığıyla değiştirebilirsiniz [IOT hub'ı kaynak sağlayıcısı REST API'leri](/rest/api/iothub/iothubresource), veya [Azure portalında](https://portal.azure.com).

IOT hub'ı sunan **iletiler/olaylar** , hub tarafından alınan CİHAZDAN buluta iletileri okumak, arka uç Hizmetleri için yerleşik uç nokta. Bu uç nokta olaydır mekanizmalarının kullanmak için Event Hubs hizmeti sağlayan Hub ile uyumlu iletileri okumak için destekler.

## <a name="read-from-the-built-in-endpoint"></a>Yerleşik uç noktadan okuma

Kullanırken [.NET için Azure hizmet veri yolu SDK'sı](https://www.nuget.org/packages/WindowsAzure.ServiceBus) veya [Event Hubs - olay işleyicisi ana bilgisayarı](..//event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md), doğru izinler ile IOT Hub bağlantı dizelerini kullanabilirsiniz. Ardından **iletiler/olaylar** olay hub'ı adı.

SDK'ları (veya ürün tümleştirmeleri) kullandığınızda, IOT hub'farkında olan, bir Event Hub ile uyumlu uç nokta ve Event Hub ile uyumlu adı almanız gerekir:

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin.

2. Tıklayın **yerleşik uç noktaları**.

3. **Olayları** bölümünde aşağıdaki değerleri içerir: **Event Hub ile uyumlu uç nokta**, **Event Hub ile uyumlu adı**, **bölümleri**, **Elde tutma süresi**, ve **tüketici grupları**.

    ![Cihazdan buluta ayarları](./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png)

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

* IOT Hub uç noktaları hakkında daha fazla bilgi için bkz: [IOT Hub uç noktaları](iot-hub-devguide-endpoints.md).

* [Hızlı Başlangıçlar](quickstart-send-telemetry-node.md) sanal cihazlardan CİHAZDAN buluta iletiler gönderir ve iletilerini yerleşik uç noktadan okuma işlemini gösterir. 

Daha fazla ayrıntı için [yolları kullanarak işlem IOT Hub CİHAZDAN buluta iletileri](tutorial-routing.md) öğretici.

* Özel uç noktalar için CİHAZDAN buluta iletileri yönlendirmek istiyorsanız, bkz. [CİHAZDAN buluta iletiler için ileti yollarını ve özel uç noktaları kullanma](iot-hub-devguide-messages-read-custom.md).