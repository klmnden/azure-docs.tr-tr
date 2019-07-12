---
title: Yapılandırma ve Azure Cosmos DB'de yaşam süresi yönetme hakkında bilgi edinin
description: Yapılandırma ve Azure Cosmos DB'de yaşam süresi yönetme hakkında bilgi edinin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: mjbrown
ms.openlocfilehash: 618e7e19b20f361aa0a8c668e9621a29db43772d
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797747"
---
# <a name="configure-time-to-live-in-azure-cosmos-db"></a>Azure Cosmos DB'de yaşam süresi yapılandırma

Azure Cosmos DB, yaşam süresi (TTL) kapsayıcı düzeyinde yapılandırmayı seçtiğinize veya kapsayıcının ayarlanmasından sonra bir öğe düzeyinde kılabilirsiniz. Azure portalı veya dile özgü SDK'larını kullanarak bir kapsayıcı için TTL yapılandırabilirsiniz. Öğe düzeyinde TTL geçersiz kılmalar, SDK'ları kullanılarak yapılandırılabilir.

## <a name="enable-time-to-live-on-a-container-using-azure-portal"></a>Azure portalını kullanarak bir kapsayıcı yaşam süresi etkinleştir

Süre sonu olmayan bir kapsayıcıda yaşam süresi'ni etkinleştirmek için aşağıdaki adımları kullanın. Öğe düzeyinde geçersiz kılınacak TTL izin vermek için bunu etkinleştirin. TTL saniye için sıfır olmayan bir değer girerek de ayarlayabilirsiniz.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Yeni bir Azure Cosmos hesabı oluşturun veya mevcut bir hesabı seçin.

3. Açık **Veri Gezgini** bölmesi.

4. Var olan bir kapsayıcı seçin, genişletin ve aşağıdaki değerleri değiştirin:

   * Açık **ölçek ve ayarlar** penceresi.
   * Altında **ayarı** Bul **yaşam süresi**.
   * Seçin **(varsayılan) üzerinde** veya **üzerinde** ve TTL değerini ayarlayın
   * Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

   ![Azure portalında yaşam süresi yapılandırma](./media/how-to-time-to-live/how-to-time-to-live-portal.png)


- DefaultTimeToLive null olduğunda, yaşam süresi kapalı ise
- DefaultTimeToLive -1 sonra canlı ayarı zaman olduğunda (varsayılan olarak) olan
- Herhangi bir tamsayı değer (0) hariç DefaultTimeToLive sahip olduğunda Canlı ayarı zaman açıktır

## <a name="enable-time-to-live-on-a-container-using-sdk"></a>SDK'sını kullanarak bir kapsayıcı yaşam süresi etkinleştir

### <a id="dotnet-enable-noexpiry"></a>.NET SDK

```csharp
// Create a new collection with TTL enabled and without any expiration value
DocumentCollection collectionDefinition = new DocumentCollection();
collectionDefinition.Id = "myContainer";
collectionDefinition.PartitionKey.Paths.Add("/myPartitionKey");
collectionDefinition.DefaultTimeToLive = -1; //(never expire by default)

DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    collectionDefinition,
    new RequestOptions { OfferThroughput = 20000 });
```

## <a name="set-time-to-live-on-a-container-using-sdk"></a>SDK'sını kullanarak bir kapsayıcı yaşam süresi ayarlayın

### <a id="dotnet-enable-withexpiry"></a>.NET SDK

Bir kapsayıcı yaşam süresini ayarlamak için süre saniye cinsinden belirten sıfır olmayan pozitif bir sayı vermeniz gerekir. Yapılandırılmış TTL değeri temel alınarak, tüm öğeleri kapsayıcıda son öğenin zaman damgasını değiştirildikten sonra `_ts` silinir.

```csharp
// Create a new collection with TTL enabled and a 90 day expiration
DocumentCollection collectionDefinition = new DocumentCollection();
collectionDefinition.Id = "myContainer";
collectionDefinition.PartitionKey.Paths.Add("/myPartitionKey");
collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days

DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    collectionDefinition,
    new RequestOptions { OfferThroughput = 20000 });
```

### <a id="nodejs-enable-withexpiry"></a>NodeJS SDK'sı

```javascript
const containerDefinition = {
          id: "sample container1",
        };

async function createcontainerWithTTL(db: Database, containerDefinition: ContainerDefinition, collId: any, defaultTtl: number) {
      containerDefinition.id = collId;
      containerDefinition.defaultTtl = defaultTtl;
      await db.containers.create(containerDefinition);
}
```

## <a name="set-time-to-live-on-an-item"></a>Bir öğe yaşam süresini ayarlama

Bir varsayılan bir kapsayıcı yaşam süresi ayarlamanın yanı sıra, bir öğe için bir süresi ayarlayabilirsiniz. Öğe düzeyinde yaşam süresi ayarı varsayılan TTL kapsayıcıdaki öğesi geçersiz kılar.

* TTL öge üzerinde ayarlamak için öğenin son öğenin zaman damgasını değiştirildikten sonra süresi dolacak şekilde saniye cinsinden süre belirten sıfır olmayan pozitif bir sayı, sağlamanız gerekir `_ts`.

* TTL alan öğe yoksa, varsayılan olarak, kapsayıcıya ayarlamak TTL öğesine uygulanır.

* TTL kapsayıcı düzeyinde devre dışı bırakılırsa, TTL kapsayıcıdaki yeniden etkinleştirilene kadar TTL alan öğe üzerinde yoksayılır.

### <a id="portal-set-ttl-item"></a>Azure portal

Bir öğe üzerinde yaşam süresi'ni etkinleştirmek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Yeni bir Azure Cosmos hesabı oluşturun veya mevcut bir hesabı seçin.

3. Açık **Veri Gezgini** bölmesi.

4. Var olan bir kapsayıcı seçin, genişletin ve aşağıdaki değerleri değiştirin:

   * Açık **ölçek ve ayarlar** penceresi.
   * Altında **ayarı** Bul **yaşam süresi**.
   * Seçin **(varsayılan) üzerinde** veya **üzerinde** ve TTL değerini ayarlayın. 
   * Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

5. Sonraki zaman canlı, eklemek için ayarlamak istediğiniz öğeye Git `ttl` özelliğini tıklatın ve **güncelleştirme**. 

   ```json
   {
    "id": "1",
    "_rid": "Jic9ANWdO-EFAAAAAAAAAA==",
    "_self": "dbs/Jic9AA==/colls/Jic9ANWdO-E=/docs/Jic9ANWdO-EFAAAAAAAAAA==/",
    "_etag": "\"0d00b23f-0000-0000-0000-5c7712e80000\"",
    "_attachments": "attachments/",
    "ttl": 10,
    "_ts": 1551307496
   }
   ```

### <a id="dotnet-set-ttl-item"></a>.NET SDK

```csharp
// Include a property that serializes to "ttl" in JSON
public class SalesOrder
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    [JsonProperty(PropertyName="cid")]
    public string CustomerId { get; set; }
    // used to set expiration policy
    [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
    public int? ttl { get; set; }

    //...
}
// Set the value to the expiration in seconds
SalesOrder salesOrder = new SalesOrder
{
    Id = "SO05",
    CustomerId = "CO18009186470",
    ttl = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days
};
```

### <a id="nodejs-set-ttl-item"></a>NodeJS SDK'sı

```javascript
const itemDefinition = {
          id: "doc",
          name: "sample Item",
          key: "value", 
          ttl: 2
        };
```


## <a name="reset-time-to-live"></a>Yaşam süresi Sıfırla

Bir öğe üzerinde bir yazma gerçekleştirerek live veya güncelleştirme işlemi ' maddesinde zaman sıfırlayabilirsiniz. Yazma veya güncelleştirme işlemi ayarlayacak `_ts` geçerli saat ve TTL süresi dolacak şekilde öğe için yeniden başlar. Bir öğenin TTL değiştirmek istiyorsanız, herhangi bir alan güncelleştirirken, alanın güncelleştirebilirsiniz.

### <a id="dotnet-extend-ttl-item"></a>.NET SDK

```csharp
// This examples leverages the Sales Order class above.
// Read a document, update its TTL, save it.
response = await client.ReadDocumentAsync(
    "/dbs/salesdb/colls/orders/docs/SO05"),
    new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });

Document readDocument = response.Resource;
readDocument.ttl = 60 * 30 * 30; // update time to live
response = await client.ReplaceDocumentAsync(readDocument);
```

## <a name="turn-off-time-to-live"></a>Yaşam süresi Kapat

Yaşam süresi bir öğe üzerinde ayarlanmış ve süresi dolacak şekilde bu öğe artık istemiyorsanız, ardından öğesi alın, TTL alanı kaldırabileceğiniz ve öğe sunucuda değiştirin. TTL alan öğe kaldırıldığında, kapsayıcıya atanmış varsayılan TTL değeri öğesine uygulanır. Bir öğenin süresinin dolmasını engellemek için ve TTL değeri kapsayıcıdan devralmayı-1 TTL değerini ayarlayın.

### <a id="dotnet-turn-off-ttl-item"></a>.NET SDK

```csharp
// This examples leverages the Sales Order class above.
// Read a document, turn off its override TTL, save it.
response = await client.ReadDocumentAsync(
    "/dbs/salesdb/colls/orders/docs/SO05"),
    new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });

Document readDocument = response.Resource;
readDocument.ttl = null; // inherit the default TTL of the collection

response = await client.ReplaceDocumentAsync(readDocument);
```

## <a name="disable-time-to-live"></a>Yaşam süresi devre dışı bırak

Bir kapsayıcı canlı ve süresi geçmiş öğeler için denetlemesini arka plan işlemini durdurmak için zaman devre dışı bırakmak için `DefaultTimeToLive` özelliği kapsayıcı üzerindeki silinmelidir. Bu özelliği silme -1 olarak ayarlama özelliğinden farklıdır. -1 olarak ayarlarsanız, kapsayıcıya eklenen yeni öğeleri kapsayıcıdaki belirli öğeleri bu değeri geçersiz kılabilirsiniz ancak sonsuza kadar yaşayacaksa. TTL özelliğine kapsayıcıdan kaldırdığınızda olsa bile önceki varsayılan TTL değeri açıkça geçersiz öğeleri hiçbir zaman, sona erecek.

### <a id="dotnet-disable-ttl"></a>.NET SDK

```csharp
// Get the collection, update DefaultTimeToLive to null
DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
// Disable TTL
collection.DefaultTimeToLive = null;
await client.ReplaceDocumentCollectionAsync(collection);
```

## <a name="next-steps"></a>Sonraki adımlar

Şu makalede yaşam süresi hakkında daha fazla bilgi edinin:

* [yaşam süresi](time-to-live.md)
