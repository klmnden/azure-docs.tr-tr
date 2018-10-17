---
title: İzleme ve Azure Cosmos DB'de ölçümlerle hata ayıklama | Microsoft Docs
description: Ölçümler, sık karşılaşılan sorunları hata ayıklama ve veritabanını izlemek için Azure Cosmos DB'de kullanın.
keywords: metrics
services: cosmos-db
author: kanshiG
manager: kfile
editor: ''
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/25/2017
ms.author: govindk
ms.openlocfilehash: 5c9dded95fe3ae36a716544368e3dc44c9b86afe
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365501"
---
# <a name="monitoring-and-debugging-with-metrics-in-azure-cosmos-db"></a>İzleme ve Azure Cosmos DB'de ölçümlerle hata ayıklama

Azure Cosmos DB, aktarım hızı, depolama, tutarlılık, kullanılabilirlik ve gecikme süresi için ölçümler sağlar. [Azure portalında](https://portal.azure.com) Bu ölçümler; daha ayrıntılı ölçümler, hem de istemci SDK'sı için toplu bir görünümünü sağlar ve [tanılama günlükleri](./logging.md) kullanılabilir.

Bu makalede, yaygın kullanım örnekleri ve Azure Cosmos DB ölçümleri analiz edin ve bu sorunların hatalarını ayıklamak için nasıl kullanılabileceği gösterilmektedir. Ölçümler, beş dakikada bir toplanan ve yedi gün boyunca saklanır.

## <a name="understanding-how-many-requests-are-succeeding-or-causing-errors"></a>Kaç istek doğrulamanın başarılı olması veya hatalara neden anlama

Başlamak için head [Azure portalında](https://portal.azure.com) gidin **ölçümleri** dikey penceresi. Dikey pencerede Bul **1 dakika başına kapasiteyi aşan istek sayısı** grafiği. Bu grafik, durum kodu ile bölümlenmiş bir dakika tarafından dakika toplam istek gösterir. HTTP durum kodları hakkında daha fazla bilgi için bkz. [Azure Cosmos DB için HTTP durum kodları](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb).

En yaygın hata durum kodu 429 (hız sınırlama/azaltma), istekleri Azure Cosmos DB'de sağlanan aktarım hızı aşan anlamına gelir. Bu en yaygın çözüm [RU'ları ölçeklendirme](./set-throughput.md) bir koleksiyon için.

![Dakika başına istek sayısı](media/use-metrics/metrics-12.png)

## <a name="determining-the-throughput-distribution-across-partitions"></a>Bölümler arasında aktarım hızı dağıtım belirleme

İyi bir bölüm anahtarlarınızı kardinalitesi sahip, ölçeklenebilir bir uygulama için gereklidir. Bölümler tarafından ayrılmış herhangi bir bölünmüş kapsayıcı aktarım hızı dağıtımını belirlemek için gidin **ölçümler dikey penceresine** içinde [Azure portalında](https://portal.azure.com). İçinde **aktarım hızı** sekmesi, depolama dökümü içinde gösterilmiştir **RU/saniye her bir fiziksel bölüm tarafından kullanılan en fazla** grafiği. Aşağıdaki grafikte, veri'yi en sol tarafındaki dengesiz bölümü tarafından olarak düşük bir dağıtım örneği gösterilmektedir. 

![Tek bölüm 3: 05'te yoğun kullanım görme](media/use-metrics/metrics-17.png)

Düzensiz aktarım hızı dağıtım neden *sık erişimli* daraltılmış istekler neden olabilir ve yeniden bölümlenmesi gerekebilir, bölümler. Azure Cosmos DB'de bölümleme hakkında daha fazla bilgi için bkz. [bölümleme ve ölçeklendirme Azure Cosmos DB'de](./partition-data.md).

## <a name="determining-the-storage-distribution-across-partitions"></a>Bölümler arasında depolama dağıtım belirleme

İyi bir kardinalite bölümünüzün sahip, ölçeklenebilir bir uygulama için gereklidir. Bölümler tarafından ayrılmış herhangi bir bölünmüş kapsayıcı aktarım hızı dağıtımını belirlemek için ölçümler dikey penceresine gidin [Azure portalında](https://portal.azure.com). Aktarım hızı sekmede RU/saniye her bir fiziksel bölüm grafik tarafından kullanılan en fazla depolama dökümü gösterilir. Aşağıdaki grafikte, veri'yi en sol tarafındaki dengesiz bölümü tarafından olarak düşük bir dağıtım gösterilmektedir. 

![Zayıf veri dağıtım örneği](media/use-metrics/metrics-07.png)

Hangi bölüm anahtarı bölüm grafikte tıklayarak dağıtım eğriltme neden kökü olabilir. 

![Bölüm anahtarı, dağıtım eğme](media/use-metrics/metrics-05.png)

Hangi bölüm anahtarı tanımlayan eğriltme dağıtımlarında neden olan sonra kapsayıcınızı daha fazla dağıtılmış bir bölüm anahtarıyla yeniden bölmek zorunda kalabilirsiniz. Azure Cosmos DB'de bölümleme hakkında daha fazla bilgi için bkz. [bölümleme ve ölçeklendirme Azure Cosmos DB'de](./partition-data.md).

## <a name="comparing-data-size-against-index-size"></a>Dizin boyutu karşı karşılaştırma veri boyutu

Azure Cosmos DB'de toplam tüketilen depolama alanı veri boyutu ve dizin boyutu birleşimidir. Genellikle, dizin boyutu bir veri boyutu bölümüdür. Ölçümler dikey penceresinde [Azure portalında](https://portal.azure.com), depolama sekmesi veri ve dizin göre depolama alanı tüketiminin dökümünü gösterir. 

```csharp
// Measure the document size usage (which includes the index size)  
ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll")); 
 Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);
``` 
Dizin alanından tasarruf etmek istiyorsanız, ayarlayabileceğiniz [dizin oluşturma ilkesi](./indexing-policies.md).

## <a name="debugging-why-queries-are-running-slow"></a>Yavaş çalışan sorguları neden hata ayıklama

SQL API'si SDK'ları Azure Cosmos DB sorgu yürütme istatistikleri sağlar. 

```csharp
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
 UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
 "SELECT * FROM c WHERE c.city = 'Seattle'", 
 new FeedOptions 
 { 
 PopulateQueryMetrics = true, 
 MaxItemCount = -1, 
 MaxDegreeOfParallelism = -1, 
 EnableCrossPartitionQuery = true 
 }).AsDocumentQuery();
FeedResponse<dynamic> result = await query.ExecuteNextAsync();

// Returns metrics by partition key range Id 
IReadOnlyDictionary<string, QueryMetrics> metrics = result.QueryMetrics;
```

*QueryMetrics* ne kadar sorgunun her bir bileşeni yürütmeyi sürdüğünü ayrıntılar sağlar. Uzun çalışan en yaygın kök nedeni ile daha iyi bir filtre koşulu çözümlenebilir (sorgu, dizinleri yararlanarak başaramadı) taramalar sorgulardır.

## <a name="next-steps"></a>Sonraki adımlar

Size nasıl izleneceği öğrendiğinize göre ve Azure portalında sağlanan ölçümler kullanarak hata ayıklama sorunlarını, aşağıdaki makaleleri okuyarak, veritabanı performansını iyileştirme hakkında daha fazla bilgi isteyebilirsiniz:

* [Performans ve ölçek testi ile Azure Cosmos DB](performance-testing.md)
* [Azure Cosmos DB için performans ipuçları](performance-tips.md)
