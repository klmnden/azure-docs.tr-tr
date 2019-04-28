---
title: Azure Cosmos DB'de çok ana yapılandırma
description: Azure Cosmos DB'de uygulamalarınızdaki çok ana yapılandırma hakkında bilgi edinin
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: rimman
ms.openlocfilehash: b862c59002369662d37b6d6a9de28370b0000497
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61054601"
---
# <a name="how-to-configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Uygulamalarınızda Azure Cosmos DB kullanmayı çok ana yapılandırma

Çok yöneticili özelliğini kullanmak için çok bölgeli yazma etkinleştirmek ve Azure Cosmos DB çok girişli özelliğini yapılandırmak gerekir. Çoklu yönlendirmeyi, uygulamanın dağıtıldığı bölge ayarlayarak yapılandırılır.

## <a id="netv2"></a>.NET SDK'sı v2

Çok yöneticili uygulamaları kümenizdeki etkinleştirmek için `UseMultipleWriteLocations` true ve yapılandırmak için `SetCurrentLocation` bölgeye uygulama dağıtılır ve Azure Cosmos DB çoğaltılır.

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

Çok yöneticili uygulamaları kümenizdeki etkinleştirmek için `policy.setUsingMultipleWriteLocations(true)` ve yapılandırma `policy.setPreferredLocations` bölgeye uygulama dağıtılır ve Cosmos DB çoğaltılır.

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

Ardından aşağıdaki makaleleri okuyabilirsiniz:

* [Azure Cosmos DB'deki tutarlılık yönetmek için oturum belirteçleri kullanma](how-to-manage-consistency.md#utilize-session-tokens)
* [Çakışma türlerini ve Azure Cosmos DB'de çözümleme ilkeleri](conflict-resolution-policies.md)
* [Azure Cosmos DB'de yüksek kullanılabilirlik](high-availability.md)
* [Azure Cosmos DB'deki tutarlılık düzeyleri](consistency-levels.md)
* [Azure Cosmos DB'de doğru tutarlılık düzeyi seçme](consistency-levels-choosing.md)
* [Azure Cosmos DB'deki tutarlılık, kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Genel olarak sağlanan aktarım hızı ölçeklendirme](scaling-throughput.md)
* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
