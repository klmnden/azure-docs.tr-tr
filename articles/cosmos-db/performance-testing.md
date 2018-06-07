---
title: Azure Cosmos DB ölçek ve performans testi | Microsoft Docs
description: Ölçek ve performans ile Azure Cosmos DB testi gerçekleştirmek öğrenin
keywords: Performans testi
services: cosmos-db
author: SnehaGunda
manager: kfile
editor: ''
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 08/29/2017
ms.author: sngun
ms.openlocfilehash: ce2c0ddcce3813990bf819477f7db425d70e3595
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34612308"
---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a>Performansı ve ölçeği Azure Cosmos DB ile test etme

Performans ve ölçek testi adımdır bir anahtar uygulama geliştirme. Birçok uygulama için veritabanı katmanı genel performans ve ölçeklenebilirlik üzerinde önemli bir etkiye sahiptir. Bu nedenle performans testi için kritik bir bileşen olur. [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) esnek ölçek ve tahmin edilebilir performans için amaca değil. Bu özellikler bir yüksek performanslı veritabanı katmanı gereken uygulamalar için mükemmel bir sığdırma kolaylaştırır. 

Bu makalede kendi Azure Cosmos DB iş yükleri için performansı test paketleri uygulama geliştiricileri için başvuru değildir. Ayrıca, yüksek performanslı uygulama senaryoları için Azure Cosmos DB değerlendirmek için de kullanılabilir. Yalıtılmış performans veritabanını öncelikle sınama odaklanır, ancak aynı zamanda üretim uygulamaları için en iyi yöntemleri içerir.

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlayın mümkün olacaktır: 

* Azure Cosmos DB performans testi için bir örnek .NET istemci uygulaması nereden bulabilirim? 
* My istemci uygulamasından nasıl Azure Cosmos DB ile yüksek işleme düzeyleri elde?

Kodu ile çalışmaya başlamak için projesinden indirmeniz [Azure Cosmos DB performans örnek testi](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

> [!NOTE]
> Bu uygulama amacı, az sayıda istemci makineleri ile Azure Cosmos DB'den en iyi performansı elde etme göstermektir. Örnek (hangi herhangi bir sınır ölçeklendirebilirsiniz) Azure Cosmos DB en yüksek işleme kapasitesi değil elde etmek için hedefidir.
> 
> 

Azure Cosmos DB performansını artırmak istemci tarafı yapılandırma seçenekleri arıyorsanız bkz [Azure Cosmos DB performans ipuçları](performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Performans uygulama testi çalıştırma
Başlamak için en hızlı derlemek ve .NET örneği çalıştırmak için aşağıdaki adımlarda açıklandığı gibi yoludur. Ayrıca, kaynak kodu gözden geçirin ve benzer yapılandırmaları üzerinde kendi istemci uygulamalarını uygulamak.

**1. adım:** projesinden indirmeniz [Azure Cosmos DB performans örnek testi](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), veya GitHub depoyu çatallaştırmanız.

**2. adım:** EndpointUrl ve AuthorizationKey, CollectionThroughput ve DocumentTemplate (App.config dosyasında isteğe bağlı) ayarlarını değiştirin.

> [!NOTE]
> Yüksek verimlilik koleksiyonlarla sağlamadan önce başvurmak [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/) koleksiyon başına maliyetleri tahmin etme. Azure Cosmos DB faturaları depolama ve işleme saatlik temelinde bağımsız olarak. Silerek veya test sonra Azure Cosmos DB koleksiyonlarınızı verimini azaltmayı maliyetleri kaydedebilirsiniz.
> 
> 

**3. adım:** derleyin ve komut satırından konsol uygulamasını çalıştırın. Aşağıdakine benzer bir çıktı görmeniz gerekir:

    C:\Users\cosmosdb\Desktop\Benchmark>DocumentDBBenchmark.exe
    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://arramacquerymetrics.documents.azure.com:443/
    Collection : db.data at 100000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: -1
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Found collection data with 100000 RU/s
    Starting Inserts with 100 tasks
    Inserted 4503 docs @ 4491 writes/s, 47070 RU/s (122B max monthly 1KB reads)
    Inserted 17910 docs @ 8862 writes/s, 92878 RU/s (241B max monthly 1KB reads)
    Inserted 32339 docs @ 10531 writes/s, 110366 RU/s (286B max monthly 1KB reads)
    Inserted 47848 docs @ 11675 writes/s, 122357 RU/s (317B max monthly 1KB reads)
    Inserted 58857 docs @ 11545 writes/s, 120992 RU/s (314B max monthly 1KB reads)
    Inserted 69547 docs @ 11378 writes/s, 119237 RU/s (309B max monthly 1KB reads)
    Inserted 80687 docs @ 11345 writes/s, 118896 RU/s (308B max monthly 1KB reads)
    Inserted 91455 docs @ 11272 writes/s, 118131 RU/s (306B max monthly 1KB reads)
    Inserted 102129 docs @ 11208 writes/s, 117461 RU/s (304B max monthly 1KB reads)
    Inserted 112444 docs @ 11120 writes/s, 116538 RU/s (302B max monthly 1KB reads)
    Inserted 122927 docs @ 11063 writes/s, 115936 RU/s (301B max monthly 1KB reads)
    Inserted 133157 docs @ 10993 writes/s, 115208 RU/s (299B max monthly 1KB reads)
    Inserted 144078 docs @ 10988 writes/s, 115159 RU/s (298B max monthly 1KB reads)
    Inserted 155415 docs @ 11013 writes/s, 115415 RU/s (299B max monthly 1KB reads)
    Inserted 166126 docs @ 10992 writes/s, 115198 RU/s (299B max monthly 1KB reads)
    Inserted 173051 docs @ 10739 writes/s, 112544 RU/s (292B max monthly 1KB reads)
    Inserted 180169 docs @ 10527 writes/s, 110324 RU/s (286B max monthly 1KB reads)
    Inserted 192469 docs @ 10616 writes/s, 111256 RU/s (288B max monthly 1KB reads)
    Inserted 199107 docs @ 10406 writes/s, 109054 RU/s (283B max monthly 1KB reads)
    Inserted 200000 docs @ 9930 writes/s, 104065 RU/s (270B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 200000 docs @ 9928 writes/s, 104063 RU/s (270B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.
    Press any key to exit...


**(Gerekiyorsa) 4. adım:** bildirilen üretilen işi (RU/s) aracından aynı olması gerekir ya da daha yüksek sağlanan işleme koleksiyonunun veya koleksiyonları kümesi. Değilse, küçük artışlarla DegreeOfParallelism artırma sınırına ulaştığında yardımcı olabilir. İstemci uygulamanızı akışından plateaus, uygulamanın birden çok örneğini ek istemci makineleri başlatın. Bu adım yardıma gereksinim duyarsanız, e-posta askcosmosdb@microsoft.com veya bir destek bileti gelen dosya [Azure portal](https://portal.azure.com).

Uygulamayı oluşturduktan sonra farklı deneyebilirsiniz [ilkeleri dizin](indexing-policies.md) ve [tutarlılık düzeylerini](consistency-levels.md) üretilen iş ve gecikmeyi üzerindeki etkilerini anlamak için. Kaynak kodu gözden geçirmek ve kendi test paketleri ya da üretim uygulamaları için benzer yapılandırmaları uygulamak.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, nasıl performansı ve ölçeği bir .NET konsol uygulaması kullanarak Azure Cosmos DB ile test gerçekleştirebileceğiniz en arama. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Cosmos DB performans örnek testi](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Azure Cosmos DB performansını artırmak için istemci yapılandırma seçenekleri](performance-tips.md)
* [Sunucu tarafı Azure Cosmos DB'de bölümlendirme](partition-data.md)


