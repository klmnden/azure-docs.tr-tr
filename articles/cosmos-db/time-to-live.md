---
title: Yaşam süresi Azure Cosmos veritabanı verileriyle sona | Microsoft Docs
description: TTL ile Microsoft Azure Cosmos DB sistemden bir süre sonra otomatik olarak temizlenir dosyalarınız olanağı sağlar.
services: cosmos-db
documentationcenter: ''
keywords: yaşam süresi
author: arramac
manager: jhubbard
editor: ''
ms.assetid: 25fcbbda-71f7-414a-bf57-d8671358ca3f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2017
ms.author: arramac
ms.openlocfilehash: 6f8ce4e270b94bf1043c27ba879878e20372ffe7
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="expire-data-in-azure-cosmos-db-collections-automatically-with-time-to-live"></a>Otomatik olarak süresi ile Azure Cosmos DB koleksiyonlarda verileri süresi dolacak
Uygulamalar oluşturmak ve çok büyük miktarda veri depolayın. Bu veriler, bazı bilgiler yalnızca sınırlı bir süre için yararlıdır oluşturulan makine olay verileri, günlükler ve kullanıcı oturumu ister. Veri olduktan sonra uygulamanızın gereksinimlerine fazlalık, bu verileri temizlemek ve bir uygulamanın depolama gereksinimlerini azaltmak güvenlidir.

"Yaşam süresi" veya TTL ile Microsoft Azure Cosmos DB veritabanından bir süre sonra otomatik olarak temizlenir dosyalarınız olanağı sağlar. Varsayılan yaşam süresi koleksiyon düzeyinde ayarlamak ve bir belge başına temelinde geçersiz kılındı. Bir koleksiyonu varsayılan olarak veya bir belge düzeyinde TTL ayarlandıktan sonra Cosmos DB son değiştirildiği olduğundan, bu süre, saniye sonra mevcut belgeleri otomatik olarak kaldırır.

Cosmos DB'de yaşam süresi belgenin en son değiştirildiği bir uzaklık karşı kullanır. Bunu kullandığı yapmak için `_ts` her belge üzerinde var olan alan. Tarih ve saatini temsil eden bir UNIX stili dönem zaman damgası _ts alanıdır. `_ts` Alanı her zaman bir belge değiştirildiğinde güncelleştirilir. 

## <a name="ttl-behavior"></a>TTL davranışı
TTL özelliği, iki düzeyde - koleksiyon düzeyinde ve belge düzeyi TTL özellikleri tarafından denetlenir. Saniye cinsinden ayarlanır ve bir delta olarak kabul edilir. değerler `_ts` belgeyi son değiştirilme saati.

1. Koleksiyon için DefaultTTL
   
   * Eksikse (veya null olarak ayarlanır) belgeleri otomatik olarak silinmez.
   * Mevcut değer ise "-1" sonsuz – = belgeleri varsayılan olarak süresi yok
   * Mevcut ve değeri bir numara ("n") – belgelerin süresi dolsun varsa "n" son değişikliğinden sonra saniye
2. TTL belgeler için: 
   
   * Özelliği, yalnızca DefaultTTL üst koleksiyonu için mevcut olduğunda geçerlidir.
   * Üst koleksiyonun DefaultTTL değerini geçersiz kılar.

Belgenin süresi doldu hemen (`ttl`  +  `_ts` < geçerli sunucu zamanı =), belgenin "süresi"olarak işaretlenmiş. Bu tarihten sonra bu belgeler üzerinde hiçbir işlem izin verilecek ve gerçekleştirilen herhangi bir sorgu sonuçlarından bırakılır. Belgeleri sistemde fiziksel olarak silinir ve arka planda mümkün daha sonra silinir. Bu kullanılmasına neden değil [istek birimlerine (RUs)](request-units.md) koleksiyonu bütçeden.

Yukarıdaki mantığı aşağıdaki matrisinde gösterilebilir:

|  | DefaultTTL eksik değil koleksiyonda ayarlayın. | DefaultTTL = -1 koleksiyonu | DefaultTTL koleksiyonunda "n" = |
| --- |:--- |:--- |:--- |
| TTL eksik belgesi |Belge ve koleksiyon TTL kavramına sahip olduğundan belge düzeyinde geçersiz kılmak için bir şey yok. |Bu koleksiyonun hiç belgelerde sona erecek. |Bu koleksiyon belgelerde aralığı n sona erdiğinde sona erecek. |
| TTL belgesinde -1 = |Belge düzeyinde toplamadan beri geçersiz kılmak için hiçbir şey bir belge kılabilirsiniz DefaultTTL özelliği tanımlamıyor. TTL belgesinde sistem tarafından yorumlanan kaydetmeyin. |Bu koleksiyonun hiç belgelerde sona erecek. |TTL =-1'de bu koleksiyonun belgeyle asla sona erecek. Diğer tüm belgeler "n" aralığından sonra sona erecek. |
| TTL belgesinde n = |Belge düzeyinde geçersiz kılmak için bir şey yok. TTL belgesinde sistem tarafından yorumlanan kaydetmeyin. |TTL belgeyle = n saniye cinsinden aralık n sonra dolacak. Diğer belgeleri -1 aralığı devralır ve süresi dolmayacak. |TTL belgeyle = n saniye cinsinden aralık n sonra dolacak. Diğer belgeler "n" aralığı koleksiyondan devralır. |

## <a name="configuring-ttl"></a>TTL yapılandırma
Varsayılan olarak, yaşam süresi tüm Cosmos DB koleksiyonlarında ve tüm belgeler üzerinde varsayılan olarak devre dışıdır. TTL ayarlanabilir program aracılığıyla veya Azure Portalı'nda, **ayarları** bölüm koleksiyonu. 

## <a name="enabling-ttl"></a>TTL etkinleştirme
Bir koleksiyon veya bir koleksiyon içindeki belgelerde TTL etkinleştirmek için bir koleksiyonun DefaultTTL özelliğini -1 veya sıfır olmayan pozitif bir sayı için ayarlamanız gerekir. DefaultTTL-1 koleksiyondaki tüm belgeleri sonsuza kadar Canlı varsayılan ancak Cosmos DB hizmeti tarafından olarak ayarlandığında bu koleksiyon, bu varsayılanı geçersiz belgeler için izlemeniz gerekir.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a>Bir koleksiyonda varsayılan TTL yapılandırma
Bir varsayılan koleksiyon düzeyinde yaşam süresi yapılandırmadı. Bir koleksiyonda TTL ayarlamak için zaman damgası belgenin son değiştiren sonra koleksiyondaki tüm belgeleri süresi dolacak şekilde saniye cinsinden süre gösteren sıfır olmayan pozitif bir sayı sağlamanız gerekir (`_ts`). Veya, koleksiyona eklenen tüm belgeleri süresiz olarak varsayılan olarak dinamik gelir-1 varsayılan ayarlayabilirsiniz.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a>Bir belge TTL ayarı
Bir koleksiyonda varsayılan TTL ayarlanmasına ek olarak, bir belge düzeyinde belirli TTL ayarlayabilirsiniz. Bunun yapılması varsayılan koleksiyon değerini geçersiz kılar.

* Bir belgeyi TTL ayarlamak için zaman damgası belgenin son değiştiren sonra belgenin süresi dolacak şekilde saniye cinsinden süre gösteren sıfır olmayan pozitif bir sayı sağlamanız gerekir (`_ts`).
* Bir belge TTL alanı varsa, varsayılan koleksiyon uygulanır.
* TTL koleksiyon düzeyinde devre dışıysa, TTL koleksiyonda yeniden etkinleştirilene kadar belgeyi TTL alanı yoksayılacak.

TTL sona erme bir belgeyi ayarlama gösteren bir parçacığı aşağıda verilmiştir:

    // Include a property that serializes to "ttl" in JSON
    public class SalesOrder
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        
        [JsonProperty(PropertyName="cid")]
        public string CustomerId { get; set; }
        
        // used to set expiration policy
        [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
        public int? TimeToLive { get; set; }
        
        //...
    }
    
    // Set the value to the expiration in seconds
    SalesOrder salesOrder = new SalesOrder
    {
        Id = "SO05",
        CustomerId = "CO18009186470",
        TimeToLive = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days 
    };


## <a name="extending-ttl-on-an-existing-document"></a>Var olan bir belgeyi TTL genişletme
Belge üzerinde herhangi bir yazma işlemi yaparak belgesinde TTL sıfırlayabilirsiniz. Bunu yaptığınızda bu ayarlayacak `_ts` geçerli saati ve belirlediği belge süre sonu için geri sayım `ttl`, yeniden başlar. Değiştirmek istiyorsanız `ttl` bir belgenin diğer ayarlanabilir alan yaptığınız gibi alanı güncelleştirebilirsiniz.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time to live
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="removing-ttl-from-a-document"></a>Bir belgeden TTL kaldırma
TTL bir belgeyi ayarlama ve süresi dolacak şekilde bu belgeyi artık istiyorsanız sonra belgeyi almak, TTL alanı kaldırabileceğiniz ve sunucuda belge değiştirin. TTL alanı belgeden kaldırıldığında, koleksiyon varsayılan uygulanır. Bir belgeden zaman aşımına uğramak durdurup koleksiyondan devralmayan sonra TTL değeri -1 olarak ayarlandığında gerekir.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit the default TTL of the collection
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="disabling-ttl"></a>TTL devre dışı bırakma
TTL bir koleksiyonda tamamen devre dışı bırakın ve koleksiyon DefaultTTL özellikte süresi dolan belgeleri mi arıyorsunuz gelen arka plan işlemi durdurmak için silinmesi gerekir. Bu özellik silme -1 olarak ayarından farklıdır. Koleksiyona eklenen yeni belgeler-1 anlamına gelir ayarına sonsuza kadar Canlı ancak, bu koleksiyondaki belirli belgeleri kılabilirsiniz. Önceki bir varsayılan açıkça silmiş belgeleri olsa bile bu özellik tamamen koleksiyonundan kaldırılması hiçbir belge dolacağını anlamına gelir.

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);

<a id="ttl-and-index-interaction"></a> 
## <a name="ttl-and-index-interaction"></a>TTL ve dizin etkileşimi
Ekleyerek veya değiştirerek bir koleksiyon TTL ayarda temel dizin değiştirir. TTL değeri gelen kapatmak için açık değiştirildiğinde, koleksiyon reindexed. Dizin oluşturma modu tutarlı olduğunda dizin oluşturma ilkesini değişiklikler yaparken, kullanıcıların dizin için bir değişiklik olduğunu fark etmez. Dizin oluşturma modu olduğunda çok yavaş ayarlanır dizini her zaman yakalama ve TTL değeri değiştirdiyseniz, dizini sıfırdan yeniden oluşturulur. TTL değeri değiştirildiğinde ve dizini modu yavaş ayarlanır, dizini yeniden oluşturma sırasında yapılan sorguları tamamlandı ya doğru sonuçlar döndürmeyin.

Döndürülen verilerin tam gerekiyorsa, dizin oluşturma modu yavaş ayarlandığında TTL değeri değiştirmeyin. İdeal olarak tutarlı dizini tutarlı sorgu sonuçları emin olmak için seçilmelidir. 

## <a name="faq"></a>SSS
**Ne TTL bana maliyeti ne olacak?**

Bir belge üzerinde bir TTL ayarına ek bir maliyet yoktur.

**Ne kadar TTL yukarı olduğunda my belge silmek için sürer?**

Hemen TTL çalışır durumda ve CRUD veya sorgu API'leri yoluyla erişilemeyecek belgeleri süresi dolmuş. 

**TTL belgesinde herhangi RU giderler etkileyecek?**

Hayır, hiçbir etkisi olacaktır RU masrafları Cosmos DB'de TTL aracılığıyla süresi dolan belgelerin silme işlemleri için.

**TTL özelliği yalnızca tüm belgeler için geçerli veya belgenin özellik değerlerini süresinin dolmasını sağlayabilir mi?**

TTL belgenin tamamına uygulanır. Ardından bir belgeyi yalnızca bir kısmı süresi dolacak şekilde isterseniz, bölümü ana belge için ayrı bir "bağlı" Belge ayıklayın ve ardından TTL ayıklanan bu belgeyi kullanmak önerilir.

**TTL özelliği, belirli bir dizin oluşturma gereksinimleri var mı?**

Evet. Koleksiyon olmalıdır [ilke kümesi dizin](indexing-policies.md) CONSISTENT veya Lazy. None olarak ayarlanmış dizin ile bir koleksiyonda DefaultTTL ayarlanmaya çalışılırken bir hata, önceden ayarlanmış bir DefaultTTL sahip bir koleksiyonun üzerinde dizin oluşturmayı devre dışı bırakma çalışılırken olarak neden olur.

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB hakkında daha fazla bilgi için hizmete bakın [ *belgelerine* ](https://azure.microsoft.com/documentation/services/cosmos-db/) sayfası.

