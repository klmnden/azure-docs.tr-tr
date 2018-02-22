---
title: "Azure IOT Hub ve olay kılavuz | Microsoft Docs"
description: "IOT hub'ında gerçekleşecek Eylemler temel işlemleri tetiklemek için Azure olay kılavuzunu kullanın."
services: iot-hub
documentationcenter: 
author: kgremban
manager: timlt
editor: 
ms.service: iot-hub
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2018
ms.author: kgremban
ms.openlocfilehash: 6123039ba5eeb720e0ca590fa69af915da91367c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="react-to-iot-hub-events-by-using-event-grid-to-trigger-actions---preview"></a>Tetikleyici eylemleri - Önizleme olay kılavuz kullanarak IOT hub'ı olaylarına tepki

Böylece diğer hizmetlere olay bildirimleri gönderin ve aşağı akış işlemleri tetiklemesini azure IOT hub'ı Azure olay kılavuz ile tümleşir. İş uygulamalarınız, kritik olayları için güvenilir, ölçeklenebilir ve güvenli bir şekilde tepki verme IOT hub'ı olaylarını dinleyecek şekilde yapılandırın. Örneğin, bir veritabanını güncelleştirmek, bir anahtar oluşturma ve yeni bir IOT cihaz IOT hub'ınıza kayıtlı her zaman bir e-posta bildirimi gönderiliyor gibi birden çok eylemleri gerçekleştirmek için bir uygulama oluşturun. 

[Azure olay kılavuz] [ lnk-eg-overview] yayımlamayı kullanan bir tam olarak yönetilen olay yönlendirme hizmeti-model abone olun. Olay kılavuz sahip gibi Azure Hizmetleri için yerleşik destek [Azure işlevleri](../azure-functions/functions-overview.md) ve [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md)ve olay uyarıları Web kancalarını kullanarak Azure olmayan hizmetlerine sunabilir. Olay kılavuz destekleyen olay işleyicileri tam bir listesi için bkz: [Azure olay kılavuzuna giriş][lnk-eg-overview]. 

![Azure olay kılavuz mimarisi](./media/iot-hub-event-grid/event-grid-functional-model.png)

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Tümleştirme genel önizlemede olan olay kılavuz böylece kullanılabilir bölgeleri sınırlı bir süre içinde. Aşağıdaki bölgede bulunan IOT hub'ları için tümleştirme çalışır:

* Orta ABD
* Doğu ABD
* Doğu ABD 2
* Batı Orta ABD
* Batı ABD
* Batı ABD 2

## <a name="event-types"></a>Olay türleri

IOT Hub aşağıdaki olay türlerini yayımlar: 

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | IOT hub'a bir cihaz kaydedildiğinde yayımladı. |
| Microsoft.Devices.DeviceDeleted | Bir cihaz IOT hub'ından silindiğinde yayımladı. | 

Her IOT hub'ından yayımlamak için hangi olayların yapılandırmak için Azure portalında veya Azure CLI kullanın. Örneğin, öğreticiyi deneyin [Logic Apps kullanarak Azure IOT Hub olayları hakkında e-posta bildirim gönderme](../event-grid/publish-iot-hub-events-to-logic-apps.md). 

## <a name="event-schema"></a>Olay şeması

IOT hub'ı olayları cihaz yaşam döngüsü değişiklikleri yanıtlamaları gereken tüm bilgileri içerir. EventType özelliği ile başlayan denetleyerek bir IOT hub'ı olay tanımlayabilirsiniz **Microsoft.Devices**. Olay kılavuz olay özelliklerini kullanma hakkında daha fazla bilgi için bkz: [olay kılavuz olay şema](../event-grid/event-schema.md).

### <a name="device-created-schema"></a>Oluşturulan cihaz şeması

Aşağıdaki örnek, olay oluşturulan bir aygıtı şeması gösterir: 

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
    "deviceId": "LogicAppTestDevice",
    "operationTimestamp": "2018-01-02T19:17:44.4383997Z",
    "opType": "DeviceCreated"
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

Her bir özellik ayrıntılı bir açıklaması için bkz: [IOT Hub için Azure olay kılavuz olay şeması](../event-grid/event-schema-iot-hub.md)

## <a name="filter-events"></a>Olaylar filtresi

IOT hub'ı olay abonelikleri olayları olay türü ve cihaz adına göre filtre uygulayabilirsiniz. Olay kılavuz iş konu filtrelere bağlı olarak **önek** ve **soneki** eşleşir. Filtre kullanır, bir `AND` için önek ve sonek eşleşen bir konu olaylarla aboneye teslim işleci. 

IOT olayları konu biçimi kullanır:

```json
devices/{deviceId}
```

### <a name="tips-for-consuming-events"></a>Olayları tüketimi için ipuçları

IOT hub'ı olayları işlemek uygulamalar bu önerilen uygulamaları izlemelidir:

* Olayları belirli bir kaynaktan olduğunu varsayar değil önemlidir birden çok abonelik aynı olay işleyicisi için rota olayları için yapılandırılabilir. Her zaman beklediğiniz IOT hub'ından geldiğinden emin olmak için ileti konusu denetleyin. 
* Aldığınız tüm olayları beklediğiniz türleri olduğunu varsayar yok. Her zaman eventType ileti işlemeden önce denetleyin.
* İletiler, bozuk veya farklı bir gecikmeden sonra geldiğinde. Bilgilerinizi nesneler hakkında güncel olup olmadığını anlamak için etag alanını kullanın.



## <a name="next-steps"></a>Sonraki adımlar

* [IOT hub'ı olayları öğretici deneyin](../event-grid/publish-iot-hub-events-to-logic-apps.md)
* [Olay kılavuz hakkında daha fazla bilgi edinin][lnk-eg-overview]
* [Yönlendirme IOT hub'ı olaylarını ve iletileri arasındaki farklar Karşılaştır][lnk-eg-compare]

<!-- Links -->
[lnk-eg-overview]: ../event-grid/overview.md
[lnk-eg-compare]: iot-hub-event-grid-routing-comparison.md