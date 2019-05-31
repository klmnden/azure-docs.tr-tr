---
title: İzleyici ve Azure Cosmos DB'de ölçümlerle hata ayıklama
description: Ölçümler, sık karşılaşılan sorunları hata ayıklama ve veritabanını izlemek için Azure Cosmos DB'de kullanın.
ms.service: cosmos-db
author: kanshiG
ms.author: sngun
ms.topic: conceptual
ms.date: 05/23/2019
ms.reviewer: sngun
ms.openlocfilehash: b7633b75bbb6d37c68a562560a6459e35d03b810
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242549"
---
# <a name="monitor-and-debug-with-metrics-in-azure-cosmos-db"></a>İzleyici ve Azure Cosmos DB'de ölçümlerle hata ayıklama

Azure Cosmos DB, aktarım hızı, depolama, tutarlılık, kullanılabilirlik ve gecikme süresi için ölçümler sağlar. [Azure portalında](https://portal.azure.com) Bu ölçümler toplu bir görünümünü sağlar. Daha ayrıntılı ölçümler, hem de istemci SDK'sı ve [tanılama günlükleri](./logging.md) kullanılabilir.

Bu makalede, yaygın kullanım örnekleri ve Azure Cosmos DB ölçümleri analiz edin ve bu sorunların hatalarını ayıklamak için nasıl kullanılabileceği gösterilmektedir. Ölçümler, beş dakikada bir toplanan ve yedi gün boyunca tutulur.

## <a name="understand-how-many-requests-are-succeeding-or-causing-errors"></a>Kaç istek doğrulamanın başarılı olması veya hatalara neden anlama

Başlamak için head [Azure portalında](https://portal.azure.com) gidin **ölçümleri** dikey penceresi. Dikey pencerede Bul **1 dakika başına kapasiteyi aşan istek sayısı** grafiği. Bu grafik, durum kodu ile bölümlenmiş bir dakika tarafından dakika toplam istek gösterir. HTTP durum kodları hakkında daha fazla bilgi için bkz. [Azure Cosmos DB için HTTP durum kodları](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb).

En yaygın hata durum kodu 429 (hızı sınırlama/azaltma) ' dir. Bu hata Azure Cosmos DB'de istekleri birden çok sağlanan aktarım hızı anlamına gelir. Bu sorunun en yaygın çözüm [RU'ları ölçeklendirme](./set-throughput.md) bir koleksiyon için.

![Dakika başına istek sayısı](media/use-metrics/metrics-12.png)

## <a name="determine-the-throughput-distribution-across-partitions"></a>Bölümler arasında aktarım hızı dağıtım belirleme

İyi bir bölüm anahtarlarınızı kardinalitesi sahip, ölçeklenebilir bir uygulama için gereklidir. Bölümler tarafından ayrılmış herhangi bir bölünmüş kapsayıcı aktarım hızı dağıtımını belirlemek için gidin **ölçümler dikey penceresine** içinde [Azure portalında](https://portal.azure.com). İçinde **aktarım hızı** sekmesi, depolama dökümü içinde gösterilmiştir **RU/saniye her bir fiziksel bölüm tarafından kullanılan en fazla** grafiği. Aşağıdaki grafikte, en sol tarafındaki dengesiz bölümü tarafından gösterilen veri düşük bir dağıtım örneği gösterilmektedir.

![Tek bölüm 3: 05'te yoğun kullanım görme](media/use-metrics/metrics-17.png)

Düzensiz aktarım hızı dağıtım neden *sık erişimli* daraltılmış istekler neden olabilir ve yeniden bölümlenmesi gerekebilir, bölümler. Azure Cosmos DB'de bölümleme hakkında daha fazla bilgi için bkz. [bölümleme ve ölçeklendirme Azure Cosmos DB'de](./partition-data.md).

## <a name="determine-the-storage-distribution-across-partitions"></a>Bölümler arasında depolama dağıtım belirleme

İyi bir kardinalite bölümünüzün sahip, ölçeklenebilir bir uygulama için gereklidir. Bölümler tarafından ayrılmış herhangi bir bölünmüş kapsayıcı depolama dağıtımını belirlemek için ölçümler dikey penceresine gidin [Azure portalında](https://portal.azure.com). Depolama sekmede verilerdeki depolama dökümü gösterilen + dizin üst bölüm anahtarları grafik tarafından kullanılan depolama miktarı. Aşağıdaki grafikte en sol tarafındaki dengesiz bölüm gösterildiği gibi veri depolama, düşük bir dağıtım gösterilmektedir.

![Zayıf veri dağıtım örneği](media/use-metrics/metrics-07.png)

Hangi bölüm anahtarı bölüm grafikte tıklayarak dağıtım eğriltme neden kökü olabilir.

![Bölüm anahtarı, dağıtım eğme](media/use-metrics/metrics-05.png)

Hangi bölüm anahtarı tanımlayan eğriltme dağıtımlarında neden olan sonra kapsayıcınızı daha fazla dağıtılmış bir bölüm anahtarıyla yeniden bölmek zorunda kalabilirsiniz. Azure Cosmos DB'de bölümleme hakkında daha fazla bilgi için bkz. [bölümleme ve ölçeklendirme Azure Cosmos DB'de](./partition-data.md).

## <a name="compare-data-size-against-index-size"></a>Dizin boyutu karşı veri boyutunu karşılaştırın

Azure Cosmos DB'de toplam tüketilen depolama alanı veri boyutu ve dizin boyutu birleşimidir. Genellikle, dizin boyutu bir veri boyutu bölümüdür. Ölçümler dikey penceresinde [Azure portalında](https://portal.azure.com), depolama sekmesi veri ve dizin göre depolama alanı tüketiminin dökümünü gösterir.

```csharp
// Measure the document size usage (which includes the index size)  
ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
 Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);
```

Dizin alanından tasarruf etmek istiyorsanız, ayarlayabileceğiniz [dizin oluşturma ilkesi](index-policy.md).

## <a name="debug-why-queries-are-running-slow"></a>Hata ayıklama neden sorguların yavaş çalışıyor

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

*QueryMetrics* ne kadar sorgunun her bir bileşeni yürütmeyi sürdüğünü ayrıntılar sağlar. En yaygın kök nedeni taramalar uzun sorguları çalıştırmak için sorgu anlamına gelir, dizinleri yararlanarak başaramadı. Daha iyi bir filtre koşulu ile bu sorun çözülebilir.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi, izleme ve Azure portalında sağlanan ölçümler kullanarak hata ayıklama sorunlarını öğrendiniz. Aşağıdaki makaleleri okuyarak, veritabanı performansını iyileştirme hakkında daha fazla bilgi isteyebilirsiniz:

* [Performans ve ölçek testi ile Azure Cosmos DB](performance-testing.md)
* [Azure Cosmos DB için performans ipuçları](performance-tips.md)
