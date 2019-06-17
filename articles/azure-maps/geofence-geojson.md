---
title: Azure haritalar Döndürürüz GeoJSON veri biçiminde | Microsoft Docs
description: Azure haritalar Döndürürüz GeoJSON veri biçimi hakkında bilgi edinin
author: walsehgal
ms.author: v-musehg
ms.date: 02/14/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: d4b6c8289ae7c22521fc433c928f2b25a56c87ef
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64723580"
---
# <a name="geofencing-geojson-data"></a>Bölge sınırlaması GeoJSON veri

Azure haritalar [alma Döndürürüz](/rest/api/maps/spatial/getgeofence) ve [POST Döndürürüz](/rest/api/maps/spatial/postgeofence) API'leri yakınlığını sağlanan döndürürüz göreli koordinat veya sınırlar kümesini almanıza olanak sağlar. Bu makalede Azure haritalar alın ve POST API'sini kullanılabilir döndürürüz verileri hazırlama işlemi açıklanmaktadır.

Bölge sınırının veya bölge sınırlarını dizi verilerini tarafından temsil edilen `Feature` nesne ve `FeatureCollection` nesnesine `GeoJSON` tanımlanan biçimi [rfc7946](https://tools.ietf.org/html/rfc7946). Bu ek olarak:

* GeoJSON nesne türü olabilir bir `Feature` nesnesi veya bir `FeatureCollection` nesne.
* Geometri nesnesi tür olabilir bir `Point`, `MultiPoint`, `LineString`, `MultiLineString`, `Polygon`, `MultiPolygon`, ve `GeometryCollection`.
* Tüm özellik değerleri içermesi gereken bir `geometryId`, döndürürüz tanımlamak için kullanılır.
* İle özellik `Point`, `MultiPoint`, `LineString`, `MultiLineString` içermelidir `radius` özellikleri. `radius` değer metre cinsinden ölçülen `radius` değer aralıklarına 1 10000.
* İle özellik `polygon` ve `multipolygon` geometri türü, bir RADIUS özelliğe sahip değildir.
* `validityTime` kullanıcının sağlayan isteğe bağlı bir özellik, süre sonu zamanı ve geçerlilik döndürürüz veriler için zaman aralığı ayarlayın. Belirtilmezse, veriler hiçbir zaman süresi dolar ve her zaman geçerlidir.
* `expiredTime` Sona erme tarihi ve saati bölge sınırlaması veri. Varsa değerini `userTime` istekte olduğu daha sonra bu değeri veri süresi dolmuş verisi olarak kabul edilir ve yok sorgulanır karşılık gelen döndürürüz. Hangi veri dahil edilecek bu bölge sınırının, geometryId üzerine `expiredGeofenceGeometryId` döndürürüz yanıt içindeki dizi.
* `validityPeriod` Süre döndürürüz geçerlilik listesi verilmiştir. Varsa değerini `userTime` talep düştüğünde geçerlilik süresinin dışında karşılık gelen döndürürüz verileri geçersiz olarak kabul edilir ve yok sorgulanır. Bu bölge sınırının verilerin geometryId dahil `invalidPeriodGeofenceGeometryId` döndürürüz yanıt içindeki dizi. Aşağıdaki tabloda GeçerlilikSüresi öğenin özelliklerini gösterir.

| Ad | Type | Gerekli  | Açıklama |
| :------------ |:------------: |:---------------:| :-----|
| startTime | DateTime  | true | Başlangıç tarihi geçerlilik süresini süre. |
| endTime   | DateTime  | true |  Bitiş tarihi geçerlilik süresini süre. |
| recurrenceType | string | false |   Yinelenme Türü süresi. Değer olabilir `Daily`, `Weekly`, `Monthly`, veya `Yearly`. Varsayılan değer `Daily`.|
| businessDayOnly | Boolean | false |  Verileri yalnızca iş gün boyunca geçerli olup olmadığını gösterir. Varsayılan değer `false`.|


* Tüm koordinat değerleri [enlem, boylam] temsil edilir tanımlanan `WGS84`.
* Her bir özellik içeren `MultiPoint`, `MultiLineString`, `MultiPolygon` , veya `GeometryCollection`, özellikler, tüm öğelere uygulanır. Örneğin: Tüm noktaları `MultiPoint` aynı RADIUS birden çok bir daire döndürürüz oluşturmak için kullanır.
* Noktası daire senaryoda bir daire geometri kullanarak temsil edilebilen bir `Point` geometri nesnesi özellikleri ayrıntılı olarak [genişletme GeoJSON geometriler](https://docs.microsoft.com/azure/azure-maps/extend-geojson).      

Örnek istek gövdesi içinde bir daire döndürürüz geometri olarak temsil edilen bir bölge sınırının için aşağıdadır `GeoJSON` merkez noktası ve bir RADIUS kullanarak. 2018-10-yinelenen hafta sonu dışında her gün 22 09: 00 için 17: 00, geçerli dönemin döndürürüz verilerin başlatılır. `expiredTime` Bu bölge sınırının veri değerlendirilir süresi dolduğunda, varsa gösterir `userTime` istekte daha olduğu `2019-01-01`.  

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "geometryId" : "1",
        "subType": "Circle",
        "radius": 500,
        "validityTime": 
        {
            "expiredTime": "2019-01-01T00:00:00",
            "validityPeriod": [
                {
                    "startTime": "2018-10-22T09:00:00",
                    "endTime": "2018-10-22T17:00:00",
                    "recurrenceType": "Daily",
                    "recurrenceFrequency": 1,
                    "businessDayOnly": true
                }
            ]
        }
    }
}
```