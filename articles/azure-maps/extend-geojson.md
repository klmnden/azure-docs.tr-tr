---
title: GeoJSON geometri Azure Maps genişletme | Microsoft Docs
description: GeoJSON geometri Azure Maps genişletmeyi öğrenin
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 2cc0e29615ad4fc19040055d847435a9dffa9c95
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655382"
---
# <a name="extending-geojson-geometries"></a>GeoJSON geometri genişletme

Azure eşlemeleri iç/boyunca coğrafi özellikleri aramak için güçlü API listesini sağlar.
Bu API'leri üzerinde standartlaştırmak [GeoJSON spec] [ 1] coğrafi özellikleri temsil etmek için (örneğin: durum sınırları, yolları).  

[GeoJSON spec] [ 1] yalnızca aşağıdaki geometri destekler:

* GeometryCollection
* LineString
* MultiLineString
* MultiPoint
* MultiPolygon
* noktası
* Çokgen

Bazı Azure haritaları API'si (örneğin: [içinde arama geometri](https://docs.microsoft.com/en-us/rest/api/maps/search/postsearchinsidegeometry)) olmayan geometri "Daire" gibi kabul parçası [GeoJSON spec][1].

Bu makalede Azure eşlemeleri nasıl genişlettiğini ayrıntılı bir açıklama sağlar [GeoJSON spec] [ 1] belirli geometri temsil etmek için.

### <a name="circle"></a>Daire

`Circle` Tarafından geometri desteklenmiyor [GeoJSON spec][1]. Kullanırız `GeoJSON Feature` bir daire temsil eden nesne.

A `Circle` geometri temsil kullanarak `GeoJSON Feature` nesne __gerekir__ aşağıdakileri içerir:

1. Orta
   >Dairenin merkezi kullanarak temsil edilen bir `GeoJSON Point` türü.

2. Yarıçap
   >Dairenin `radius` kullanılarak temsil `GeoJSON Feature`ait özellikler. Yarıçap değeri olarak _metre_ ve türünde olmalıdır `double`.

3. Alt türü
   >Daire geometri da içermelidir `subType` özelliği. Bu özellik bir parçası olmalıdır `GeoJSON Feature`'s özellikleri ve buna ait değer olmalıdır _daire_


#### <a name="example"></a>Örnek

İşte nasıl adresindeki ortalanmış bir daire temsil (enlem: 47.639754, boylam:-122.126986) 100 kullanarak ölçümler için eşit bir RADIUS ile bir `GeoJSON Feature` nesnesi:

```json            
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}          
```

[1]: https://tools.ietf.org/html/rfc7946
