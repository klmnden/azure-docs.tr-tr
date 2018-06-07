---
title: İzleme ve ölçümleri Azure Cosmos veritabanı ile hata ayıklama | Microsoft Docs
description: Ölçümleri Azure Cosmos DB'de ortak hatalarını ayıklamanıza ve veritabanını izlemek için kullanın.
keywords: metrics
services: cosmos-db
author: gnot
manager: kfile
editor: ''
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/25/2017
ms.author: govindk
ms.openlocfilehash: 49a381efa0603889336f43e409698bbcef44f41f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34615650"
---
# <a name="monitoring-and-debugging-with-metrics-in-azure-cosmos-db"></a>İzleme ve ölçümleri Azure Cosmos veritabanı ile hata ayıklama

Azure Cosmos DB verimlilik, depolama, tutarlılık, kullanılabilirlik ve gecikme ölçümleri sağlar. [Azure portal](https://portal.azure.com) bu ölçümleri daha ayrıntılı ölçümler, hem de istemci SDK'sı için; birleşik bir görünümünü sağlar ve [tanılama günlüklerini](./logging.md) kullanılabilir.

Yeni ölçümleri özetini almak ve Bul dinamik bölümleri veritabanınızdaki öğrenmek için aşağıdaki Azure Cuma videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Cosmos-DB-Get-the-Most-Out-of-Provisioned-Throughput/player]
> 

Bu makalede, ortak kullanım durumları ve analiz etmek ve bu sorunların hatalarını ayıklamak üzere Azure Cosmos DB ölçümleri'nın nasıl kullanılabileceğini aracılığıyla anlatılmaktadır. Ölçümleri beş dakikada bir toplanan ve yedi gün boyunca saklanır.

## <a name="understanding-how-many-requests-are-succeeding-or-causing-errors"></a>Kaç istek doğrulamanın başarılı olması veya hatalara neden anlama

Başlamak için head [Azure portal](https://portal.azure.com) gidin **ölçümleri** dikey. Dikey penceresinde Bul **istek sayısını 1 dakika başına kapasitesi aşıldı** grafik. Bu grafik durum kodu tarafından bölümlenmiş bir dakika tarafından dakika toplam istek sayısı gösterilir. HTTP durum kodları hakkında daha fazla bilgi için bkz: [Azure Cosmos DB için HTTP durum kodları](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb).

Azure Cosmos DB isteklerine sağlanan işleme aşan anlamına gelir (azaltma) 429 en sık karşılaşılan hata durum kodu var. Bunun en yaygın çözüm [RUs ölçeklendirme](./set-throughput.md) bir koleksiyon için.

![Dakika başına istek sayısı](media/use-metrics/metrics-12.png)

## <a name="determining-the-throughput-distribution-across-partitions"></a>Üretilen iş dağıtım bölümler belirleme

Bölüm anahtarlarının iyi kardinalitesi sahip, ölçeklenebilir bir uygulama için gereklidir. Bölümler tarafından ayrılmış herhangi bir bölümlenmiş koleksiyonu verimlilik dağıtımını belirlemek için gidin **ölçümleri dikey** içinde [Azure portal](https://portal.azure.com). İçinde **işleme** sekmesi, depolama dökümü görüntülenir **RU/saniye her fiziksel bir bölümü tarafından tüketilen en fazla** grafik. Aşağıdaki grafikte veri yi en solundaki çarpık bölüme göre olarak düşük bir dağıtım örneği gösterilmektedir. 

![Tek bir bölüm 3: 05'pm ağır kullanım görme](media/use-metrics/metrics-17.png)

Düzensiz verimlilik dağıtım neden olabilir *etkin* daraltılmış isteklerine neden ve genişletebilme gerektirebilir bölümler. Azure Cosmos DB'de bölümleme hakkında daha fazla bilgi için bkz: [bölüm ve Azure Cosmos veritabanı ölçek](./partition-data.md).

## <a name="determining-the-storage-distribution-across-partitions"></a>Depolama dağıtım bölümler belirleme

İyi kardinalitesi bölümünüzün sahip, ölçeklenebilir bir uygulama için gereklidir. Bölümler tarafından ayrılmış herhangi bir bölümlenmiş koleksiyonu verimlilik dağıtımını belirlemek için head ölçümleri dikey penceresinde için [Azure portal](https://portal.azure.com). Üretilen iş sekmesinde, depolama dökümü RU/saniye her fiziksel bölüm grafik tarafından tüketilen en fazla gösterilir. Aşağıdaki grafikte veri yi en solundaki çarpık bölüme göre olarak zayıf bir dağıtımını gösterilmektedir. 

![Zayıf veri dağıtım örneği](media/use-metrics/metrics-07.png)

Hangi bölüm anahtarı grafik bölümünde tıklayarak dağıtım eğriltme neden kökü olabilir. 

![Bölüm anahtarı dağıtım eğme](media/use-metrics/metrics-05.png)

Hangi bölüm anahtarı tanımlayan eğme dağıtımlarında neden olan sonra daha fazla dağıtılmış bir bölüm anahtarının koleksiyonunuzu bölümlendirmek gerekebilir. Azure Cosmos DB'de bölümleme hakkında daha fazla bilgi için bkz: [bölüm ve Azure Cosmos veritabanı ölçek](./partition-data.md).

## <a name="comparing-data-size-against-index-size"></a>Veri boyutu dizin boyutu karşı karşılaştırma

Azure Cosmos DB'de toplam harcanan depolama veri boyutu ve dizin boyutu birleşimidir. Genellikle, dizin boyutu bir veri boyutu bölümüdür. Ölçümleri dikey penceresinde de [Azure portal](https://portal.azure.com), depolama sekmesi veri ve dizin bağlı depolama alanı tüketimi dökümünü gösterir. Görüntü (belki de) alternatif olarak, SDK'dan gelen geçerli depolama kullanımı okuma koleksiyonu aracılığıyla bulabilirsiniz.
```csharp
// Measure the document size usage (which includes the index size)  
ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll")); 
 Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);
``` 
Dizin alanından tasarruf etmek istiyorsanız, ayarlayabileceğiniz [dizin oluşturma ilkesi](./indexing-policies.md).

## <a name="debugging-why-queries-are-running-slow"></a>Sorguları yavaş çalışıyor neden hata ayıklama

SQL API SDK Azure Cosmos DB sorgu yürütme istatistikleri sağlar. 

```csharp
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
 UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
 “SELECT * FROM c WHERE c.city = ‘Seattle’”, 
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

*QueryMetrics* ne kadar her bir bileşeninin sorgu yürütme için sürdü ayrıntıları sağlar. Uzun çalışan en yaygın kök nedeni sorgular ile daha iyi bir filtre koşulu çözümlenebilir (sorgu, dizinleri yararlanan alamadı) taramaları gerçekleşiyor.

## <a name="next-steps"></a>Sonraki adımlar

Artık nasıl izleneceği öğrendiğinize göre ve Azure portalında Metrikler kullanarak hata ayıklama sorunlarını, aşağıdaki makaleleri okuyarak veritabanı performansını iyileştirme hakkında daha fazla bilgi isteyebilirsiniz:

* [Performansı ve ölçeği Azure Cosmos DB ile test etme](performance-testing.md)
* [Azure Cosmos DB performans ipuçları](performance-tips.md)
