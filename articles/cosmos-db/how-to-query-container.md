---
title: Azure Cosmos DB'de kapsayıcıları sorgulama
description: Azure Cosmos DB'de kapsayıcıları sorgulamayı öğrenin
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 11c68b61802f6c7b3755da71c176ea777f171e4c
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409845"
---
# <a name="query-containers-in-azure-cosmos-db"></a>Azure Cosmos DB'de kapsayıcıları sorgulama

Bu makalede bir Azure Cosmos DB’deki kapsayıcının (koleksiyon, grafik, tablo) nasıl sorgulanacağı açıklanır.

## <a name="in-partition-query"></a>Bölüm içi sorgu

Belirtilen bir bölüm anahtarı filtresi sorgu varsa, kapsayıcılar, verileri sorguladığınızda, Azure Cosmos DB sorguyu otomatik olarak filtrede belirtilen bölüm anahtarı değerlerine karşılık gelen bölümlere yönlendirir. Örneğin, aşağıdaki sorgu, bölüm anahtarı değeri "XMS-0001" karşılık gelen tüm belgelerin tutan DeviceID bölüme yönlendirilir.

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

- **Maxanalyticsunits**: Kapsayıcının bölümleri için en fazla eşzamanlı ağ bağlantı sayısını ayarlar. Bu özelliği -1 olarak ayarlarsanız, paralellik derecesi SDK tarafından yönetilir. MaxDegreeOfParallelism belirtilmezse veya 0 olarak ayarlanırsa, kapsayıcının bölümlerine tek ağ bağlantısı kurulur.

- **MaxBufferedItemCount**: Gecikme süresi ile istemci tarafı bellek kullanımı başardı sorgulayın. Bu seçenek atlanırsa veya -1 olarak ayarlanırsa, paralel sorgu yürütme işlemi sırasında arabelleğe alınan öğelerin sayısı SDK tarafından yönetilir.

Koleksiyonun durumu aynı olduğunda, paralel sorgu sonuçları seri yürütme ile aynı sırada döndürür. Sıralama işleçleri (ORDER BY ve/veya TOP) içeren bir bölümler arası sorgu gerçekleştirirken, Azure Cosmos DB SDK'sı sorguyu bölümler arasında paralel olarak gönderir ve genel olarak sıralanmış sonuçları oluşturmak için sunucu tarafında kısmen sıralanmış sonuçları birleştirir.

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB'de bölümleme hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
- [Azure Cosmos DB'de yapay bölümleme anahtarları](synthetic-partition-keys.md)
