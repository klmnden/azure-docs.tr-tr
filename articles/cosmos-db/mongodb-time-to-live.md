---
title: Azure Cosmos DB MongoDB başına belge TTL özelliği
description: Canlı değeri otomatik olarak sistemden bir süre sonra temizlemek MongoDB için Azure Cosmos DB API hesabı süresini ayarlama konusunda bilgi edinin.
services: cosmos-db
author: orestis-ms
ms.author: orkostak
ms.service: cosmos-db
ms.devlang: javascript
ms.topic: quickstart
ms.date: 08/10/2018
ms.openlocfilehash: e82a6b055df87ea01025d01e32f31b0e962cf307
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53543674"
---
# <a name="expire-data-in-azure-cosmos-db-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API verileri süresi dolacak

Yaşam süresi (TTL) işlevi, veritabanının verilerin süresini otomatik olarak sonlandırmasını sağlar. MongoDB için Azure Cosmos DB API, Azure Cosmos DB'nin TTL özelliklerini kullanır. İki mod desteklenir: koleksiyonun tamamı için varsayılan TTL değeri ayarlama ve her belge için ayrı bir TTL değeri ayarlama. TTL dizinlerini ve belgeye özgü TTL değerlerini MongoDB API içinde yöneten mantık [Azure Cosmos DB ile aynıdır](../cosmos-db/mongodb-indexing.md).

## <a name="ttl-indexes"></a>TTL dizinleri
TTL'yi bir koleksiyonun tamamında etkinleştirmek için bir ["TTL dizini" (yaşam süresi dizini)](../cosmos-db/mongodb-indexing.md) oluşturulması gerekir. TTL dizini, _ts alanında "expireAfterSeconds" değerini içeren bir dizindir.

Örnek:
```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : true,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 4
}
```

Yukarıdaki örnekte verilen komut, TTL işlevine sahip bir dizin oluşturur. Dizin oluşturulduktan sonra veritabanı koleksiyon içinde bulunan ve son 10 saniyede değiştirilmemiş olan belgeleri otomatik olarak siler. 

> [!NOTE]
> **_ts**, Cosmos DB’ye özel bir alandır ve MongoDB istemcilerinden erişilemez. Belgenin son değiştirme tarihinin zaman damgasını içeren ayrılmış (sistem) bir özelliktir.
>
    
C# örneği de aşağıda belirtilmiştir: 

```csharp
var options = new CreateIndexOptions {ExpireAfter = TimeSpan.FromSeconds(10)}; 
var field = new StringFieldDefinition<BsonDocument>("_ts"); 
var indexDefinition = new IndexKeysDefinitionBuilder<BsonDocument>().Ascending(field); 
await collection.Indexes.CreateOneAsync(indexDefinition, options); 
``` 

## <a name="set-time-to-live-value-for-a-document"></a>Bir belge için yaşam süresi değeri belirleme 
Belge başına TTL değerleri de desteklenir. Belgelerde "ttl" (küçük harfle) kök düzey özellik olması ve bulunduğu koleksiyon için yukarıda anlatılan TTL dizini oluşturulmalıdır. Bir belge için ayarlanan TTL değerleri, koleksiyonunun TTL değerini geçersiz kılacaktır.

TTL değeri bir int32 olmalıdır. Alternatif olarak int32 aralığına sığan bir int64 değeri veya int32 aralığına sığan ve ondalık basamağı olmayan bir çift değer de kullanılabilir. Bu kurallara uymayan TTL özelliklerine izin verilir ancak bu değerler anlamlı bir belge TTL değeri olarak işlenmez.

Belge için TTL değeri isteğe bağlıdır; TTL değeri bulunmayan belgeler de aynı koleksiyona eklenebilir.  Bu durumda koleksiyonun TTL değeri kullanılacaktır. 

Aşağıdaki belgeler geçerli TTL değerlerine sahiptir. Belgeler eklendikten sonra belge TTL değeri koleksiyonun TTL değerini geçersiz kılar. Bu nedenle belgeler 20 saniye sonra kaldırılır.  

```JavaScript 
globaldb:PRIMARY> db.coll.insert({id:1, location: "Paris", ttl: 20.0}) 
globaldb:PRIMARY> db.coll.insert({id:1, location: "Paris", ttl: NumberInt(20)}) 
globaldb:PRIMARY> db.coll.insert({id:1, location: "Paris", ttl: NumberLong(20)}) 
```

Aşağıdaki belgeler geçersiz TTL değerlerine sahiptir. Bu belgeler eklenir ancak belgenin TTL değeri kullanılmaz. Bu nedenle belgeler koleksiyonun TTL değeri olan 10 saniye sonra kaldırılır. 

```JavaScript 
globaldb:PRIMARY> db.coll.insert({id:1, location: "Paris", ttl: 20.5}) //TTL value contains non-zero decimal part. 
globaldb:PRIMARY> db.coll.insert({id:1, location: "Paris", ttl: NumberLong(2147483649)}) //TTL value is greater than Int32.MaxValue (2,147,483,648). 
``` 

## <a name="how-to-activate-the-per-document-ttl-feature"></a>Belgeye özgü TTL özelliğini etkinleştirme
Belge başına TTL Özelliği Azure Cosmos DB API hesabı, MongoDB için etkinleştirilebilir. 

![Portalda belgeye özgü TTL özelliğini etkinleştirme ekran görüntüsü](./media/mongodb-ttl/mongodb_portal_ttl.png) 

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Cosmos DB koleksiyonlarındaki verileri yaşam süresi ile otomatik olarak sonlandır](../cosmos-db/time-to-live.md)
* [MongoDB için Azure Cosmos DB API dizin oluşturma](../cosmos-db/mongodb-indexing.md)
