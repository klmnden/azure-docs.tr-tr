---
title: Azure Cosmos kapsayıcısı için benzersiz anahtarlar tanımlama
description: Azure Cosmos kapsayıcısı için benzersiz anahtarlar tanımlamayı öğrenin
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: sample
ms.date: 5/3/2019
ms.author: thweiss
ms.openlocfilehash: 1e8a88b640d588fbdb96089f11672b09590ee5d9
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65597732"
---
# <a name="define-unique-keys-for-an-azure-cosmos-container"></a>Azure Cosmos kapsayıcısı için benzersiz anahtarlar tanımlama

Bu makalede tanımlamak için farklı yollar sunar [benzersiz anahtarlar](unique-keys.md) bir Azure Cosmos kapsayıcı oluştururken. Bu, şu anda Azure portalını kullanarak ya da Sdk'lardan birini aracılığıyla bu işlemi gerçekleştirmek mümkündür.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturma](create-sql-api-dotnet.md#create-account) veya var olan bir hesabı seçin.

1. Açık **Veri Gezgini** bölmesi ve üzerinde çalışmak istediğiniz kapsayıcıyı seçin.

1. Tıklayarak **yeni kapsayıcı**.

1. İçinde **kapsayıcı Ekle** iletişim kutusunda, tıklayarak **+ Ekle benzersiz anahtar** benzersiz bir anahtar girişi eklemek için.

1. Benzersiz anahtar kısıtlamasının yolu girin

1. Gerekirse, tıklayarak daha fazla benzersiz anahtar girişi ekleyin **+ benzersiz anahtar Ekle**

![Benzersiz anahtar kısıtlaması girişinin Azure portalı ekran görüntüsü](./media/how-to-define-unique-keys/unique-keys-portal.png)

## <a name="use-the-net-sdk-v2"></a>.NET SDK'sı V2 kullanın

Kullanarak yeni bir kapsayıcı oluştururken [.NET SDK'sı v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/), `UniqueKeyPolicy` nesne benzersiz anahtar kısıtlamaları tanımlamak için kullanılabilir.

```csharp
client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("database"), new DocumentCollection
{
    Id = "container",
    UniqueKeyPolicy = new UniqueKeyPolicy
    {
        UniqueKeys = new Collection<UniqueKey>(new List<UniqueKey>
        {
            new UniqueKey { Paths = new Collection<string>(new List<string> { "/firstName", "/lastName", "emailAddress" }) },
            new UniqueKey { Paths = new Collection<string>(new List<string> { "/address/zipCode" }) }
        })
    }
});
```

## <a name="use-the-java-sdk"></a>Java kullanma SDK'sı

Kullanarak yeni bir kapsayıcı oluştururken [Java SDK'sı](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb), `UniqueKeyPolicy` nesne benzersiz anahtar kısıtlamaları tanımlamak için kullanılabilir.

```java
// create a new DocumentCollection object
DocumentCollection container = new DocumentCollection();
container.setId("container");
// create array of strings and populate them with the unique key paths
Collection<String> uniqueKey1Paths = new ArrayList<String>();
uniqueKey1Paths.add("/firstName");
uniqueKey1Paths.add("/lastName");
uniqueKey1Paths.add("/emailAddress");
Collection<String> uniqueKey2Paths = new ArrayList<String>();
uniqueKey2Paths.add("/address/zipCode");
// create UniqueKey objects and set their paths
UniqueKey uniqueKey1 = new UniqueKey();
UniqueKey uniqueKey2 = new UniqueKey();
uniqueKey1.setPaths(uniqueKey1Paths);
uniqueKey2.setPaths(uniqueKey2Paths);
// create a new UniqueKeyPolicy object and set its unique keys
UniqueKeyPolicy uniqueKeyPolicy = new UniqueKeyPolicy();
Collection<UniqueKey> uniqueKeys = new ArrayList<UniqueKey>();
uniqueKeys.add(uniqueKey1);
uniqueKeys.add(uniqueKey2);
uniqueKeyPolicy.setUniqueKeys(uniqueKeys);
// set the unique key policy
container.setUniqueKeyPolicy(uniqueKeyPolicy);
// create the container
client.createCollection(String.format("/dbs/%s", "database"), container, null);
```

## <a name="use-the-nodejs-sdk"></a>Node.js SDK'sını kullanma

Kullanarak yeni bir kapsayıcı oluştururken [Node.js SDK'sı](https://www.npmjs.com/package/@azure/cosmos), `UniqueKeyPolicy` nesne benzersiz anahtar kısıtlamaları tanımlamak için kullanılabilir.

```javascript
client.database('database').containers.create({
    id: 'container',
    uniqueKeyPolicy: {
        uniqueKeys: [
            { paths: ['/firstName', '/lastName', '/emailAddress'] },
            { paths: ['/address/zipCode'] }
        ]
    }
});
```

## <a name="use-the-python-sdk"></a>Python SDK'sını kullanma

Kullanarak yeni bir kapsayıcı oluştururken [Python SDK'sı](https://pypi.org/project/azure-cosmos/), benzersiz anahtar kısıtlamaları, parametre olarak geçirilen sözlük bir parçası olarak belirtilebilir.

```python
client.CreateContainer('dbs/' + config['DATABASE'], {
    'id': 'container',
    'uniqueKeyPolicy': {
        'uniqueKeys': [
            { 'paths': ['/firstName', '/lastName', '/emailAddress'] },
            { 'paths': ['/address/zipCode'] }
        ]
    }
})
```

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [bölümleme](partition-data.md)
- Keşfedin [dizin oluşturma nasıl çalışır](index-overview.md)