---
title: Azure Cosmos DB bölgeleri arasındaki çakışmaları yönetme
description: Azure Cosmos DB'deki çakışmaları yönetme
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: rimman
ms.openlocfilehash: 6f4e6fce88815490ceffd7456cde045b38e32b29
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680222"
---
# <a name="manage-conflict-resolution-policies-in-azure-cosmos-db"></a>Azure Cosmos DB'de Çakışma çözümlemesi ilkelerini yönetme

Birden çok istemci aynı öğeye yazdığında, çok bölgeli yazma ile çakışmaları oluşabilir. Bir çakışma oluştuğunda, farklı çakışma çözümlemesi ilkeleri kullanarak çakışmayı çözebilirsiniz. Bu makalede, Çakışma çözümlemesi ilkelerini yönetmek açıklar.

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

## <a name="create-a-custom-conflict-resolution-policy-using-a-stored-procedure"></a>Bir saklı yordamı kullanarak bir özel çakışma çözüm ilkesi oluşturma

Bu örnekler, bir özel çakışma çözüm İlkesi çakışmayı çözmek için bir saklı yordamı kullanarak bir kapsayıcı ayarlama gösterir. Bu çakışmaları, saklı yordamda hata olmadığı sürece akış çakışma görünmüyor.

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
```

Kapsayıcı oluşturulduktan sonra oluşturmalısınız `resolver` saklı yordamı.

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

## <a name="create-a-last-writer-wins-lww-conflict-resolution-policy"></a>Son yazıcı WINS (LWW) çakışma çözüm ilkesi oluşturma

Bu örnekler, son yazıcı WINS çakışma çözüm ilkesi içeren bir kapsayıcıya nasıl gösterir. Yolu ayarlanmamış veya geçersiz ise varsayılan `_ts` özelliği. Bu özellik, zaman damgası alanı olur. Bu çakışmaları çakışması akıştaki gösterilmez.

### <a id="create-custom-conflict-resolution-policy-lww-dotnet"></a>.NET SDK

```csharp
DocumentCollection lwwCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.lwwCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.LastWriterWins,
          ConflictResolutionPath = "/regionId",
      },
  });
```

### <a id="create-custom-conflict-resolution-policy-lww-java-async"></a>Java Async SDK’sı

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createLastWriterWinsPolicy(conflictResolutionPath);
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

### <a id="create-custom-conflict-resolution-policy-lww-java-sync"></a>Java Sync SDK’sı

```java
DocumentCollection lwwCollection = new DocumentCollection();
lwwCollection.setId(this.lwwCollectionName);
ConflictResolutionPolicy lwwPolicy = ConflictResolutionPolicy.createLastWriterWinsPolicy("/regionId");
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
      conflictResolutionPath: "/regionId"
    }
  }
);
```

Seçeneğini kullanmazsanız `conflictResolutionPath` özelliği varsayılan olarak için `_ts` özelliği.

### <a id="create-custom-conflict-resolution-policy-lww-python"></a>Python SDK’sı

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

## <a name="read-from-conflict-feed"></a>Çakışma akışından okuma

Bu örnekler, kapsayıcının çakışma akışından okuma yöntemlerini göstermektedir. Çakışmaları yalnızca akış çakışmayı bunlar otomatik olarak, örn, el ile çakışma çözüm ilkesi seçtiğiniz ve saklı yordam kaydetmedi çözümlenemedi. varsa gösterilir.

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