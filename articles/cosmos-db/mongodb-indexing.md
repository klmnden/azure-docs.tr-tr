---
title: MongoDB için Azure Cosmos DB'nin API'SİNDE dizinleme
description: Dizinleme özelliklerine genel bakış için MongoDB API'si ile Azure Cosmos DB'nin sunar.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 12/26/2018
author: sivethe
ms.author: sivethe
ms.openlocfilehash: de037316efa50dd25ea04c370fa0e5878fb52ba1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60448512"
---
# <a name="indexing-using-azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB'nin MongoDB kullanarak dizin oluşturma

Azure Cosmos DB'nin MongoDB API'si, Cosmos DB'nin otomatik dizin yönetimi özelliklerinden yararlanır. Sonuç olarak, kullanıcılar varsayılan dizinleme ilkelerine Cosmos DB'nin erişebilir. Bu nedenle, kullanıcı tarafından tanımlanmış dizin yok veya dizin bırakılmamışsa, ardından tüm alanları otomatik olarak bir koleksiyona eklendiğinde varsayılan olarak dizinlenir. Çoğu senaryoda, hesap üzerinde ayarlanmış varsayılan dizinleme ilkesinin kullanılması önerilir.

## <a name="dropping-the-default-indexes"></a>Varsayılan dizinleri bırakma

Bir koleksiyon ```coll``` için varsayılan dizinleri bırakmak üzere aşağıdaki komut kullanılabilir:

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

## <a name="creating-compound-indexes"></a>Bileşik dizinler oluşturma

Bileşik dizinler bir belgenin birden çok alanına başvurular içerir. Mantıksal olarak, alan başına birden fazla tek dizin oluşturmaya eşdeğerdir. Cosmos DB dizinleme teknikleri tarafından sağlanan iyileştirmelerden yararlanmak için tek (benzersiz olmayan) bir bileşik dizin yerine birden fazla tek dizin oluşturulması önerilir.

## <a name="creating-unique-indexes"></a>Benzersiz dizinler oluşturma

[Benzersiz dizinler](unique-keys.md), iki veya daha fazla belgeyi dizini oluşturulmuş alanlar için aynı değeri içermemeye zorlarken yararlıdır. 
>[!important] 
> Şu anda benzersiz dizinler yalnızca koleksiyon boş olduğunda (hiçbir belge içermediğinde) oluşturulabilir. 

Aşağıdaki komut, "student_id" alan benzersiz bir dizin oluşturur:

```JavaScript
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1 }, {unique:true} ) 
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 4
}
```

Parçalı koleksiyonlar için MongoDB davranışına göre benzersiz bir dizin oluşturmak, parça (bölüm) anahtarının sağlanmasını gerektirir. Diğer bir deyişle, parçalı bir koleksiyondaki tüm benzersiz dizinler, alanlardan birinin bölüm anahtarı olduğu bileşik dizinlerdir.

Aşağıdaki komutlar student_id ve university alanlarında benzersiz bir dizin ile ```coll``` parçalı koleksiyonu (parça anahtarı ```university```) oluşturur:

```JavaScript
globaldb:PRIMARY> db.runCommand({shardCollection: db.coll._fullName, key: { university: "hashed"}});
{
        "_t" : "ShardCollectionResponse",
        "ok" : 1,
        "collectionsharded" : "test.coll"
}
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1, "university" : 1 }, {unique:true})
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 3,
        "numIndexesAfter" : 4
}
```

Yukarıdaki örnekte ```"university":1``` yan tümcesi çıkarılsaydı aşağıdaki iletiyle bir hata döndürülürdü:

```"cannot create unique index over {student_id : 1.0} with shard key pattern { university : 1.0 }"```

## <a name="ttl-indexes"></a>TTL dizinleri

Belirli bir koleksiyonda belge geçerlilik süresini etkinleştirmek için bir ["TTL dizini" (yaşam süresi dizini)](../cosmos-db/time-to-live.md) oluşturulması gerekir. TTL dizini, _ts alanında "expireAfterSeconds" değerini içeren bir dizindir.
 
Örnek:
```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

Yukarıdaki komut, ```db.coll``` koleksiyonunda son 10 saniye içinde değiştirilmemiş tüm belgelerin silinmesine neden olur. 
 
> [!NOTE]
> **_ts**, Cosmos DB’ye özel bir alandır ve MongoDB istemcilerinden erişilemez. Belgenin son değiştirme tarihinin zaman damgasını içeren ayrılmış (sistem) bir özelliktir.
>

## <a name="migrating-collections-with-indexes"></a>Koleksiyonları dizinlerle geçirme

Şu anda benzersiz dizinler yalnızca koleksiyon hiçbir belge içermediğinde oluşturulabilir. Popüler MongoDB geçiş araçları, verileri içeri aktardıktan sonra benzersiz dizin oluşturmaya çalışır. Bu sorunu aşmak için kullanıcıların geçiş aracına izin vermek yerine karşılık gelen koleksiyonları ve benzersiz dizinleri el ile oluşturması önerilir (```mongorestore``` için bu davranış komut satırında --noIndexRestore bayrağı kullanılarak elde edilir).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Cosmos DB'yi dizine ekleme](../cosmos-db/index-policy.md)
* [Yaşam süresi otomatik olarak Azure Cosmos DB'de verilerle süresi dolacak](../cosmos-db/time-to-live.md)
