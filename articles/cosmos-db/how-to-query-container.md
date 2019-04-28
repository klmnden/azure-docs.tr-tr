---
title: Azure Cosmos DB'de kapsayıcıları sorgulama
description: Azure Cosmos DB'de kapsayıcıları sorgulamayı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 2ea228a1db204170f947b5fe71f1865a4620b0f4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61053961"
---
# <a name="query-an-azure-cosmos-container"></a>Sorgu bir Azure Cosmos kapsayıcısı

Bu makalede, Azure Cosmos DB'de bir kapsayıcı (koleksiyon, graf veya tablo) sorgu açıklanmaktadır.

## <a name="in-partition-query"></a>Bölüm içi sorgu

Belirtilen bir bölüm anahtarı filtresi sorgu varsa, kapsayıcılar, verileri sorguladığınızda, Azure Cosmos DB sorguyu otomatik olarak işler. Sorgu, filtrede belirtilen bölüm anahtarı değerlerine karşılık gelen bölümlere yönlendirir. Aşağıdaki sorgu gibi yönlendirilir `DeviceId` bölüm anahtarı değerine karşılık gelen tüm belgelerin tutan bölüm `XMS-0001`.

```csharp
// Query using partition key into a class called, DeviceReading
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```

## <a name="cross-partition-query"></a>Bölümler arası sorgu

Aşağıdaki sorgunun bölüm anahtarına göre bir filtre yok (`DeviceId`) ve onu çalıştırdığı bölümün dizinine göre tüm bölümleri için yayılır. Bölümler arasında sorgu çalıştırmak için ayarlanmış `EnableCrossPartitionQuery` true (veya `x-ms-documentdb-query-enablecrosspartition`  REST API'de).

EnableCrossPartitionQuery özelliği bir Boole değeri kabul eder. Ayarlandığında true ve sorgunuzu bir bölüm anahtarı yoksa, Azure Cosmos DB bölümler arasında sorguyu yayar. Tüm bölümler için ayrı ayrı sorgular göndererek yayma gerçekleştirilir. Sorgu sonuçları okumak için istemci uygulamaları FeedResponse sonuçlardan kullanma ve ContinuationToken özelliğini denetleyin. Tüm sonuçları okumak için ContinuationToken null olana kadar veriler üzerinde yineleme tutun. 

```csharp
// Query across partition keys into a class called, DeviceReading
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

Azure Cosmos DB SQL kullanarak kapsayıcıları üzerinde toplama işlevleri sayısı, MIN, Maks ve ortalama destekler. Kapsayıcıları başlayıp 1.12.0 SDK sürümü ve üzeri üzerinde toplama işlevleri. Sorgular tek bir toplama işleci içermelidir ve projeksiyon tek bir değer içermelidir.

## <a name="parallel-cross-partition-query"></a>Paralel bölümler arası sorgu

Azure Cosmos DB SDK 1.9.0 ve üstü destek paralel sorgu yürütme seçenekleri. Paralel bölümler arası sorgular, düşük gecikme süreli bölümler arası sorgular yürütmenize olanak tanır. Örneğin, aşağıdaki sorgu bölümler arasında paralel çalıştırılacak şekilde yapılandırılmıştır.

```csharp
// Cross-partition Order By Query with parallel execution
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),  
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```

Aşağıdaki parametreleri ayarlayarak paralel sorgu yürütme işlemini yönetebilirsiniz:

- **Maxanalyticsunits**: Kapsayıcının bölümleri için en fazla eşzamanlı ağ bağlantı sayısını ayarlar. Bu özelliği -1 olarak ayarlarsanız, paralellik derecesi SDK yönetir. Varsa `MaxDegreeOfParallelism` varsayılan değer, belirtilen veya ayarlanmış 0 değil, kapsayıcının bölümlerine tek ağ bağlantısı.

- **MaxBufferedItemCount**: Gecikme süresi ile istemci tarafı bellek kullanımı başardı sorgulayın. Bu seçeneği atlanırsa veya -1 olarak ayarlamak için paralel sorgu yürütme işlemi sırasında arabelleğe alınan öğelerin sayısı SDK'yı yöneten olur.

Koleksiyon aynı durumuyla paralel sorgu sonuçları seri yürütme ile aynı sırada döndürür. Sıralama (ORDER BY arama, üst) işleçleri içeren bir bölümler arası sorgu gerçekleştirirken, Azure Cosmos DB SDK'sı bölümler arasında paralel sorgu verir. Bu, genel olarak sıralanmış sonuçları oluşturmak için sunucu tarafında kısmen sıralanmış sonuçları birleştirir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de bölümleme hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
- [Azure Cosmos DB'de yapay bölümleme anahtarları](synthetic-partition-keys.md)
