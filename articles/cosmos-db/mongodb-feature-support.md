---
title: MongoDB için Azure Cosmos DB özellik desteği | Microsoft Docs
description: Azure Cosmos DB MongoDB API'sinin MongoDB 3.4 için sağladığı özellik desteği hakkında bilgi edinin.
services: cosmos-db
author: alekseys
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: overview
ms.date: 11/15/2017
ms.author: alekseys
ms.openlocfilehash: 2c86cbe2ac9a0611873aca35480af92304abe5b5
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928701"
---
# <a name="mongodb-api-support-for-mongodb-features-and-syntax"></a>MongoDB özellikleri ve söz dizimi için MongoDB API’si desteği

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Veritabanının MongoDB API'siyle iletişim kurmak için herhangi bir açık kaynaklı MongoDB istemcisinin [sürücülerini](https://docs.mongodb.org/ecosystem/drivers) kullanabilirsiniz. MongoDB API'si, MongoDB [kablo protokolüne](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol) bağlı kalarak mevcut istemci sürücülerinin kullanımını etkinleştirir.

Azure Cosmos DB MongoDB API'sini kullanarak, Azure Cosmos DB'nin sağladığı tüm kurumsal olanaklarla birlikte tanıdığınız MongoDB API'lerinin avantajlarından yararlanabilirsiniz: [küresel dağıtım](distribute-data-globally.md), [otomatik parçalama](partition-data.md), kullanılabilirlik ve gecikme süresi garantileri, her alanın otomatik olarak dizininin oluşturulması, REST'te şifreleme, yedekler ve daha pek çok şey.

## <a name="mongodb-protocol-support"></a>MongoDB Protokol Desteği

Azure Cosmos DB MongoDB API'si, MongoDB Sunucusu’nun **3.2** sürümüyle varsayılan olarak uyumludur. Desteklenen işleçler ve tüm sınırlamalar veya özel durumlar aşağıda listelenmiştir. MongoDB’nin **3.4** sürümünde eklenen özellikler ve sorgu işleçleri şu anda bir önizleme özelliği olarak kullanılabilir. Bu protokolleri anlayan bir istemci sürücüsü Cosmos DB'ye MongoDB API'sini kullanarak bağlanabilir.

[MongoDB toplama işlem hattı](#aggregation-pipeline) da ayrı bir önizleme özelliği olarak kullanılabilir.

## <a name="mongodb-query-language-support"></a>MongoDB sorgu dili desteği

Azure Cosmos DB MongoDB API'si, MongoDB sorgu dili yapıları için kapsamlı destek sağlar. Aşağıda şu anda desteklenen işlem, işleç, aşama, komut ve seçeneklerin ayrıntılı bir listesini bulabilirsiniz.

## <a name="database-commands"></a>Veritabanı komutları

Azure Cosmos DB, tüm MongoDB API'si hesaplarında aşağıdaki veritabanı komutlarını destekler.

### <a name="query-and-write-operation-commands"></a>Sorgulama ve yazma işlemi komutları
- delete
- find
- findAndModify
- getLastError
- getMore
- insert
- update

### <a name="authentication-commands"></a>Kimlik doğrulama komutları
- logout
- authenticate
- getnonce

### <a name="administration-commands"></a>Yönetim komutları
- dropDatabase
- listCollections
- drop
- oluşturmaya
- filemd5
- createIndexes
- listIndexes
- dropIndexes
- connectionStatus
- reIndex

### <a name="diagnostics-commands"></a>Tanılama komutları
- buildInfo
- collStats
- dbStats
- hostInfo
- listDatabases
- whatsmyuri

<a name="aggregation-pipeline"/>

## <a name="aggregation-pipelinea"></a>Toplama ardışık düzeni</a>

Azure Cosmos DB, genel önizlemede toplama ardışık düzenini destekler. Genel önizlemenin katılımı hakkında yönergeler için [Azure bloguna](https://aka.ms/mongodb-aggregation) bakın.

### <a name="aggregation-commands"></a>Toplama komutları
- aggregate
- count
- distinct

### <a name="aggregation-stages"></a>Toplama aşamaları
- $project
- $match
- $limit
- $skip
- $unwind
- $group
- $sample
- $sort
- $lookup
- $out
- $count
- $addFields

### <a name="aggregation-expressions"></a>Toplama ifadeleri

#### <a name="boolean-expressions"></a>Mantıksal ifadeler
- $and
- $or
- $not

#### <a name="set-expressions"></a>Küme ifadeleri
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
- $add
- $ceil
- $divide
- $exp
- $floor
- $ln
- $log
- $log10
- $mod
- $multiply
- $pow
- $sqrt
- $subtract
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
- $in

#### <a name="date-expressions"></a>Tarih ifadeleri
- $dayOfYear
- $dayOfMonth
- $dayOfWeek
- $year
- $month
- $week
- $hour
- $minute
- $second
- $millisecond
- $isoDayOfWeek
- $isoWeek

#### <a name="conditional-expressions"></a>Koşullu ifadeler
- $cond
- $ifNull

## <a name="aggregation-accumulators"></a>Toplama biriktiricileri
- $sum
- $avg
- $first
- $last
- $max
- $min
- $push
- $addToSet

## <a name="operators"></a>İşleçler

Desteklenen işleçler, bunların kullanımıyla ilgili örneklerle birlikte aşağıda verilmiştir. Sorgularda kullanılan bu örnek belgeyi göz önünde bulundurun:

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
$in | ``` { "Volcano Name": { $in: ["St. Helens", "Rainier", "Glacier Peak"] } } ``` |  | -
$nin | ``` { "Volcano Name": { $nin: ["Lassen Peak", "Hood", "Baker"] } } ``` | | -
$or | ``` { $or: [ { Elevation: { $lt: 4000 } }, { "Volcano Name": "Rainier" } ] } ``` |  | -
$and | ``` { $and: [ { Elevation: { $gt: 4000 } }, { "Volcano Name": "Rainier" } ] } ``` |  | -
$not | ``` { "Elevation": { $not: { $gt: 5000 } } } ```|  | -
$nor | ``` { $nor: [ { "Elevation": { $lt: 4000 } }, { "Volcano Name": "Baker" } ] } ``` |  | -
$exists | ``` { "Status": { $exists: true } } ```|  | -
$type | ``` { "Status": { $type: "string" } } ```|  | -
$mod | ``` { "Elevation": { $mod: [ 4, 0 ] } } ``` |  | -
$regex | ``` { "Volcano Name": { $regex: "^Rain"} } ```|  | -

### <a name="notes"></a>Notlar

Normal ifade ($regex) sorgularında, soldan başlayan ifadeler dizin aramasına izin verir. Ancak 'i' değiştiricisini (büyük/küçük harf duyarlığı) ve 'm' değiştiricisini (çok satırlılık) kullanmak, tüm ifadelerde koleksiyon taramasına neden olur.
'$' veya '|' karakterinin dahil edilmesinin gerektiği durumlarda iki (veya daha fazla) normal ifade sorgusu oluşturmak en iyisidir. Örneğin şu özgün sorgu: ```find({x:{$regex: /^abc$/})``` şu şekilde değiştirilmelidir: ```find({x:{$regex: /^abc/, x:{$regex:/^abc$/}})```.
İlk kısım aramayı ^abc ile başlayan belgeler ile sınırlamak için dizini kullanır, ikinci kısım ise tam girişler ile eşleşir. Çubuk işleci '|' "veya" işlevini görür - ```find({x:{$regex: /^abc|^def/})``` sorgusu 'x' alanının değerlerinin "abc" ile veya "def" ile başladığı belgelerle eşleşir. Dizinden yararlanmak için sorgunun $or işleci ile birleştirilen iki farklı sorguya bölünmesi gerekir: ```find( {$or : [{x: $regex: /^abc/}, {$regex: /^def/}] })```.

### <a name="update-operators"></a>Güncelleştirme işleçleri

#### <a name="field-update-operators"></a>Alan güncelleştirme işleçleri
- $inc
- $mul
- $rename
- $setOnInsert
- $set
- $unset
- $min
- $max
- $currentDate

#### <a name="array-update-operators"></a>Dizi güncelleştirme işleçleri
- $addToSet
- $pop
- $pullAll
- $pull (Not: koşulu $pull desteklenmez)
- $pushAll
- $push
- $each
- $slice
- $sort
- $position

#### <a name="bitwise-update-operator"></a>Bit düzeyinde güncelleştirme işleci
- $bit

### <a name="geospatial-operators"></a>Jeo-uzamsal işleçler

İşleç | Örnek 
--- | --- |
$geoWithin | ```{ "Location.coordinates": { $geoWithin: { $centerSphere: [ [ -121, 46 ], 5 ] } } }``` | Yes
$geoIntersects |  ```{ "Location.coordinates": { $geoIntersects: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Yes
$near | ```{ "Location.coordinates": { $near: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Yes
$nearSphere | ```{ "Location.coordinates": { $nearSphere : [ -121, 46  ], $maxDistance: 0.50 } }``` | Yes
$geometry | ```{ "Location.coordinates": { $geoWithin: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Yes
$minDistance | ```{ "Location.coordinates": { $nearSphere : { $geometry: {type: "Point", coordinates: [ -121, 46 ]}, $minDistance: 1000, $maxDistance: 1000000 } } }``` | Yes
$maxDistance | ```{ "Location.coordinates": { $nearSphere : [ -121, 46  ], $maxDistance: 0.50 } }``` | Yes
$center | ```{ "Location.coordinates": { $geoWithin: { $center: [ [-121, 46], 1 ] } } }``` | Yes
$centerSphere | ```{ "Location.coordinates": { $geoWithin: { $centerSphere: [ [ -121, 46 ], 5 ] } } }``` | Yes
$box | ```{ "Location.coordinates": { $geoWithin: { $box:  [ [ 0, 0 ], [ -122, 47 ] ] } } }``` | Yes
$polygon | ```{ "Location.coordinates": { $near: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Yes

## <a name="additional-operators"></a>Ek işleçler

İşleç | Örnek | Notlar 
--- | --- | --- |
$all | ```{ "Location.coordinates": { $all: [-121.758, 46.87] } }``` | 
$elemMatch | ```{ "Location.coordinates": { $elemMatch: {  $lt: 0 } } }``` |  
$size | ```{ "Location.coordinates": { $size: 2 } }``` | 
$comment |  ```{ "Location.coordinates": { $elemMatch: {  $lt: 0 } }, $comment: "Negative values"}``` | 
$text |  | Desteklenmiyor. Bunun yerine $regex kullanın.

## <a name="unsupported-operators"></a>Desteklenmeyen işleçler

```$where``` ve ```$eval``` işleçleri, Azure Cosmos DB tarafından desteklenmiyor.

### <a name="methods"></a>Yöntemler

Aşağıdaki yöntemler desteklenir:

#### <a name="cursor-methods"></a>İmleç yöntemleri

Yöntem | Örnek | Notlar 
--- | --- | --- |
cursor.sort() | ```cursor.sort({ "Elevation": -1 })``` | Sıralama anahtarı olmayan belgeler döndürülmez

## <a name="unique-indexes"></a>Benzersiz dizinler

Azure Cosmos DB, varsayılan olarak veritabanına yazılan belgelerdeki her alanın dizinini oluşturur. Benzersiz dizinler, varsayılan "_id" anahtarında özgünlüğün korunmasına benzer bir şekilde bir koleksiyondaki tüm belgeler genelinde belirli bir alanda yinelenen değer olmamasını sağlar. Azure Cosmos DB'de artık createIndex komutunu kullanarak, 'unique' kısıtlamasını dahil ederek özel dizinler oluşturabilirsiniz.

Benzersiz dizinler tüm MongoDB API hesaplarında bulunur.

## <a name="time-to-live-ttl"></a>Etkin kalma süresi (TTL)

Azure Cosmos DB, belgenin zaman damgasına bağlı göreli bir etkin kalma süresini (TTL) destekler. TTL, MongoDB API koleksiyonlarında [Azure portalı](https://portal.azure.com) aracılığıyla etkinleştirilebilir.

## <a name="user-and-role-management"></a>Kullanıcı ve rol yönetimi

Azure Cosmos DB henüz kullanıcıları ve rolleri desteklememektedir. Azure Cosmos DB, rol tabanlı erişim denetimini (RBAC) ve [Azure portalı](https://portal.azure.com) (Bağlantı Dizesi sayfası) aracılığıyla edinilebilecek okuma-yazma ve salt-okuma parolalarını/anahtarlarını destekler.

## <a name="replication"></a>Çoğaltma

Azure Cosmos DB, en düşük katmanlarda otomatik, yerel çoğaltmayı destekler. Bu mantık, düşük gecikme süresi ve küresel çoğaltma elde etmek için genişletilir. Azure Cosmos DB, el ile çoğaltma komutlarını desteklemez.

## <a name="write-concern"></a>Yazma Sorunu

Belirli MongoDB API’leri, bir yazma işlemi sırasında gereken yanıt sayısını belirleyen [Yazma Sorunu](https://docs.mongodb.com/manual/reference/write-concern/)’nu belirtme özelliğini destekler. Cosmos DB’nin arka planda çoğaltma işleme şekli nedeniyle varsayılan olarak tüm yazma işlemleri otomatik olarak çekirdektir. İstemci kodu tarafından belirtilen tüm yazma sorunları göz ardı edilir. Daha fazla bilgi için bkz. [Kullanılabilirlik ve performansı en üst düzeye çıkarmak için tutarlılık düzeylerini kullanma](consistency-levels.md).

## <a name="sharding"></a>Parçalama

Azure Cosmos DB, otomatik, sunucu tarafı parçalamasını destekler. Azure Cosmos DB, el ile parçalama komutlarını desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

- Bir API for MongoDB veritabanı ile [Studio 3T kullanmayı](mongodb-mongochef.md) öğrenin.
- Bir API for MongoDB veritabanı ile [Robo 3T kullanmayı](mongodb-robomongo.md) öğrenin.
- MongoDB için protokol desteği [örnekleri](mongodb-samples.md) ile Azure Cosmos DB'yi keşfedin.
