---
title: Azure Cosmos DB'de depolama maliyetini en iyi duruma getirme
description: Bu makalede, Azure Cosmos DB'de depolanan verilerin depolama maliyetlerini yönetmek açıklanmaktadır
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.openlocfilehash: 71f1f8896126728277ba6f0bf2c0ded1b2a608b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65967251"
---
# <a name="optimize-storage-cost-in-azure-cosmos-db"></a>Azure Cosmos DB'de depolama maliyetini en iyi duruma getirme

Azure Cosmos DB, sınırsız depolama ve aktarım hızı sunar. Azure Cosmos kapsayıcılar veya veritabanları sağlamak ve yapılandırmak için sahip, aktarım hızı depolama tüketim temelinde faturalandırılır. Tükettiğiniz yalnızca mantıksal depolama birimi için faturalandırılır ve herhangi bir depolama alanı önceden ayırmanıza gerek yoktur. Depolama göre otomatik olarak yukarı ve aşağı ölçeklendirir ekler veya bir Azure Cosmos DB kapsayıcısı için veriler.

## <a name="storage-cost"></a>Depolama maliyeti

Depolama GB birimle faturalandırılır. Yerel SSD destekli depolama, verilerinizi tarafından kullanılan ve dizin oluşturma. Kullanılan toplam depolama alanı verileri için gerekli olan depolama ve Azure Cosmos DB burada kullandığınız tüm bölgeler arasında kullanılan dizinlerin eşittir. Genel olarak bir Azure Cosmos hesabı üç bölgeler arasında çoğaltmak, her üç konusu bölgeler toplam depolama maliyeti için ödeme yaparsınız. Depolama gereksinimi tahmin etmek için bkz: [kapasite Planlayıcısı](https://www.documentdb.com/capacityplanner) aracı. Azure Cosmos DB'de depolama maliyetini 0,25 dolar GB'dir/ay bkz [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/) en son güncelleştirmeler için. Azure Cosmos kapsayıcınızı tarafından kullanılan depolama alanını belirlemek için uyarılar ayarlayın depolama alanınızı izlemek için bkz [İzleyici Azure Cosmos DB](monitor-accounts.md)) makalesi.

## <a name="optimize-cost-with-item-size"></a>Maliyet öğe boyutu ile en iyi duruma getirme

Azure Cosmos DB öğesi boyutunun 2 MB olmasını bekliyor veya en iyi performans ve maliyet avantajları için daha az. Depolamak için herhangi bir öğeyi veri 2 MB'tan büyük gerekiyorsa öğesi şema yeniden tasarlamayı göz önünde bulundurun. Şemayı yeniden tasarlamanız olamaz ender olayda öğesi alt öğesi Böl ve bunları mantıksal olarak bir ortak identifier(ID) ile bağlayabilirsiniz. Tüm Azure Cosmos DB özellikleri için mantıksal tanımlayıcı bağlama göre tutarlı bir şekilde çalışmaz.

## <a name="optimize-cost-with-indexing"></a>Dizin oluşturma ile maliyeti iyileştirin

Varsayılan olarak, veriler otomatik olarak, hangi tüketilen toplam depolama alanı artırabilirsiniz dizine alınır. Ancak, bu ek yükü azaltmak için özel dizin ilkeleri uygulayabilirsiniz. İlke ile ayarlanan değil otomatik dizin oluşturma, öğe boyutunun yaklaşık 10-%20 olur. Kaldırma veya dizin ilkeleri özelleştirme ödemeyin ekstra maliyet yazma ve ek aktarım hızı kapasitesine gerekmez. Bkz: [Azure Cosmos DB'de dizinleme](indexing-policies.md) özel dizin oluşturma ilkeleri yapılandırmak için. Önce ilişkisel veritabanları ile çalıştıysanız, "her şeyi dizine" depolama veya üzeri Katlama anlamına düşünebilirsiniz. Ancak, Azure Cosmos DB'de ORTANCA durumda bu çok daha düşüktür. Azure Cosmos DB'de dizin depolama yükü (10-%20) genellikle düşük bir düşük depolama ayak izini için tasarlandığından bile otomatik dizin oluşturma, ile. Dizin oluşturma ilkesini yöneterek artırabilen dizin Ayak izi ve sorgu performansını daha ayrıntılı bir şekilde denetleyebilirsiniz.

## <a name="optimize-cost-with-time-to-live-and-change-feed"></a>Canlı ve değişiklik akışını zaman maliyeti iyileştirin

Sonra artık gereksiniminiz düzgün bir şekilde Azure Cosmos hesabınızdan kullanarak silebilirsiniz [yaşam süresi](time-to-live.md), [değişiklik akışını](change-feed.md) veya eski verileri Azure blob depolama gibi başka bir veri deposuna geçirebilirsiniz veya Azure veri ambarı. Yaşam süresi veya TTL, Azure Cosmos DB öğeleri belirli bir zaman aralığına sonra otomatik olarak bir kapsayıcıdan silme olanağı sağlar. Varsayılan olarak, zaman kapsayıcı düzeyinde canlı ve geçersiz kılma değeri öğe başına temelinde ayarlayabilirsiniz. Bir kapsayıcı veya bir öğe düzeyinde TTL ayarladıktan sonra Azure Cosmos DB son değiştirme zamanı beri bir süre sonra otomatik olarak bu öğeleri kaldırır. Değişiklik akışı kullanarak, Azure Cosmos DB'de ya da başka bir kapsayıcı veya dış veri deposuna verileri geçirebilirsiniz. Sıfır kesinti geçiş alır ve istediğiniz zaman geçişini, Bitti, silmek veya kaynak Azure Cosmos kapsayıcı silmek için yaşam süresi yapılandırabilirsiniz.

## <a name="optimize-cost-with-rich-media-data-types"></a>Zengin medya veri türleriyle maliyeti iyileştirin 

Zengin medya türleri, örneğin, videolar, resimler, vs. depolamak istiyorsanız Azure Cosmos DB içinde birkaç seçenek vardır. Azure Cosmos öğeleri olarak bu zengin medya türleri depolamak bir seçenektir. Öğe başına 2 MB'lık bir sınır yoktur ve birden çok alt öğeler veri öğesi zincirleme tarafından bu sınırı önleyebilirsiniz. Veya Azure Blob storage'da depolamak ve bunları Azure Cosmos öğelerinizi başvurmak için meta verileri kullanın. Bir dizi Artıları ve eksileri bu yaklaşım vardır. İlk yaklaşım gecikme süresi, aktarım hızı SLA'ları ve normal Azure Cosmos öğelerinizi yanı sıra zengin medya veri türleri için anahtar teslim küresel dağıtım özellikleri açısından en iyi performansı alır. Ancak daha yüksek bir fiyatla desteği sunulmaktadır. Ortam Azure Blob Depolama alanında depolayarak, genel maliyetlerinizi düşürün. Gecikme süresinin kritik olduğu, premium depolama, Azure Cosmos öğelerinden başvurulan zengin medya dosyaları için de kullanabilirsiniz. Bu yerel olarak coğrafi kısıtlama aşmak için daha düşük maliyetle uç sunucu görüntülerini sunmak için CDN ile tümleşir. Bu senaryo ile olumsuz tarafı, iki hizmet ile - Azure Cosmos DB ve işletimsel maliyetlerinizi artırabilir Azure Blob Depolama, dağıtılacak sahip olduğunu belirtir. 

## <a name="check-storage-consumed"></a>Tüketilen depolama alanını kontrol edin

Bir Azure Cosmos kapsayıcı depolama kullanımını denetlemek için kapsayıcıdaki bir baş veya GET isteği çalıştırın ve incelemek `x-ms-request-quota` ve `x-ms-request-usage` üstbilgileri. Alternatif olarak, .NET SDK ile çalışırken kullanabileceğiniz [DocumentSizeQuota](https://docs.microsoft.com/previous-versions/azure/dn850325(v%3Dazure.100)), ve [DocumentSizeUsage](https://msdn.microsoft.com/library/azure/dn850324.aspx) tüketilen depolama alanı almak için özellikleri.

## <a name="using-sdk"></a>SDK'sını kullanma

```csharp
// Measure the item size usage (which includes the index size)
ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));   

Console.WriteLine("Item size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde Azure Cosmos DB'de maliyet iyileştirmesi hakkında daha fazla bilgi edinmek için sonraki geçebilirsiniz:

* Daha fazla bilgi edinin [en iyi duruma getirme için geliştirme ve test etme](optimize-dev-test.md)
* Daha fazla bilgi edinin [Azure Cosmos DB faturanızı anlama](understand-your-bill.md)
* Daha fazla bilgi edinin [aktarım hızı maliyeti en iyi duruma getirme](optimize-cost-throughput.md)
* Daha fazla bilgi edinin [okuma ve yazma işlemleri maliyetini en iyi duruma getirme](optimize-cost-reads-writes.md)
* Daha fazla bilgi edinin [sorguları maliyetini en iyi duruma getirme](optimize-cost-queries.md)
* Daha fazla bilgi edinin [çok bölgeli Azure Cosmos hesapları maliyetini en iyi duruma getirme](optimize-cost-regions.md)

