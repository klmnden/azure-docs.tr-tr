---
title: Azure Cosmos DB'de kapsayıcıları sorgulama
description: Azure Cosmos DB'de kapsayıcıları sorgulamayı öğrenin
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 5d64aa8b50cdde23d1bb8980510cfac202204f9a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262463"
---
# <a name="query-containers-in-azure-cosmos-db"></a>Azure Cosmos DB'de kapsayıcıları sorgulama

Bu makalede bir Azure Cosmos DB’deki kapsayıcının (koleksiyon, grafik, tablo) nasıl sorgulanacağı açıklanır.

## <a name="in-partition-query"></a>Bölüm içi sorgu

Kapsayıcılardaki verileri sorguladığınızda, Azure Cosmos DB sorguyu otomatik olarak filtrede belirtilen bölüm anahtarı değerlerine (varsa) karşılık gelen bölümlere yönlendirir. Örneğin, bu sorgu doğrudan "XMS-0001" bölüm anahtarını içeren bölüme yönlendirilir.

```csharp
// Query using partition key into a class called, DeviceReading
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```

## <a name="cross-partition-query"></a>Bölümler arası sorgu

Aşağıdaki sorgunun bölüm anahtarında (DeviceId) filtresi yoktur ve bölümün dizinine göre yürütüldüğü tüm bölümleri yayılır. Bölümler arasında sorgu yürütmek için, **EnableCrossPartitionQuery** değerini true olarak ayarlayın (veya REST API'de x-ms-documentdb-query-enablecrosspartition).

```csharp
// Query across partition keys into a class called, DeviceReading
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

Cosmos DB, SQL kullanılarak kapsayıcılar üzerinde toplama işlevlerini (COUNT, MIN, MAX, ve AVG) destekler. SDK sürüm 1.12.0'dan başlayarak kapsayıcılar üzerinde toplama işlevleri. Sorguların tek bir toplama işleci ve izdüşümde tek bir değer içermesi gerekir.

## <a name="parallel-cross-partition-query"></a>Paralel bölümler arası sorgu

Sürümü 1.9.0 ve üstü olan Cosmos DB SDK'ları, paralel sorgu yürütme seçeneklerini destekler.  Paralel bölümler arası sorgular, düşük gecikme süreli bölümler arası sorgular yürütmenize olanak tanır. Örneğin, aşağıdaki sorgu bölümler arasında paralel çalıştırılacak şekilde yapılandırılmıştır.

```csharp
// Cross-partition Order By Query with parallel execution
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),  
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```

Aşağıdaki parametreleri ayarlayarak paralel sorgu yürütme işlemini yönetebilirsiniz:

- **MaxDegreeOfParallelism**: Kapsayıcının bölümlerine yönelik eşzamanlı ağ bağlantıları sayısı üst sınırını ayarlar. Bu özelliği -1 olarak ayarlarsanız, paralellik derecesi SDK tarafından yönetilir. MaxDegreeOfParallelism belirtilmezse veya 0 olarak ayarlanırsa, kapsayıcının bölümlerine tek ağ bağlantısı kurulur.

- **MaxBufferedItemCount**: Sorgu gecikme süresiyle istemci tarafı bellek kullanımı arasında denge kurar. Bu seçenek atlanırsa veya -1 olarak ayarlanırsa, paralel sorgu yürütme işlemi sırasında arabelleğe alınan öğelerin sayısı SDK tarafından yönetilir.

Koleksiyonun durumu aynı olduğunda, paralel sorgu sonuçları seri yürütme ile aynı sırada döndürür. Sıralama işleçleri (ORDER BY ve/veya TOP) içeren bir bölümler arası sorgu gerçekleştirirken, Azure Cosmos DB SDK'sı sorguyu bölümler arasında paralel olarak gönderir ve genel olarak sıralanmış sonuçları oluşturmak için sunucu tarafında kısmen sıralanmış sonuçları birleştirir.

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB'de bölümleme hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
- [Azure Cosmos DB'de yapay bölümleme anahtarları](synthetic-partition-keys.md)
