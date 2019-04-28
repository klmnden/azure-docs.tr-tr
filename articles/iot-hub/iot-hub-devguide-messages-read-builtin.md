---
title: Azure IOT hub'ı yerleşik uç anlama | Microsoft Docs
description: Geliştirici Kılavuzu - CİHAZDAN buluta iletileri okumak için yerleşik, Event Hub ile uyumlu uç noktası kullanma işlemini açıklar.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/26/2019
ms.openlocfilehash: 827d7d9a3d584342703a84dd2a42e5cda9b3a656
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61364068"
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Cihazdan buluta iletilerini yerleşik uç noktadan okuma

Varsayılan olarak, iletiler yerleşik hizmet'e yönelik uç noktaya yönlendirilir (**iletiler/olaylar**) ile uyumlu [Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/). Bu uç noktası şu anda yalnızca sunulan kullanmaktır [AMQP](https://www.amqp.org/) 5671 numaralı bağlantı noktasında protokolü. IOT hub'ı yerleşik Event Hub ile uyumlu Mesajlaşma uç denetim sağlamak için aşağıdaki özellikleri sunan **iletiler/olaylar**.

| Özellik            | Açıklama |
| ------------------- | ----------- |
| **Bölüm sayısı** | Oluşturma sırasında sayısını tanımlamak için bu özelliği ayarlayın [bölümleri](../event-hubs/event-hubs-features.md#partitions) CİHAZDAN buluta olay alımı için. |
| **Elde tutma süresi**  | Bu özellik ne kadar süreyle iletileri IOT Hub tarafından korunur gün cinsinden belirtir. Varsayılan bir gündür, ancak yedi gün olarak artırılabilir. |

IOT hub'ı en fazla 7 gün için yerleşik olay hub'larındaki veri saklama sağlar. IOT hub'ınızın oluşturulması sırasında bekletme süresini ayarlayabilirsiniz. Veri saklama zamanı IOT hub, IOT hub katmanını ve birim türü bağlıdır. Boyutu bakımından en büyük ileti boyutu kotası için en az 24 saat kadar iletilerini yerleşik Event Hubs tutabilirsiniz. Örneğin, IOT hub'ı en az korumak için yeterli depolama alanı sağlar 1 S1 birimi için 400 binden iletileri 4 k her boyutu. Cihazlarınızı küçük ileti göndermesi için (ne kadar depolama alanı tüketilen bağlı olarak uzun varan 7 gün) korunmayabilir. Belirtilen bekletme süresi en düşük için veri koruma garanti ediyoruz.

IOT Hub ayrıca yönetmenize imkan sağlar tüketici grupları yerleşik cihaz-bulut uç noktası alırsınız.

Kullanıyorsanız [ileti yönlendirme](iot-hub-devguide-messages-d2c.md) ve [geri dönüş rota](iot-hub-devguide-messages-d2c.md#fallback-route) olan etkin bir sorgu tüm yönlendiricilerin eşleşmeyen tüm iletileri yerleşik uç noktasına gidin. Bu geri dönüş rota devre dışı bırakırsanız, herhangi bir sorgu eşleşmeyen iletiler bırakılır.

Elde tutma süresi aşağıdakilerden birini kullanarak program aracılığıyla değiştirebilirsiniz [IOT hub'ı kaynak sağlayıcısı REST API'leri](/rest/api/iothub/iothubresource), veya [Azure portalında](https://portal.azure.com).

IOT hub'ı sunan **iletiler/olaylar** , hub tarafından alınan CİHAZDAN buluta iletileri okumak, arka uç Hizmetleri için yerleşik uç nokta. Bu uç nokta olaydır mekanizmalarının kullanmak için Event Hubs hizmeti sağlayan Hub ile uyumlu iletileri okumak için destekler.

## <a name="read-from-the-built-in-endpoint"></a>Yerleşik uç noktadan okuma

Bazı ürün tümleştirmeleri ve olay hub'ları SDK'ları, IOT hub'farkındayız ve yerleşik uç noktaya bağlanmak için IOT hub hizmet bağlantı dizenizi kullanmanıza izin verir.

Event Hubs SDK'ları veya IOT hub'ını simgelerinde ürün tümleştirmeleri kullanırken, bir Event Hub ile uyumlu uç nokta ve Event Hub ile uyumlu adı gerekir. Bu değerleri gibi portaldan alabilirsiniz:

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin.

2. Tıklayın **yerleşik uç noktaları**.

3. **Olayları** bölümünde aşağıdaki değerleri içerir: **Bölümler**, **Event Hub ile uyumlu adı**, **Event Hub ile uyumlu uç nokta**, **elde tutma süresi**, ve **tüketicigrupları**.

    ![Cihazdan buluta ayarları](./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png)

Portalda, Event Hub ile uyumlu uç nokta alanına benzeyen tam bir Event Hubs bağlantı dizesi içerir: **Endpoint=sb://abcd1234namespace.servicebus.windows.net/;SharedAccessKeyName=iothubowner;SharedAccessKey=keykeykeykeykeykey=;EntityPath=iothub-ehub-abcd-1234-123456**. Ardından, kullanmakta olduğunuz SDK diğer değerleri gerektiriyorsa, bunlar olur:

| Ad | Değer |
| ---- | ----- |
| Uç Nokta | SB://abcd1234namespace.servicebus.Windows.NET/ |
| Ana Bilgisayar Adı | abcd1234namespace.servicebus.Windows.NET |
| Ad alanı | abcd1234namespace |

Daha sonra olan herhangi bir paylaşılan erişim ilkesinin kullanabilirsiniz **ServiceConnect** belirtilen olay Hub'ına bağlanmak için izinleri.

IOT hub'ı sunan yerleşik Event Hub ile uyumlu uç noktasını bağlamak için kullanabileceğiniz SDK'lar şunlardır:

| Dil | SDK | Örnek | Notlar |
| -------- | --- | ------ | ----- |
| .NET | https://github.com/Azure/azure-event-hubs-dotnet | [Hızlı Başlangıç](quickstart-send-telemetry-dotnet.md) | Event Hubs ile uyumlu bilgileri kullanır |
 Java | https://github.com/Azure/azure-event-hubs-java | [Hızlı Başlangıç](quickstart-send-telemetry-java.md) | Event Hubs ile uyumlu bilgileri kullanır |
| Node.js | https://github.com/Azure/azure-event-hubs-node | [Hızlı Başlangıç](quickstart-send-telemetry-node.md) | IOT Hub bağlantı dizesini kullanır |
| Python | https://github.com/Azure/azure-event-hubs-python | https://github.com/Azure/azure-event-hubs-python/blob/master/examples/iothub_recv.py | IOT Hub bağlantı dizesini kullanır |

IOT hub'ı sunan yerleşik Event Hub ile uyumlu uç nokta ile kullanabileceğiniz Ürün tümleştirmeleri şunlardır:

* [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/). Bkz: [Azure işlevleri ile IOT Hub'ından veri işleme](https://azure.microsoft.com/resources/samples/functions-js-iot-hub-processing/).
* [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/). Bkz: [Stream Analytics'e giriş olarak veri Stream](../stream-analytics/stream-analytics-define-inputs.md#stream-data-from-iot-hub).
* [Zaman serisi öngörüleri](https://docs.microsoft.com/azure/time-series-insights/). Bkz: [zaman serisi görüşleri ortamınıza bir IOT hub olay kaynağı ekleme](../time-series-insights/time-series-insights-how-to-add-an-event-source-iothub.md).
* [Apache Storm spout](../hdinsight/storm/apache-storm-develop-csharp-event-hub-topology.md). Görüntüleyebileceğiniz [kaynak spout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) GitHub üzerinde.
* [Apache Spark tümleştirme](../hdinsight/spark/apache-spark-eventhub-streaming.md).
* [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/).

## <a name="next-steps"></a>Sonraki adımlar

* IOT Hub uç noktaları hakkında daha fazla bilgi için bkz: [IOT Hub uç noktaları](iot-hub-devguide-endpoints.md).

* [Hızlı Başlangıçlar](quickstart-send-telemetry-node.md) sanal cihazlardan CİHAZDAN buluta iletiler gönderir ve iletilerini yerleşik uç noktadan okuma işlemini gösterir. 

Daha fazla ayrıntı için [yolları kullanarak işlem IOT Hub CİHAZDAN buluta iletileri](tutorial-routing.md) öğretici.

* Özel uç noktalar için CİHAZDAN buluta iletileri yönlendirmek istiyorsanız, bkz. [CİHAZDAN buluta iletiler için ileti yollarını ve özel uç noktaları kullanma](iot-hub-devguide-messages-read-custom.md).
