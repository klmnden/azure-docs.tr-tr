---
title: "MongoDB için Azure Cosmos DB özellik desteği | Microsoft Docs"
description: "Azure Cosmos DB MongoDB API MongoDB 3.4 sağlar özellik desteği hakkında bilgi edinin."
services: cosmos-db
author: alekseys
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 29b6547c-3201-44b6-9e0b-e6f56e473e24
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: alekseys
ms.openlocfilehash: 007b530cd7a14f063ae4f86d18daa9742c6655c2
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="mongodb-api-support-for-mongodb-features-and-syntax"></a>MongoDB özellikleri ve sözdizimi için MongoDB API desteği

Azure Cosmos DB Microsoft'un Genel dağıtılmış birden çok model veritabanı hizmetidir. Açık kaynak MongoDB istemci kanalıyla veritabanının MongoDB API ile iletişim kurabilir [sürücüleri](https://docs.mongodb.org/ecosystem/drivers). MongoDB API MongoDB için uygun olarak yüklemeyi mevcut istemci sürücüleri kullanılmasına izin verir [wire Protokolü](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol).

Azure Cosmos DB MongoDB API'sini kullanarak MongoDB için kullandığınız, tüm Azure Cosmos DB'ın Kurumsal özellikler ile API'leri yararları keyfini çıkarabilirsiniz: [genel dağıtım](distribute-data-globally.md), [otomatik parçalanmasını](partition-data.md), Kullanılabilirlik ve gecikme garantileri, dizin oluşturma, her alanı, rest, yedeklemeler ve daha fazlasını şifreleme otomatik.

## <a name="mongodb-query-language-support"></a>MongoDB sorgu dil desteği

Azure Cosmos DB MongoDB API MongoDB sorgu dil yapıları için kapsamlı destek sağlar. Aşağıda şu anda desteklenen işlemler, işleçler, aşama, komutları ve seçenekleri ayrıntılı listesini bulabilirsiniz.


## <a name="database-commands"></a>Veritabanı komutları

Azure Cosmos DB tüm MongoDB API hesaplarında veritabanı aşağıdakileri destekler. 

### <a name="query-and-write-operation-commands"></a>Sorgu ve yazma işlemi komutları
- sil
- bul
- findAndModify
- getLastError
- kitaplarından elde
- Ekle
- Güncelleştirme

### <a name="authentication-commands"></a>Kimlik doğrulama komutları
- oturumu kapat
- kimlik doğrulaması
- getnonce

### <a name="administration-commands"></a>Yönetim komutları
- dropDatabase
- listCollections
- bırakma
- oluşturmaya
- filemd5
- createIndexes
- listIndexes
- dropIndexes
- ConnectionStatus
- yeniden dizin oluşturma

### <a name="diagnostics-commands"></a>Tanılama komutları
- buildInfo
- collStats
- dbStats
- hostInfo
- listDatabases
- whatsmyuri

<a name="aggregation-pipeline"/>

## <a name="aggregation-pipelinea"></a>Toplama ardışık düzen</a>

Azure Cosmos DB toplama ardışık genel önizlemede destekler. Bkz: [Azure blogu](https://aka.ms/mongodb-aggregation) hakkında yönergeler için genel Önizleme için onboarding için.

### <a name="aggregation-commands"></a>Toplama komutları
- Toplama
- sayı
- Farklı

### <a name="aggregation-stages"></a>Toplama aşamaları
- $project
- $match
- $limit
- $skip
- $ geriye doğru izleme
- $group
- $sample
- $sort
- $lookup
- $out
- $count

### <a name="aggregation-expressions"></a>Toplama ifadeleri

#### <a name="boolean-expressions"></a>Boole ifadeleri
- $ve
- $veya
- $not

#### <a name="set-expressions"></a>Set deyimleri
- $setEquals
- $setIntersection
- $setUnion
- $setDifference
- $setIsSubset
- $anyElementTrue
- $allElementsTrue

#### <a name="comparison-expressions"></a>Karşılaştırma ifadeleri
- $cmp
- $eq
- $gt
- $gte
- $lt
- $lte
- $ne

#### <a name="arithmetic-expressions"></a>Aritmetik ifadeler
- $abs
- $Ekle
- $ceil
- $divide
- $exp
- $floor
- $ln
- $log
- $log10
- $mod
- $Çarp
- $pow
- $sqrt
- $çıkarma
- $trunc

#### <a name="string-expressions"></a>Dize ifadeleri
- $concat
- $indexOfBytes
- $indexOfCP
- $split
- $strLenBytes
- $strLenCP
- $strcasecmp
- $substr
- $substrBytes
- $substrCP
- $toLower
- $toUpper

#### <a name="array-expressions"></a>Dizi ifadeleri
- $arrayElemAt
- $concatArrays
- $filter
- $indexOfArray
- $isArray
- $range
- $reverseArray
- $size
- $slice
- içinde $

#### <a name="date-expressions"></a>Tarih ifadeleri
- $dayOfYear
- $dayOfMonth
- $dayOfWeek
- $year
- $month
- $week
- $hour
- $minute
- $İkinci
- $millisecond
- $isoDayOfWeek
- $isoWeek

#### <a name="conditional-expressions"></a>Koşullu deyimler
- $cond
- $ifNull

## <a name="aggregation-accumulators"></a>Toplama accumulators
- $sum
- $avg
- $ilk
- $Son
- $max
- $min
- $push
- $addToSet

## <a name="operators"></a>İşleçler

Aşağıdaki işleçleri bunların kullanımıyla ilgili örnekler desteklenir. Sorgularda kullanılan bu örnek belgeyi göz önünde bulundurun:

```json
{
  "Volcano Name": "Rainier",
  "Country": "United States",
  "Region": "US-Washington",
  "Location": {
    "type": "Point",
    "coordinates": [
      -121.758,
      46.87
    ]
  },
  "Elevation": 4392,
  "Type": "Stratovolcano",
  "Status": "Dendrochronology",
  "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
}
```

İşleç | Örnek |
--- | --- |
$eq | ``` { "Volcano Name": { $eq: "Rainier" } } ``` |  | -
$gt | ``` { "Elevation": { $gt: 4000 } } ``` |  | -
$gte | ``` { "Elevation": { $gte: 4392 } } ``` |  | -
$lt | ``` { "Elevation": { $lt: 5000 } } ``` |  | -
$lte | ``` { "Elevation": { $lte: 5000 } } ``` | | -
$ne | ``` { "Elevation": { $ne: 1 } } ``` |  | -
içinde $ | ``` { "Volcano Name": { $in: ["St. Helens", "Rainier", "Glacier Peak"] } } ``` |  | -
$nin | ``` { "Volcano Name": { $nin: ["Lassen Peak", "Hood", "Baker"] } } ``` | | -
$veya | ``` { $or: [ { Elevation: { $lt: 4000 } }, { "Volcano Name": "Rainier" } ] } ``` |  | -
$ve | ``` { $and: [ { Elevation: { $gt: 4000 } }, { "Volcano Name": "Rainier" } ] } ``` |  | -
$not | ``` { "Elevation": { $not: { $gt: 5000 } } } ```|  | -
$ ya da | ``` { $nor: [ { "Elevation": { $lt: 4000 } }, { "Volcano Name": "Baker" } ] } ``` |  | -
$var. | ``` { "Status": { $exists: true } } ```|  | -
$type | ``` { "Status": { $type: "string" } } ```|  | -
$mod | ``` { "Elevation": { $mod: [ 4, 0 ] } } ``` |  | -
$regex | ``` { "Volcano Name": { $regex: "^Rain"} } ```|  | -

### <a name="notes"></a>Notlar

$Regex sorgularda dizin arama sola bağlantılı ifadeleri izin verin. Ancak, 'i' kullanan değiştiricisi (olmama durumunu) ve 'M ' değiştiricisi (multiline) tüm ifadelerinde toplama tarama neden olur.
Eklenecek '$' bir gereksinim olduğunda veya ' |', iki (veya daha fazla) regex sorguları oluşturmak en iyisidir. Örneğin, aşağıdaki özgün sorgu verilen: ```find({x:{$regex: /^abc$/})```, şu şekilde değiştirilecek vardır: ```find({x:{$regex: /^abc/, x:{$regex:/^abc$/}})```.
İlk bölümü dizin arama itibaren bu belgeleri sınırlamak için kullanacağı ^ tam girişleri abc ve ikinci bölümü ile eşleşir. Çubuğu işleci ' |' bir "veya" işlevi - sorgu davranır ```find({x:{$regex: /^abc|^def/})``` 'x' alanı "abc" veya "def" ile başlayan değere sahip belgeleri whin eşleşir. Dizin faydalanmak için sorgu $ya da operatör tarafından birleştirilmiş iki farklı sorgular bölüneceği önerilir: ```find( {$or : [{x: $regex: /^abc/}, {$regex: /^def/}] })```.


### <a name="geospatial-operators"></a>Jeo-uzamsal işleçleri

İşleç | Örnek 
--- | --- |
$geoWithin | ```{ "Location.coordinates": { $geoWithin: { $centerSphere: [ [ -121, 46 ], 5 ] } } }``` | Evet
$geoIntersects |  ```{ "Location.coordinates": { $geoIntersects: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Evet
$yakın | ```{ "Location.coordinates": { $near: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Evet
$nearSphere | ```{ "Location.coordinates": { $nearSphere : [ -121, 46  ], $maxDistance: 0.50 } }``` | Evet
$geometry | ```{ "Location.coordinates": { $geoWithin: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Evet
$minDistance | ```{ "Location.coordinates": { $nearSphere : { $geometry: {type: "Point", coordinates: [ -121, 46 ]}, $minDistance: 1000, $maxDistance: 1000000 } } }``` | Evet
$maxDistance | ```{ "Location.coordinates": { $nearSphere : [ -121, 46  ], $maxDistance: 0.50 } }``` | Evet
$center | ```{ "Location.coordinates": { $geoWithin: { $center: [ [-121, 46], 1 ] } } }``` | Evet
$centerSphere | ```{ "Location.coordinates": { $geoWithin: { $centerSphere: [ [ -121, 46 ], 5 ] } } }``` | Evet
$box | ```{ "Location.coordinates": { $geoWithin: { $box:  [ [ 0, 0 ], [ -122, 47 ] ] } } }``` | Evet
$polygon | ```{ "Location.coordinates": { $near: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Evet

## <a name="additional-operators"></a>Ek işleçler

İşleç | Örnek | Notlar 
--- | --- | --- |
$all | ```{ "Location.coordinates": { $all: [-121.758, 46.87] } }``` | 
$elemMatch | ```{ "Location.coordinates": { $elemMatch: {  $lt: 0 } } }``` |  
$size | ```{ "Location.coordinates": { $size: 2 } }``` | 
$comment |  ```{ "Location.coordinates": { $elemMatch: {  $lt: 0 } }, $comment: "Negative values"}``` | 
$text |  | Desteklenmiyor. Bunun yerine $regex kullanın 

### <a name="methods"></a>Yöntemler

Aşağıdaki yöntemlerden desteklenir:

#### <a name="cursor-methods"></a>İmleç yöntemleri

Yöntem | Örnek | Notlar 
--- | --- | --- |
Cursor.sort() | ```cursor.sort({ "Elevation": -1 })``` | Belgeler sıralama anahtarı olmadan geri

## <a name="unique-indexes"></a>Benzersiz dizinler

Azure Cosmos DB varsayılan olarak veritabanına yazılır belgelerde her alan dizinler. Benzersiz dizinler belirli bir alan bir koleksiyondaki tüm belgeler arasında yinelenen değer yok, varsayılan "_ıd" anahtarı benzer şekilde benzersizlik şekilde korunduğundan emin olun. Artık 'benzersiz' kısıtlaması dahil olmak üzere CreateIndex komutunu kullanarak Azure Cosmos DB'de özel dizinler oluşturabilirsiniz.

Benzersiz dizinler tüm MongoDB API hesapları için kullanılabilir.

## <a name="time-to-live-ttl"></a>İçin-yaşam süresi (TTL)

Azure Cosmos DB belgenin zaman damgasını göre göreli bir yaşam süresi (TTL) destekler. TTL, MongoDB API koleksiyonlarına için etkinleştirilebilir [Azure portal](https://portal.azure.com).

## <a name="user-and-role-management"></a>Kullanıcı ve rol yönetimi

Kullanıcıları ve rolleri Azure Cosmos DB henüz desteklemiyor. Azure Cosmos DB rol tabanlı erişim denetimi (RBAC) destekler ve okuma-yazma ve salt okunur parolaları/aracılığıyla edinilen anahtarları [Azure portal](https://portal.azure.com) (bağlantı dizesi sayfası).

## <a name="replication"></a>Çoğaltma

Azure Cosmos DB en düşük katmanları otomatik, yerel çoğaltmasını destekler. Bu mantık, düşük gecikme süreli, genel çoğaltma da elde etmek için kullanıma genişletilir. Azure Cosmos DB el ile çoğaltma komutları desteklemiyor.

## <a name="sharding"></a>Parçalama

Azure Cosmos DB otomatik, sunucu tarafı parçalama destekler. Azure Cosmos DB el ile parçalama komutları desteklemiyor.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi nasıl [Studio 3T kullanmak](mongodb-mongochef.md) bir API MongoDB veritabanı ile.
- Bilgi nasıl [Robo 3T kullanmak](mongodb-robomongo.md) bir API MongoDB veritabanı ile.
- MongoDB için protokol desteği ile Azure Cosmos DB keşfedin [örnekleri](mongodb-samples.md).
