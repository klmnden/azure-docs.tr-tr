---
title: Azure IOT Hub ve Event Grid | Microsoft Docs
description: IOT Hub'ında gerçekleşen Eylemler göre süreçlerini tetiklemek için Azure Event grid'i kullanın.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: kgremban
ms.openlocfilehash: f08845dbf4168627d0198e81d2092a1fe56c6c89
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733863"
---
# <a name="react-to-iot-hub-events-by-using-event-grid-to-trigger-actions"></a>Tetikleyici eylemlere Event Grid kullanarak IOT Hub olaylarına tepki verme

Olay bildirimleri diğer hizmetlere gönderin ve aşağı akış süreçlerini tetiklemek, azure IOT hub'ı Azure Event Grid ile tümleşir. İş uygulamalarınızı önemli olaylara güvenilir, ölçeklenebilir ve güvenli bir şekilde tepki verebilir böylece IOT Hub olaylarını dinlemek için yapılandırın. Örneğin, bir veritabanını güncelleştirir, bir iş anahtarı oluşturur ve yeni bir IOT cihazı kayıtlı her zaman bir e-posta bildirimi IOT hub'ınıza gönderir. bir uygulama oluşturun.

[Azure Event Grid](../event-grid/overview.md) Yayımla kullanan bir tam olarak yönetilen olay yönlendirme hizmetidir-abonelik modeli. Olay kılavuzu gibi Azure Hizmetleri için yerleşik desteği vardır [Azure işlevleri](../azure-functions/functions-overview.md) ve [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md)ve olay uyarıları için Web kancalarını kullanan Azure dışı hizmetleri sunabilirsiniz. Event Grid tarafından olay işleyicileri tam bir listesi için bkz. [Azure Event grid'e giriş](../event-grid/overview.md).

![Azure Event Grid mimarisi](./media/iot-hub-event-grid/event-grid-functional-model.png)

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Event Grid tümleştirmesi, Event Grid desteklendiği bölgede bulunan IOT hub'ları için kullanılabilir. Cihazın telemetri olayları hariç tüm cihaz olaylarını genel kullanıma sunulmuştur. Cihazın telemetri olayı genel Önizleme aşamasındadır ve Doğu ABD, Batı ABD, Batı Avrupa dışındaki tüm bölgelerde kullanılabilir [Azure kamu](/azure/azure-government/documentation-government-welcome), [Azure Çin 21Vianet](/azure/china/china-welcome), ve [Azure Almanya](https://azure.microsoft.com/global-infrastructure/germany/). En son bölgelerin listesi için bkz. [Azure Event grid'e giriş](../event-grid/overview.md).

## <a name="event-types"></a>Olay türleri

IOT Hub aşağıdaki olay türleri yayımlar:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | IOT hub'a bir cihaz kaydedildiğinde yayımladı. |
| Microsoft.Devices.DeviceDeleted | Bir cihaz IOT hub'ından silindiğinde yayımladı. |
| Microsoft.Devices.DeviceConnected | Bir cihaz IOT hub'a bağlandığında yayımladı. |
| Microsoft.Devices.DeviceDisconnected | Bir cihaz IOT hub'ından kesildiğinde yayımladı. |
| Microsoft.Devices.DeviceTelemetry | IOT hub'a bir cihaz telemetrisi ileti gönderildiğinde yayımlandı |

Her IOT hub'ından yayımlamak için hangi olayları yapılandırmak için Azure portal veya Azure CLI kullanın. Örneğin, öğreticiyi deneyin [Logic Apps kullanarak Azure IOT Hub olayları hakkında e-posta bildirimleri gönderme](../event-grid/publish-iot-hub-events-to-logic-apps.md).

## <a name="event-schema"></a>Olay şeması

IOT Hub olaylarını cihaz yaşam döngünüzü değişikliklere yanıt vermek için gereken tüm bilgileri içerir. EventType özelliği ile başlayan denetleyerek bir IOT Hub ve event tanımlayabilirsiniz **Microsoft.Devices**. Event Grid olay özelliklerini kullanma hakkında daha fazla bilgi için bkz. [Event Grid olay şeması](../event-grid/event-schema.md).

### <a name="device-connected-schema"></a>Cihaz bağlı şeması

Aşağıdaki örnek, bir cihaz bağlı olay şeması gösterir:

```json
[{  
  "id": "f6bbf8f4-d365-520d-a878-17bf7238abd8",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceConnected",
  "eventTime": "2018-06-02T19:17:44.4383997Z",
  "data": {
      "deviceConnectionStateEventInfo": {
        "sequenceNumber":
          "000000000000000001D4132452F67CE200000002000000000000000000000001"
      },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice",
    "moduleId" : "DeviceModuleID",
  }, 
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="device-telemetry-schema"></a>Cihazın Telemetri şeması

Cihazın telemetri iletileriyle contentType JSON olarak ayarlayın ve iletide UTF-8 olarak ayarla contentEncoding geçerli bir JSON biçiminde olmalıdır [Sistem Özellikleri](iot-hub-devguide-routing-query-syntax.md#system-properties). Bu ayarlanmazsa, IOT Hub iletilerini taban 64 kodlanmış biçimde yazılacaktır.

Event Grid için uç nokta Event Grid seçerek yayımlanmalarından önce cihazın telemetri olayları zenginleştirebilirsiniz. Daha fazla bilgi için [ileti Zenginleştirmelerinin genel bakış](iot-hub-message-enrichments-overview.md).

Aşağıdaki örnek, bir cihazın telemetri olayı şemasını gösterir:

```json
[{  
  "id": "9af86784-8d40-fe2g-8b2a-bab65e106785",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceTelemetry",
  "eventTime": "2019-01-07T20:58:30.48Z",
  "data": {
      "body": {
          "Weather": {
              "Temperature": 900
            },
            "Location": "USA"
        },
        "properties": {
            "Status": "Active"
        },
        "systemProperties": {
          "iothub-content-type": "application/json",
          "iothub-content-encoding": "utf-8",
          "iothub-connection-device-id": "d1",
          "iothub-connection-auth-method": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
          "iothub-connection-auth-generation-id": "123455432199234570",
          "iothub-enqueuedtime": "2019-01-07T20:58:30.48Z",
          "iothub-message-source": "Telemetry"
        }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="device-created-schema"></a>Cihaz oluşturuldu şeması

Aşağıdaki örnek, olay oluşturan bir cihaz şemasını gösterir:

```json
[{
  "id": "56afc886-767b-d359-d59e-0da7877166b2",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceCreated",
  "eventTime": "2018-01-02T19:17:44.4383997Z",
  "data": {
    "twin": {
      "deviceId": "LogicAppTestDevice",
      "etag": "AAAAAAAAAAE=",
      "deviceEtag":"null",
      "status": "enabled",
      "statusUpdateTime": "0001-01-01T00:00:00",
      "connectionState": "Disconnected",
      "lastActivityTime": "0001-01-01T00:00:00",
      "cloudToDeviceMessageCount": 0,
      "authenticationType": "sas",
      "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
      },
      "version": 2,
      "properties": {
        "desired": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        },
        "reported": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        }
      }
    },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

Her bir özellik ayrıntılı bir açıklaması için bkz. [IOT hub'ın Azure Event Grid olay şeması](../event-grid/event-schema-iot-hub.md).

## <a name="filter-events"></a>Olayları filtreleme

IOT Hub olay abonelikleri, olayları olay türü, veri içerikleri ve cihaz adı ve konu göre filtreleyebilirsiniz.

Event Grid sağlayan [filtreleme](../event-grid/event-filtering.md) olay türlerini, konuları ve veri içerik üzerinde. Event Grid aboneliği oluşturulurken, seçilen IOT olaylara abone seçebilirsiniz. Event Grid iş konu filtreleri temel alarak **ile başlar** (ön ek) ve **sona erer ile** (sonek) eşleşir. Filtre kullanır, bir `AND` önek ve sonek eşleşen bir konu olayları aboneye teslim edilmesi işleci.

IOT olayların konu biçimi kullanır:

```json
devices/{deviceId}
```

Event Grid, veri içerikleri dahil olmak üzere her bir olay özniteliklerinde filtreleme için de sağlar. Bu, hangi olayların telemetri ileti tabanlı içeriği teslim seçmenize olanak sağlar. Lütfen [Gelişmiş filtreleme](../event-grid/event-filtering.md#advanced-filtering) örneklerini görüntülemek için.

DeviceConnected, DeviceDisconnected DeviceCreated ve DeviceDeleted gibi telemetri dışı olaylar için filtreleme Event Grid aboneliği oluşturulurken kullanılabilir. Telemetri olaylar'Event grid'de filtreleme ek olarak, kullanıcılar ayrıca cihaz çiftleri, ileti özelliklerini ve ileti yönlendirme sorguyla gövdesi filtreleyebilirsiniz. Varsayılan oluştururuz [rota](iot-hub-devguide-messages-d2c.md) IOT Hub'ında cihaz telemetrisi, Event Grid aboneliği temel. Bu tek yolu tüm Event Grid aboneliklerinizi başa çıkabilir. Telemetri verilerini göndermeden önce iletileri filtre uygulamak için güncelleştirebilirsiniz, [yönlendirme sorgu](iot-hub-devguide-routing-query-syntax.md). İleti gövdesi için yönlendirme sorgu gövdesi JSON ise uygulanabilir unutmayın.

## <a name="limitations-for-device-connected-and-device-disconnected-events"></a>Bağlı cihaz ve cihaz sınırlamaları olayları bağlantısı kesildi

Bağlı cihaz ve cihaz bağlantısı etkinlikleri almak için cihazınız için D2C veya C2D bağlantısı açmalısınız. Cihazınızı MQTT protokolünü kullanıyorsanız, IOT Hub bağlantısını açmak C2D tutar. AMQP için çağırarak C2D bağlantıyı açabilirsiniz [alma zaman uyumsuz API](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.receiveasync?view=azure-dotnet).

Telemetri gönderiyorsanız D2C bağlantı açıktır. Cihaz bağlantı titremeyi, cihazı bağlandığında ve sık sık anlamına gelir ki her tek bağlantı durumu göndermez, ancak hangi dakikada bir anlık görüntünün alınma bağlantı durumu yayımlayacak. Kesinti bittikten hemen sonra IOT hub'ı kesinti olması durumunda, cihaz bağlantı durumu yayımlarız. Cihaz bu kesinti sırasında kesilirse, cihaz bağlantısı kesilmiş olay 10 dakika içinde yayımlanır.

## <a name="tips-for-consuming-events"></a>Olayları için ipuçları

IOT Hub olaylarını işlemek uygulamalar bu önerilen uygulamaları izlemelisiniz:

* Birden çok abonelik olayların belirli bir kaynaktan olduğunu varsaymayın bu nedenle aynı olay işleyicisi, rota olayları için yapılandırılabilir. Her zaman beklediğiniz IOT hub'dan geldiğinden emin olmak için mesajı konusuna bakın.

* Aldığınız tüm olayların beklediğiniz türleri olduğunu varsaymayın. EventType iletiyi işlemeyi önce her zaman denetleyin.

* İletiler sıralamaya veya farklı bir gecikmeden sonra geldiğinde. Etag alanı bilgilerinizi nesnelerle ilgili cihaz oluşturulan veya silinen cihaz olayları için güncel olup olmadığını anlamak için kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [IOT Hub olaylarını öğreticisini deneyin](../event-grid/publish-iot-hub-events-to-logic-apps.md)

* [Cihazla bağlantılı ve bağlantısız olayları nasıl sıralayacağınızı öğrenin](iot-hub-how-to-order-connection-state-events.md)

* [Event Grid hakkında daha fazla bilgi edinin](../event-grid/overview.md)

* [IOT Hub olaylarını yönlendirme iletileri arasındaki farkları karşılaştırın](iot-hub-event-grid-routing-comparison.md)
