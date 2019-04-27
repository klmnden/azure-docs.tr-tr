---
title: Event Grid kullanarak Azure haritalar olaylarına tepki verme | Microsoft Docs
description: Event Grid kullanarak Azure haritalar olaylarına tepki verme konusunda bilgi edinin.
author: walsehgal
ms.author: v-musehg
ms.date: 02/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: a70011b934398ac4e7f74bb67013e93bb5e86e4e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60799164"
---
# <a name="react-to-azure-maps-events-by-using-event-grid"></a>Event Grid kullanarak Azure haritalar olaylarına tepki verme 

Azure haritalar, diğer hizmetler ve aşağı akış tetikleyici işlemleri olay bildirimleri göndermenizi sağlayarak, Azure Event Grid ile tümleşir. Bu makalenin amacı, iş uygulamalarınızı önemli olaylara güvenilir, ölçeklenebilir ve güvenli bir şekilde tepki verebilir böylece Azure haritalar olaylarını dinleyecek şekilde yapılandırmanıza yardımcı olmaktır. Örneğin, bir veritabanını güncelleştirmek, bilet oluşturma ve bir cihaz bir bölge sınırının girer her zaman bir e-posta bildirimi göndermeye gibi birden çok eylem gerçekleştiren bir uygulama oluşturun.

Azure Event Grid; Yayımla kullanan bir tam olarak yönetilen olay yönlendirme hizmetini-abonelik modeli. Olay kılavuzu gibi Azure Hizmetleri için yerleşik desteği vardır [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) ve [Azure Logic Apps](https://docs.microsoft.com/azure/azure-functions/functions-overview)ve olay uyarıları için Web kancalarını kullanan Azure dışı hizmetleri sunabilirsiniz. Event Grid tarafından olay işleyicileri tam bir listesi için bkz. [Azure Event grid'e giriş](https://docs.microsoft.com/azure/event-grid/overview).


![Azure Event Grid işlevsel modeli](./media/azure-maps-event-grid-integration/azure-event-grid-functional-model.png)


## <a name="azure-maps-events-types"></a>Azure haritalar olay türleri

Event grid'i kullanır [olay abonelikleri](https://docs.microsoft.com/azure/event-grid/concepts#event-subscriptions) abonelere olay iletileri yönlendirmek için. Azure haritalar hesabı aşağıdaki olay türlerini gösterir: 

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Maps.GeofenceEntered | İçin belirtilen olarak bölge sınırı içinde dışında öğesinden alınan koordinatları taşınmış oluşturulur |
| Microsoft.Maps.GeofenceExited | İçin belirtilen olarak bölge sınırının dışında içinde öğesinden alınan koordinatları taşınmış oluşturulur |
| Microsoft.Maps.GeofenceResult | Bölge sınırlaması sorgu durumundan bağımsız olarak bir sonuç döndürür. her zaman gerçekleşti |

## <a name="event-schema"></a>Olay şeması

Aşağıdaki örnek show şeması GeofenceResult

```JSON
{   
   "id":"451675de-a67d-4929-876c-5c2bf0b2c000", 
   "topic":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Maps/accounts/{accountName}", 
   "subject":"/spatial/geofence/udid/{udid}/id/{eventId}", 
   "data":{   
      "geometries":[   
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"1", 
            "distance":999.0, 
            "nearestLat":47.609833, 
            "nearestLon":-122.148274 
         }, 
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"2", 
            "distance":999.0, 
            "nearestLat":47.621954, 
            "nearestLon":-122.131841 
         } 
      ], 
      "expiredGeofenceGeometryId":[   
      ], 
      "invalidPeriodGeofenceGeometryId":[   
      ] 
   }, 
   "eventType":"Microsoft.Maps.GeofenceResult", 
   "eventTime":"2018-11-08T00:52:08.0954283Z", 
   "metadataVersion":"1", 
   "dataVersion":"1.0" 
}
```

## <a name="tips-for-consuming-events"></a>Olayları için ipuçları

Azure haritalar döndürürüz olayları işleyen uygulamaları birkaç önerilen uygulamaları izlemelisiniz:

* Birden çok abonelik aynı olay işleyicisi için rota olayları için yapılandırılabilir. Olayları belirli bir kaynaktan olduğunu varsayar değil önemlidir. Her zaman beklediğiniz bir kaynaktan geldiğinden emin olmak için mesajı konusuna bakın.
* İletiler sıralamaya veya farklı bir gecikmeden sonra geldiğinde. Kullanım `X-Correlation-id` alanındaki nesneleri ilgili bilgilerinizi güncel olup olmadığını anlamak için yanıt üst bilgisi.
* Ne zaman Get ve POST Döndürürüz API'sini ayarlamak mod parametresi ile çağrıldığında `EnterAndExit`, kendisi için durumu'dan önceki Döndürürüz API çağrısını değişti bölge sınırının içinde her geometrisi için Enter veya çıkış bir olay oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

Bölge sınırlaması denetimi işlemleri için bir yapı sitede kullanma hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"] 
> [Azure haritalar'ı kullanarak bir bölge sınırının ayarlayın](tutorial-geofence.md)