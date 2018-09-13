---
title: Azure IOT Hub ve Event Grid | Microsoft Docs
description: IOT Hub'ında gerçekleşen Eylemler göre süreçlerini tetiklemek için Azure Event grid'i kullanın.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/14/2018
ms.author: kgremban
ms.openlocfilehash: 3c12e98137f44ac094adaae282b5d56d30061e60
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44719860"
---
# <a name="react-to-iot-hub-events-by-using-event-grid-to-trigger-actions"></a>Tetikleyici eylemlere Event Grid kullanarak IOT Hub olaylarına tepki verme

Olay bildirimleri diğer hizmetlere gönderin ve aşağı akış süreçlerini tetiklemek, azure IOT hub'ı Azure Event Grid ile tümleşir. İş uygulamalarınızı önemli olaylara güvenilir, ölçeklenebilir ve güvenli bir şekilde tepki verebilir böylece IOT Hub olaylarını dinlemek için yapılandırın. Örneğin, bir veritabanını güncelleştirmek, bilet oluşturma ve IOT hub'ınıza her yeni bir IOT cihazı kaydedildiğinde bir e-posta bildirimi göndermeye gibi birden fazla eylem gerçekleştirmek için bir uygulama oluşturun. 

[Azure Event Grid] [ lnk-eg-overview] Yayımla kullanan bir tam olarak yönetilen olay yönlendirme hizmetidir-abonelik modeli. Olay kılavuzu gibi Azure Hizmetleri için yerleşik desteği vardır [Azure işlevleri](../azure-functions/functions-overview.md) ve [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md)ve olay uyarıları için Web kancalarını kullanan Azure dışı hizmetleri sunabilirsiniz. Event Grid tarafından olay işleyicileri tam bir listesi için bkz. [Azure Event grid'e giriş][lnk-eg-overview]. 

![Azure Event Grid mimarisi](./media/iot-hub-event-grid/event-grid-functional-model.png)

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Event Grid tümleştirmesi, Event Grid desteklendiği bölgede bulunan IOT hub'ları için kullanılabilir. En son bölgelerin listesi için bkz. [Azure Event grid'e giriş][lnk-eg-overview]. 

## <a name="event-types"></a>Olay türleri

IOT Hub aşağıdaki olay türleri yayımlar: 

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | IOT hub'a bir cihaz kaydedildiğinde yayımladı. |
| Microsoft.Devices.DeviceDeleted | Bir cihaz IOT hub'ından silindiğinde yayımladı. |
| Microsoft.Devices.DeviceConnected | Bir cihaz IOT hub'a bağlandığında yayımladı. |
| Microsoft.Devices.DeviceDisconnected | Bir cihaz IOT hub'ından kesildiğinde yayımladı. |

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

Her bir özellik ayrıntılı bir açıklaması için bkz. [IOT hub'ın Azure Event Grid olay şeması](../event-grid/event-schema-iot-hub.md)

## <a name="filter-events"></a>Olayları Filtrele

IOT Hub olay abonelikleri, olayları olay türü ve cihaz adına göre filtre uygulayabilirsiniz. Event Grid iş konu filtreleri temel alarak **ile başlar** (ön ek) ve **sona erer ile** (sonek) eşleşir. Filtre kullanır, bir `AND` önek ve sonek eşleşen bir konu olayları aboneye teslim edilmesi işleci. 

IOT olayların konu biçimi kullanır:

```json
devices/{deviceId}
```
## <a name="limitations-for-device-connected-and-device-disconnected-events"></a>Bağlı cihaz ve cihaz sınırlamaları olayları bağlantısı kesildi

Bağlı cihaz ve cihaz bağlantısı etkinlikleri almak için cihazınız için D2C veya C2D bağlantısı açmalısınız. Cihazınızı MQTT protokolünü kullanıyorsanız, IOT Hub bağlantısını açmak C2D tutar. İçin AMQP çağırarak C2D bağlantıyı açabilirsiniz [alma zaman uyumsuz API](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.receiveasync?view=azure-dotnet). Telemetri gönderiyorsanız D2C bağlantı açıktır. Cihaz bağlantı titremeyi, yani cihazın kurulup kesildiğinde sık, her tek bağlantı göndermezler durum ancak anlık görüntüyle alınan dakikada bağlantı durumuna yayımlar. Kesinti bittikten hemen sonra IOT hub'ı kesinti olması durumunda, cihaz bağlantı durumu yayımlarız. Cihaz bu kesinti sırasında kesilirse, cihaz bağlantısı kesilmiş olay 10 dakika içinde yayımlanır.

## <a name="tips-for-consuming-events"></a>Olayları için ipuçları

IOT Hub olaylarını işlemek uygulamalar bu önerilen uygulamaları izlemelisiniz:

* Olayları belirli bir kaynaktan olduğunu varsayar değil önemlidir birden çok abonelik aynı olay işleyicisi, rota olayları için yapılandırılabilir. Her zaman beklediğiniz IOT hub'dan geldiğinden emin olmak için mesajı konusuna bakın. 
* Aldığınız tüm olayların beklediğiniz türleri olduğunu varsaymayın. EventType iletiyi işlemeyi önce her zaman denetleyin.
* İletiler sıralamaya veya farklı bir gecikmeden sonra geldiğinde. Etag alanı bilgilerinizi nesneler hakkında güncel olup olmadığını anlamak için kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [IOT Hub olaylarını öğreticisini deneyin](../event-grid/publish-iot-hub-events-to-logic-apps.md)
* [Cihazla bağlantılı ve bağlantısız olayları nasıl sıralayacağınızı öğrenin](../iot-hub/iot-hub-how-to-order-connection-state-events.md)
* [Event Grid hakkında daha fazla bilgi edinin][lnk-eg-overview]
* [IOT Hub olaylarını yönlendirme iletileri arasındaki farkları karşılaştırın][lnk-eg-compare]

<!-- Links -->
[lnk-eg-overview]: ../event-grid/overview.md
[lnk-eg-compare]: iot-hub-event-grid-routing-comparison.md