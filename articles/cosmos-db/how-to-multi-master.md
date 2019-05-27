---
title: Azure Cosmos DB'de çok ana yapılandırma
description: Azure Cosmos DB'de uygulamalarınızdaki çok yöneticili yapılandırmayı öğrenin.
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: rimman
ms.openlocfilehash: 1d9fa7380f62165d360888fd8cb03919f1736297
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244759"
---
# <a name="configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Uygulamalarınızda Azure Cosmos DB kullanmayı çok ana yapılandırma

Çok yöneticili özelliğini kullanmak için çok bölgeli yazma etkinleştirmek ve Azure Cosmos DB içinde birden çok girişli özelliği yapılandırmanız gerekir. Çoklu yönlendirmeyi yapılandırmak için uygulamanın dağıtıldığı bölge ayarlayın.

## <a id="netv2"></a>.NET SDK'sı v2

Uygulamanızda çok yöneticili etkinleştirmek için `UseMultipleWriteLocations` için `true`. Ayrıca, `SetCurrentLocation` bölgeye uygulama dağıtılır ve Azure Cosmos DB burada çoğaltılır:

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

Uygulamanızda çok yöneticili etkinleştirmek için `UseCurrentRegion` bölgeye uygulama dağıtılır ve Cosmos DB burada çoğaltılır:

```csharp
CosmosConfiguration config = new CosmosConfiguration("endpoint", "key");
config.UseCurrentRegion("West US");
CosmosClient client = new CosmosClient(config);
```

## <a id="java"></a>Java Async SDK’sı

Uygulamanızda çok yöneticili etkinleştirmek için `policy.setUsingMultipleWriteLocations(true)` ayarlayıp `policy.setPreferredLocations` bölgeye uygulama dağıtılır ve Cosmos DB burada çoğaltılır:

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

## <a id="javascript"></a>Node.js, JavaScript ve TypeScript SDK'ları

Uygulamanızda çok yöneticili etkinleştirmek için `connectionPolicy.UseMultipleWriteLocations` için `true`. Ayrıca, `connectionPolicy.PreferredLocations` bölgeye uygulama dağıtılır ve Cosmos DB burada çoğaltılır:

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

Uygulamanızda çok yöneticili etkinleştirmek için `connection_policy.UseMultipleWriteLocations` için `true`. Ayrıca, `connection_policy.PreferredLocations` bölgeye uygulama dağıtılır ve Cosmos DB burada çoğaltılır.

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleleri okuyun:

* [Azure Cosmos DB'deki tutarlılık yönetmek için oturum belirteçleri kullanma](how-to-manage-consistency.md#utilize-session-tokens)
* [Çakışma türlerini ve Azure Cosmos DB'de çözümleme ilkeleri](conflict-resolution-policies.md)
* [Azure Cosmos DB'de yüksek kullanılabilirlik](high-availability.md)
* [Azure Cosmos DB'deki tutarlılık düzeyleri](consistency-levels.md)
* [Azure Cosmos DB'de doğru tutarlılık düzeyi seçin](consistency-levels-choosing.md)
* [Azure Cosmos DB'deki tutarlılık, kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Genel olarak sağlanan aktarım hızı ölçeklendirme](scaling-throughput.md)
* [Genel dağıtım: Temel bileşenler](global-dist-under-the-hood.md)
