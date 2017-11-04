---
title: "Azure IOT hub'ı yerleşik uç anlama | Microsoft Docs"
description: "Geliştirici Kılavuzu - yerleşik, Event Hub ile uyumlu uç noktası o cihaz bulut iletilerini kullanmayı açıklar."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: dobett
ms.openlocfilehash: c9e6aa03e3a1e0592223630c7b81634bcb09add6
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Yerleşik uç noktasından cihaz bulut iletilerini okuyun

Varsayılan olarak, yerleşik service'e yönelik uç noktasına iletileri yönlendirilir (**iletileri/olayları**) ile uyumlu olan [Event Hubs][lnk-event-hubs]. Bu bitiş noktası şu anda yalnızca gösterilen kullanmaktır [AMQP] [ lnk-amqp] bağlantı noktası 5671 protokolü. IOT hub'ı yerleşik Event Hub ile uyumlu Mesajlaşma uç nokta denetlemenize olanak sağlamak için aşağıdaki özellikleri sunar **iletileri/olayları**.

| Özellik            | Açıklama |
| ------------------- | ----------- |
| **Bölüm sayısı** | Oluşturma sırasında sayısını tanımlamak için bu özelliği ayarlamak [bölümleri] [ lnk-event-hub-partitions] cihaz-bulut olay alımı için. |
| **Saklama süresi**  | Bu özellik ne kadar süreyle iletileri IOT Hub tarafından korunur gün cinsinden belirtir. Varsayılan bir gündür, ancak yedi gün sayısı artırılabilir. |

IOT Hub ayrıca yönetmenize imkan sağlar tüketici grupları yerleşik cihaz-bulut uç noktası alırsınız.

Varsayılan olarak, açıkça bir ileti yönlendirme kuralı ile eşleşmiyor tüm iletileri yerleşik uç noktasına yazılır. Bu geri dönüş yolu devre dışı bırakırsanız, tüm ileti yönlendirme kuralları açıkça eşleşmiyor iletileri bırakılır.

Bekletme süresini ya da değiştirebilirsiniz üzerinden programlı olarak [IOT hub'ı kaynak sağlayıcısı REST API'leri][lnk-resource-provider-apis], veya kullanarak [Azure portal][lnk-management-portal].

IOT hub'ı sunan **iletileri/olayları** hub tarafından alınan cihaz bulut iletilerini okumanızı arka uç hizmetleriniz için yerleşik bir uç nokta. Bu uç noktaya olaydır iletileri okumak için Event Hubs hizmeti mekanizmaları birini kullanmanızı sağlayan Hub ile uyumlu destekler.

## <a name="read-from-the-built-in-endpoint"></a>Yerleşik uç noktasından okumak

Kullandığınızda [.NET için Azure hizmet veri yolu SDK] [ lnk-servicebus-sdk] veya [Event Hubs - olay işleyicisi konağı][lnk-eventprocessorhost], doğru izinlere sahip IOT Hub bağlantı dizelerini kullanabilirsiniz. Ardından **iletileri/olayları** olay hub'ı adı olarak.

SDK'ları (veya ürün tümleştirmeler) kullandığınızda, IOT hub'ını farkında, IOT hub ayarlarınızı bir Event Hub ile uyumlu uç noktası ve Event Hub ile uyumlu adı almanız gerekir:

1. Oturum [Azure portal] [ lnk-management-portal] ve IOT hub'ına gidin.
1. **Uç Noktalar**’a tıklayın.
1. İçinde **yerleşik uç noktaları** 'yi tıklatın **olayları**. 
1. Aşağıdaki değerleri içeren bir özellikleri sayfası açılır: **Event Hub ile uyumlu uç nokta**, **Event Hub ile uyumlu adı**, **bölümleri**,  **Saklama süresi**, ve **tüketici grupları**.

    ![Cihaz bulut ayarları][img-eventhubcompatible]

IOT Hub SDK'sı olan IOT Hub uç nokta adı gerektirir **iletileri/olayları** altında gösterildiği gibi **uç noktaları**.

Kullanmakta olduğunuz SDK gerektiriyorsa bir **ana bilgisayar adı** veya **Namespace** değeri, düzeninden Kaldır **Event Hub ile uyumlu uç nokta**. Örneğin, Event Hub ile uyumlu uç noktanızı ise **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, **ana bilgisayar adı** olacaktır  **iothub-ns-myiothub-1234.servicebus.windows.net**. **Namespace** olacaktır **iothub-ns-myiothub-1234**.

Daha sonra sahip herhangi bir paylaşılan erişim ilkesinin kullanabilirsiniz **ServiceConnect** belirtilen olay Hub'ına bağlanmak için gereken izinleri.

Önceki bilgileri kullanarak bir Event Hub bağlantı dizesi oluşturma gerekiyorsa, şu biçimi kullanın:

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

SDK'ları ve IOT hub'ı gösteren Event Hub ile uyumlu uç noktalar ile kullanabileceğiniz tümleştirmeler, aşağıdaki listedeki öğeleri içerir:

* [Java Event Hubs istemcisi](https://github.com/Azure/azure-event-hubs-java).
* [Apache Storm spout](../hdinsight/storm/apache-storm-develop-csharp-event-hub-topology.md). Görüntüleyebileceğiniz [kaynak spout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) github'da.
* [Apache Spark tümleştirme](../hdinsight/spark/apache-spark-eventhub-streaming.md).

## <a name="next-steps"></a>Sonraki adımlar

IOT Hub uç noktaları hakkında daha fazla bilgi için bkz: [IOT Hub uç noktaları][lnk-endpoints].

[Get Started] [ lnk-get-started] öğreticileri benzetimli aygıtlardan cihaz bulut iletilerini göndermek ve iletileri yerleşik uç noktasından okumak nasıl gösterir. Daha fazla ayrıntı için [yolları kullanma işlemi IOT Hub cihaz bulut iletilerini] [ lnk-d2c-tutorial] Öğreticisi.

Özel uç noktaları için cihaz bulut iletilerini yönlendirmek istiyorsanız, bkz: [için cihaz bulut iletilerini ileti yollarını ve özel uç noktaları kullanma][lnk-custom].

[img-eventhubcompatible]: ./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png

[lnk-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-management-portal]: https://portal.azure.com
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-event-hub-partitions]: ../event-hubs/event-hubs-features.md#partitions
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx
[lnk-amqp]: https://www.amqp.org/
