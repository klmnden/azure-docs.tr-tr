---
title: Azure Event Grid Azure haritalar olay şeması
description: Azure haritalar olayları Azure Event Grid ile sağlanan şema ve özelliklerini açıklar.
services: event-grid
author: walsehgal
ms.service: event-grid
ms.topic: reference
ms.date: 02/08/2019
ms.author: v-musehg
ms.openlocfilehash: 74a3674e632f8dc3f0755bc2ad48376708c7966f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60861863"
---
# <a name="azure-event-grid-event-schema-for-azure-maps"></a>Azure haritalar için Azure Event Grid olay şeması

Bu makalede Azure haritalar olayları için şema ve özellikleri sağlar. Olay şemaları için bir giriş için bkz [Azure Event Grid olay şeması](https://docs.microsoft.com/azure/event-grid/event-schema).

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Azure haritalar hesabı aşağıdaki olay türlerini gösterir:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Maps.GeofenceEntered | İçin belirtilen olarak bölge sınırı içinde dışında öğesinden alınan koordinatları taşınmış oluşturulur |
| Microsoft.Maps.GeofenceExited | İçin belirtilen olarak bölge sınırının dışında içinde öğesinden alınan koordinatları taşınmış oluşturulur |
| Microsoft.Maps.GeofenceResult | Bölge sınırlaması sorgu durumundan bağımsız olarak bir sonuç döndürür. her zaman gerçekleşti |

## <a name="event-examples"></a>Olay örnekleri

Şeması aşağıdaki örnekte bir **GeofenceEntered** olay

```JSON
{   
   "id":"7f8446e2-1ac7-4234-8425-303726ea3981", 
   "topic":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Maps/accounts/{accountName}", 
   "subject":"/spatial/geofence/udid/{udid}/id/{eventId}", 
   "data":{   
      "geometries":[   
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"2", 
            "distance":-999.0, 
            "nearestLat":47.618786, 
            "nearestLon":-122.132151 
         } 
      ], 
      "expiredGeofenceGeometryId":[   
      ], 
      "invalidPeriodGeofenceGeometryId":[   
      ] 
   }, 
   "eventType":"Microsoft.Maps.GeofenceEntered", 
   "eventTime":"2018-11-08T00:54:17.6408601Z", 
   "metadataVersion":"1", 
   "dataVersion":"1.0" 
}
```

Aşağıdaki örnek göstermek için şema **GeofenceResult** 

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

## <a name="event-properties"></a>Olay Özellikleri

Bir olay aşağıdaki üst düzey veri vardır:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| topic | string | Olay kaynağı tam kaynak yolu. Bu alan, yazılabilir değil. Event Grid, bu değeri sağlar. |
| subject | string | Yayımcı tarafından tanımlanan olay konu yolu. |
| eventType | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| id | string | Olayın benzersiz tanımlayıcısı. |
| data | object | Bölge sınırlaması olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| apiCategory | string | Olay kategorisi API. |
| apiName | string | Olayın API adı. |
| issues | object | İşleme sırasında karşılaşılan sorunları listeler. Herhangi bir sorun yoksa, ardından olacaktır hiçbir geometriler ile bir yanıt döndürdü. |
| responseCode | number | HTTP yanıt kodu |
| geometries | object | Liste koordinatını içeren sınır geometriler getirin veya konumu etrafında searchBuffer çakışıyor. |

Haritalar API'SİNDE hata oluştuğunda hata nesnesi döndürülür. Hata nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| error | Hata ayrıntıları |Haritalar API'SİNDE hata oluştuğunda, bu nesne döndürülür  |

Haritalar API'SİNDE hata oluştuğunda ErrorDetails nesne döndürülür. Hata ayrıntıları veya nesneyi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| code | string | HTTP durum kodu. |
| message | string | Varsa, hatanın insan tarafından okunabilir bir açıklaması. |
| innererror | InnerError | Varsa, hizmete özgü hata hakkında bilgi içeren bir nesne. |

InnerError hatayla ilgili hizmete özel bilgi içeren bir nesnedir. InnerError nesnesi, aşağıdaki özelliklere sahiptir: 

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| code | string | Hata iletisi. |

Geometri nesnesi geometri göre istekteki kullanıcı süresi dolmuş bölge sınırlarının kimliklerini listeler. Geometri nesnesi, geometri öğe aşağıdaki özelliklerle sahiptir: 

| Özellik | Tür | Açıklama |
|:-------- |:---- |:----------- |
| deviceid | string | Cihaz kimliği. |
| distance | string | <p>Koordinat uzaklığı en yakın kenarlığı döndürürüz. Pozitif koordinat bölge sınırının dışında anlamına gelir. Koordinat döndürürüz, ancak birden çok değeri en yakın bölge sınırı kenarlığa uzağa searchBuffer dışında ise, değer-999'dur. Negatif koordinat bölge sınırının içinde olduğu anlamına gelir. Ardından koordinat Çokgen, ancak birden çok değeri en yakın bölge sınırlaması kenarlık uzağa searchBuffer içinde ise, değer -999 ' dir. Değeri harika güvenle olduğunu 999 anlamına gelir koordinat iyi bölge sınırının dışında ' dir. Bir harika güvenle yok-999 anlamına gelir. koordinat iyi bölge sınırı içinde değerdir.<p> |
| geometryid |string | Benzersiz kimliği döndürürüz geometri tanımlar. |
| nearestlat | number | Geometri, yakın noktasının enlem. |
| nearestlon | number | Geometri, yakın noktası bulunduğu boylam. |
| udId | string | Bir bölge sınırının karşıya yüklenirken kullanıcı karşıya yükleme hizmetten döndürülen benzersiz kimliği. Bölge sınırlaması post API'SİNDE dahil edilmez. |

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| expiredGeofenceGeometryId | string[] | Geometri Kimliğine göre istekteki kullanıcı süresi dolmuş döndürürüz listeler. |
| geometries | geometriler] |Liste koordinatını içeren sınır geometriler getirin veya konumu etrafında searchBuffer çakışıyor. |
| invalidPeriodGeofenceGeometryId | string[]  | Geometri kimliği istekteki kullanıcı zamanı göre geçersiz dönem içinde döndürürüz listeler. |
| isEventPublished | boole | En az bir olay Azure haritalar olay abonesi olarak, yanlış hiçbir olay varsa yayımlanıyorsa true Azure haritalar olay abonesi tarafından yayımlanır. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event grid'e giriş için bkz [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).