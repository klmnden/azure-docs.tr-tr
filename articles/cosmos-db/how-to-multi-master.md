---
title: Azure Cosmos DB'de çok ana yapılandırma
description: Azure Cosmos DB'de uygulamalarınızdaki çok ana yapılandırma hakkında bilgi edinin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 2/12/2019
ms.author: mjbrown
ms.openlocfilehash: effe6fa942ce0cabace08e72dba90baf8646680e
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56119289"
---
# <a name="how-to-configure-multi-master-in-your-applications-in-azure-cosmos-db"></a>Azure Cosmos DB'de uygulamalarınızdaki çok ana yapılandırma

Uygulamanızda çok bölgeli yazma etkinleştirmek ve uygulamanın geçerli bölge ayarlayarak birden çok girişe atanması özelliğini yapılandırmak için ihtiyaç duyduğunuz uygulamalarınızdaki çok yöneticili özelliklerini kullanmayı içinde dağıtılır.

## <a id="netv2"></a>.NET SDK'sı v2

Çok yöneticili uygulamaları kümenizdeki etkinleştirmek için `UseMultipleWriteLocations` true ve yapılandırmak için `SetCurrentLocation` bölgeye uygulama dağıtılır ve Cosmos DB çoğaltılır.

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");
```

## <a id="netv3"></a>.NET SDK'sı v3 (Önizleme)

Uygulamalarınızdaki çok yöneticili etkinleştirmek için yapılandırma `UseCurrentRegion` bölgeye uygulama dağıtılır ve Cosmos DB çoğaltılır.

```csharp
CosmosConfiguration config = new CosmosConfiguration("endpoint", "key");
config.UseCurrentRegion("West US");
CosmosClient client = new CosmosClient(config);
```

## <a id="java"></a>Java Async SDK’sı

Çok yöneticili uygulamaları kümenizdeki etkinleştirmek için `policy.setUsingMultipleWriteLocations(true)` true ve yapılandırmak için `policy.setPreferredLocations` bölgeye uygulama dağıtılır ve Cosmos DB çoğaltılır.

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Collections.singletonList(region));

AsyncDocumentClient client =
    new AsyncDocumentClient.Builder()
        .withMasterKeyOrResourceToken(this.accountKey)
        .withServiceEndpoint(this.accountEndpoint)
        .withConsistencyLevel(ConsistencyLevel.Eventual)
        .withConnectionPolicy(policy).build();
```

## <a id="javascript"></a>Node.js, JavaScript, TypeScript SDK'sı

Çok yöneticili uygulamaları kümenizdeki etkinleştirmek için `connectionPolicy.UseMultipleWriteLocations` true ve yapılandırmak için `connectionPolicy.PreferredLocations` bölgeye uygulama dağıtılır ve Cosmos DB çoğaltılır.

```javascript
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.UseMultipleWriteLocations = true;
connectionPolicy.PreferredLocations = [region];

const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy,
  consistencyLevel: ConsistencyLevel.Eventual
});
```

## <a id="python"></a>Python SDK’sı

Çok yöneticili uygulamaları kümenizdeki etkinleştirmek için `connection_policy.UseMultipleWriteLocations` true ve yapılandırmak için `connection_policy.PreferredLocations` bölgeye uygulama dağıtılır ve Cosmos DB çoğaltılır.

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="next-steps"></a>Sonraki adımlar

Çok yöneticili, genel dağıtım ve Azure Cosmos DB tutarlılık hakkında daha fazla bilgi edinin. Aşağıdaki makalelere bakın:

* [Azure Cosmos DB'deki tutarlılık yönetmek için oturum belirteçleri kullanma](how-to-manage-consistency.md#utilize-session-tokens)

* [Çakışma türlerini ve Azure Cosmos DB'de çözümleme ilkeleri](conflict-resolution-policies.md)

* [Azure Cosmos DB'de yüksek kullanılabilirlik](high-availability.md)

* [Azure Cosmos DB'de doğru tutarlılık düzeyi seçme](consistency-levels-choosing.md)

* [Azure Cosmos DB'deki tutarlılık, kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
