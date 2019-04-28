---
title: Azure Cosmos DB bölgeleri arasındaki çakışmaları yönetme
description: Azure Cosmos DB'deki çakışmaları yönetme
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/16/2019
ms.author: mjbrown
ms.openlocfilehash: fb9850548f0bfb71b797830eb0d5fdfddbc32306
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61054807"
---
# <a name="manage-conflict-resolution-policies-in-azure-cosmos-db"></a>Azure Cosmos DB'de Çakışma çözümlemesi ilkelerini yönetme

Birden çok istemci aynı öğeye yazdığında, çok bölgeli yazma ile çakışmaları oluşabilir. Bir çakışma oluştuğunda, farklı çakışma çözümlemesi ilkeleri kullanarak çakışmayı çözebilirsiniz. Bu makalede, Çakışma çözümlemesi ilkelerini yönetmek açıklar.

## <a name="create-a-last-writer-wins-conflict-resolution-policy"></a>Son yazıcı WINS çakışma çözüm ilkesi oluşturma

Bu örnekler, son yazıcı WINS çakışma çözüm ilkesi içeren bir kapsayıcıya nasıl gösterir. Zaman damgası alanı varsayılan yoldur son yazıcı WINS veya `_ts` özelliği. Bu da bir sayısal tür için bir kullanıcı tanımlı yol için ayarlanabilir. İçinde bir çakışma en yüksek değer kazanır. Yolu ayarlanmamış veya geçersiz ise varsayılan `_ts`. Bu ilkeyle çözümlediği çakışma çakışması akıştaki gösterilmez. Bu ilke, tüm API'ları tarafından kullanılabilir.

### <a id="create-custom-conflict-resolution-policy-lww-dotnet"></a>.NET SDK

```csharp
DocumentCollection lwwCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.lwwCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.LastWriterWins,
          ConflictResolutionPath = "/myCustomId",
      },
  });
```

### <a id="create-custom-conflict-resolution-policy-lww-java-async"></a>Java Async SDK’sı

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createLastWriterWinsPolicy("/myCustomId");
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

### <a id="create-custom-conflict-resolution-policy-lww-java-sync"></a>Java Sync SDK’sı

```java
DocumentCollection lwwCollection = new DocumentCollection();
lwwCollection.setId(this.lwwCollectionName);
ConflictResolutionPolicy lwwPolicy = ConflictResolutionPolicy.createLastWriterWinsPolicy("/myCustomId");
lwwCollection.setConflictResolutionPolicy(lwwPolicy);
DocumentCollection createdCollection = this.tryCreateDocumentCollection(createClient, database, lwwCollection);
```

### <a id="create-custom-conflict-resolution-policy-lww-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const database = client.database(this.databaseName);
const { container: lwwContainer } = await database.containers.createIfNotExists(
  {
    id: this.lwwContainerName,
    conflictResolutionPolicy: {
      mode: "LastWriterWins",
      conflictResolutionPath: "/myCustomId"
    }
  }
);
```

### <a id="create-custom-conflict-resolution-policy-lww-python"></a>Python SDK’sı

```python
udp_collection = {
                'id': self.udp_collection_name,
                'conflictResolutionPolicy': {
                    'mode': 'LastWriterWins',
                    'conflictResolutionPath': '/myCustomId'
                    }
                }
udp_collection = self.try_create_document_collection(create_client, database, udp_collection)
```

## <a name="create-a-custom-conflict-resolution-policy-using-a-stored-procedure"></a>Bir saklı yordamı kullanarak bir özel çakışma çözüm ilkesi oluşturma

Bu örnekler çakışmayı çözümlemek için saklı yordama içeren özel çakışma çözümleme ilkesine sahip bir kapsayıcı ayarlama adımlarını göstermektedir. Bu çakışmaları, saklı yordamda hata olmadığı sürece akış çakışma görünmüyor. İlke ile kapsayıcı oluşturulduktan sonra saklı yordam oluşturmak gerekir. .NET SDK'sı aşağıdaki örnekte, bir örneği gösterilmektedir. Bu ilke yalnızca çekirdek (SQL) API desteklenir.

### <a name="sample-custom-conflict-resolution-stored-procedure"></a>Saklı yordamı örnek özel çakışma çözümü

Aşağıda gösterilen işlev imzası kullanarak özel çakışma çözümlemesi depolanan yordamları uygulanmalıdır. İşlev adı saklı yordamı ile kapsayıcı kaydolurken kullandığınız adıyla eşleşmesi gerekmez ancak adlandırma basitleştirin. Bu saklı yordam için uygulanması gereken parametreler açıklaması aşağıda verilmiştir.

- **incomingItem**: Öğe eklendiğinde veya çakışmasına neden işlemede güncelleştirildi. Silme işlemleri için null olur.
- **existingItem**: Şu anda taahhüt öğe. Bu değer, null olmayan bir güncelleştirmede ve bir INSERT veya delete için null olur.
- **isTombstone**: Daha önce silinmiş bir öğeyle çakışan incomingItem olup olmadığını belirten bir Boole değeri. ExistingItem da doğru olduğunda null olur.
- **conflictingItems**: İşlenmiş sürüm incomingItem kimliğine sahip çakışan bir kapsayıcıdaki tüm öğeleri veya diğer bir benzersiz dizin özellikleri dizisi.

> [!IMPORTANT]
> Gibi herhangi bir saklı yordamı ile özel çakışma çözümleme yordamı aynı bölüm anahtarı ile tüm verilere erişmek ve herhangi INSERT işlemi, güncelleştirme veya silme işlemi çakışmaları çözümlemek için.


Bu örnek depolanan yordam en düşük değerden seçerek çakışmaları çözer `/myCustomId` yolu.

```javascript
function resolver(incomingItem, existingItem, isTombstone, conflictingItems) {
  var collection = getContext().getCollection();

  if (!incomingItem) {
      if (existingItem) {

          collection.deleteDocument(existingItem._self, {}, function (err, responseOptions) {
              if (err) throw err;
          });
      }
  } else if (isTombstone) {
      // delete always wins.
  } else {
      if (existingItem) {
          if (incomingItem.myCustomId > existingItem.myCustomId) {
              return; // existing item wins
          }
      }

      var i;
      for (i = 0; i < conflictingItems.length; i++) {
          if (incomingItem.myCustomId > conflictingItems[i].myCustomId) {
              return; // existing conflict item wins
          }
      }

      // incoming item wins - clear conflicts and replace existing with incoming.
      tryDelete(conflictingItems, incomingItem, existingItem);
  }

  function tryDelete(documents, incoming, existing) {
      if (documents.length > 0) {
          collection.deleteDocument(documents[0]._self, {}, function (err, responseOptions) {
              if (err) throw err;

              documents.shift();
              tryDelete(documents, incoming, existing);
          });
      } else if (existing) {
          collection.replaceDocument(existing._self, incoming,
              function (err, documentCreated) {
                  if (err) throw err;
              });
      } else {
          collection.createDocument(collection.getSelfLink(), incoming,
              function (err, documentCreated) {
                  if (err) throw err;
              });
      }
  }
}
```

### <a id="create-custom-conflict-resolution-policy-stored-proc-dotnet"></a>.NET SDK

```csharp
DocumentCollection udpCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.udpCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.Custom,
          ConflictResolutionProcedure = string.Format("dbs/{0}/colls/{1}/sprocs/{2}", this.databaseName, this.udpCollectionName, "resolver"),
      },
  });

//Create the stored procedure
await clients[0].CreateStoredProcedureAsync(
UriFactory.CreateStoredProcedureUri(this.databaseName, this.udpCollectionName, "resolver"), new StoredProcedure
{
    Id = "resolver",
    Body = File.ReadAllText(@"resolver.js")
});
```

### <a id="create-custom-conflict-resolution-policy-stored-proc-java-async"></a>Java Async SDK’sı

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createCustomPolicy("resolver");
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

Kapsayıcı oluşturulduktan sonra oluşturmalısınız `resolver` saklı yordamı.

### <a id="create-custom-conflict-resolution-policy-stored-proc-java-sync"></a>Java Sync SDK’sı

```java
DocumentCollection udpCollection = new DocumentCollection();
udpCollection.setId(this.udpCollectionName);
ConflictResolutionPolicy udpPolicy = ConflictResolutionPolicy.createCustomPolicy(
        String.format("dbs/%s/colls/%s/sprocs/%s", this.databaseName, this.udpCollectionName, "resolver"));
udpCollection.setConflictResolutionPolicy(udpPolicy);
DocumentCollection createdCollection = this.tryCreateDocumentCollection(createClient, database, udpCollection);
```

Kapsayıcı oluşturulduktan sonra oluşturmalısınız `resolver` saklı yordamı.

### <a id="create-custom-conflict-resolution-policy-stored-proc-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const database = client.database(this.databaseName);
const { container: udpContainer } = await database.containers.createIfNotExists(
  {
    id: this.udpContainerName,
    conflictResolutionPolicy: {
      mode: "Custom",
      conflictResolutionProcedure: `dbs/${this.databaseName}/colls/${
        this.udpContainerName
      }/sprocs/resolver`
    }
  }
);
```

Kapsayıcı oluşturulduktan sonra oluşturmalısınız `resolver` saklı yordamı.

### <a id="create-custom-conflict-resolution-policy-stored-proc-python"></a>Python SDK’sı

```python
udp_collection = {
  'id': self.udp_collection_name,
  'conflictResolutionPolicy': {
      'mode': 'Custom',
      'conflictResolutionProcedure': 'dbs/' + self.database_name + "/colls/" + self.udp_collection_name + '/sprocs/resolver'
      }
  }
udp_collection = self.try_create_document_collection(create_client, database, udp_collection)
```

Kapsayıcı oluşturulduktan sonra oluşturmalısınız `resolver` saklı yordamı.


## <a name="create-a-custom-conflict-resolution-policy"></a>Özel bir çakışma çözümleme ilkesi oluşturma

Bu örnekler özel çakışma çözümleme ilkesine sahip bir kapsayıcı ayarlama adımlarını göstermektedir. Bu çakışmaları çakışması akıştaki gösterilir.

### <a id="create-custom-conflict-resolution-policy-dotnet"></a>.NET SDK

```csharp
DocumentCollection manualCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.manualCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.Custom,
      },
  });
```

### <a id="create-custom-conflict-resolution-policy-java-async"></a>Java Async SDK’sı

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createCustomPolicy();
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

### <a id="create-custom-conflict-resolution-policy-java-sync"></a>Java Sync SDK’sı

```java
DocumentCollection manualCollection = new DocumentCollection();
manualCollection.setId(this.manualCollectionName);
ConflictResolutionPolicy customPolicy = ConflictResolutionPolicy.createCustomPolicy(null);
manualCollection.setConflictResolutionPolicy(customPolicy);
DocumentCollection createdCollection = client.createCollection(database.getSelfLink(), collection, null).getResource();
```

### <a id="create-custom-conflict-resolution-policy-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const database = client.database(this.databaseName);
const {
  container: manualContainer
} = await database.containers.createIfNotExists({
  id: this.manualContainerName,
  conflictResolutionPolicy: {
    mode: "Custom"
  }
});
```

### <a id="create-custom-conflict-resolution-policy-python"></a>Python SDK’sı

```python
database = client.ReadDatabase("dbs/" + self.database_name)
manual_collection = {
                    'id': self.manual_collection_name,
                    'conflictResolutionPolicy': {
                          'mode': 'Custom'
                        }
                    }
manual_collection = client.CreateContainer(database['_self'], collection)
```

## <a name="read-from-conflict-feed"></a>Çakışma akışından okuma

Bu örnekler, kapsayıcının çakışma akışından okuma yöntemlerini göstermektedir. Yalnızca bunlar otomatik olarak çözümlenip olmayan veya özel çakışma İlkesi kullanarak akış çakışmayı çakışmaları gösterilir.

### <a id="read-from-conflict-feed-dotnet"></a>.NET SDK

```csharp
FeedResponse<Conflict> conflicts = await delClient.ReadConflictFeedAsync(this.collectionUri);
```

### <a id="read-from-conflict-feed-java-async"></a>Java Async SDK’sı

```java
FeedResponse<Conflict> response = client.readConflicts(this.manualCollectionUri, null)
                    .first().toBlocking().single();
for (Conflict conflict : response.getResults()) {
    /* Do something with conflict */
}
```

### <a id="read-from-conflict-feed-java-sync"></a>Java Sync SDK’sı

```java
Iterator<Conflict> conflictsIterator = client.readConflicts(this.collectionLink, null).getQueryIterator();
while (conflictsIterator.hasNext()) {
    Conflict conflict = conflictsIterator.next();
    /* Do something with conflict */
}
```

### <a id="read-from-conflict-feed-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const container = client
  .database(this.databaseName)
  .container(this.lwwContainerName);

const { result: conflicts } = await container.conflicts.readAll().toArray();
```

### <a id="read-from-conflict-feed-python"></a>Python

```python
conflicts_iterator = iter(client.ReadConflicts(self.manual_collection_link))
conflict = next(conflicts_iterator, None)
while conflict:
    # Do something with conflict
    conflict = next(conflicts_iterator, None)
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki Azure Cosmos DB kavramları hakkında bilgi edinin:

* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
* [Çok yöneticili uygulamalarınızda yapılandırma](how-to-multi-master.md)
* [Birden çok giriş için istemcileri yapılandırma](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Bölge ekleme veya Azure Cosmos DB hesabınızdan kaldırma](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Çok yöneticili uygulamalarınızda yapılandırma](how-to-multi-master.md).
* [Bölümleme ve veri dağıtımı](partition-data.md)
* [Azure Cosmos DB'yi dizine ekleme](indexing-policies.md)