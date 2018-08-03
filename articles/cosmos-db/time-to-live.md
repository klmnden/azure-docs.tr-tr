---
title: Azure Cosmos DB'de veri yaşam süresi ile sona | Microsoft Docs
description: TTL ile Microsoft Azure Cosmos DB sistemden bir süre sonra otomatik olarak temizlenir belgeleriniz olanağı sağlar.
services: cosmos-db
keywords: yaşam süresi
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 08/29/2017
ms.author: sngun
ms.openlocfilehash: 49f6d6ee65ffae71cba8c73301355bfe2bdcd1d6
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480565"
---
# <a name="expire-data-in-azure-cosmos-db-collections-automatically-with-time-to-live"></a>Yaşam süresi otomatik olarak ile Azure Cosmos DB koleksiyonlarındaki verileri süresi dolacak
Uygulamalar oluşturmak ve çok büyük miktarda veri depolayın. Bu verilerin bazılarını bilgiler yalnızca sınırlı bir süre için yararlıdır oluşturulan makine olay verileri, günlükler ve kullanıcı oturumu ister. Veri ile sunulduktan sonra Fazlalık için uygulamanızın gereksinimlerine, bu verileri temizlemek ve bir uygulamanın depolama gereksinimlerini azaltmak güvenli.

"Yaşam süresi" veya TTL, Microsoft Azure Cosmos DB veritabanından bir süre sonra otomatik olarak temizlenir belgeleriniz olanağı sağlar. Varsayılan yaşam süresi koleksiyon düzeyinde ayarlayın ve belge başına temelinde geçersiz kılındı. Koleksiyonun varsayılan olarak ya da bir belge düzeyinde TTL ayarlandıktan sonra Cosmos DB son değiştirme olduğundan bu süreyi saniye cinsinden sonra mevcut belgeleri otomatik olarak kaldırır.

Belgenin son değiştirildiği zamana göre bir uzaklık Azure Cosmos DB'de yaşam süresi kullanır. Kullandığı bunu yapmanın `_ts` her belgede var olan bir alan. Tarih ve saatini temsil eden bir UNIX stili dönem zaman damgası _ts alandır. `_ts` Alan, bir belge değiştirilir her zaman güncelleştirilir. 

## <a name="ttl-behavior"></a>TTL davranışı
TTL özellik TTL özellikleri, iki düzeyde - koleksiyon düzeyinde ve belge düzeyinde denetlenir. Değerleri saniye olarak ayarlanır ve bir delta olarak kabul edilir `_ts` belgenin son değiştirilme saati.

1. Koleksiyon için DefaultTTL
   
   * Eksikse (veya null olarak ayarlanır) belgeleri otomatik olarak silinmez.
   * Mevcut ve değeri ayarlandığında, "-1" sonsuz – = belgeleri, varsayılan olarak süresi yok
   * Mevcut ve değeri bazı numarasına ("n") olarak ayarlanır: belgelerin süresi dolsun, son değiştirme sonrasında "n" saniye
2. TTL belgeler için: 
   
   * Özelliği, yalnızca DefaultTTL üst koleksiyon için mevcut olduğunda geçerlidir.
   * Üst koleksiyon DefaultTTL değerini geçersiz kılar.

Belge doldu hemen sonra (`ttl`  +  `_ts` < geçerli sunucu saatinden =), belge "süresi dolmuş."olarak işaretlenir. Bu tarihten sonra bu belgeleri üzerinde işlem izin verilecek ve gerçekleştirilen tüm sorguların Eşleşmeyenler sonuçlara dahil. Belgeler, sistemde fiziksel olarak silinir ve arka planda modülüne daha sonra silinir. Bu herhangi tüketmez [istek birimi (RU)](request-units.md) koleksiyon bütçeden.

Yukarıdaki mantıksal aşağıdaki matriste gösterilebilir:

|  | DefaultTTL eksik / koleksiyonunda belirlenen değil | DefaultTTL = -1 koleksiyonu | DefaultTTL = "n" koleksiyonu |
| --- |:--- |:--- |:--- |
| Belge TTL eksik |TTL kavramı belge ve koleksiyonu olduğundan, belge düzeyinde geçersiz kılmak için bir şey yok. |Bu koleksiyonda belge sona erecek. |Aralığı n sona erdiğinde, bu koleksiyondaki belgeleri sona erecek. |
| TTL belgesinde -1 = |Toplama işleminden itibaren belge düzeyinde geçersiz kılmak için hiçbir şey bir belge kılabileceğiniz DefaultTTL özelliği tanımlamıyor. Sistem tarafından bir belge TTL yorumlanmayan. |Bu koleksiyonda belge sona erecek. |Belge TTL =-1'de bu koleksiyonu ile hiçbir zaman sona erecek. Diğer tüm belgeler "n" aralığından sonra sona erecek. |
| TTL belgesinde n = |Belge düzeyinde geçersiz kılmak için bir şey yok. Sistem tarafından bir belge TTL yorumlanmayan. |Belge TTL ile = n saniye cinsinden aralık n sonra sona erecek. Diğer belgeler,-1 Aralık devralır ve süresi dolmayacak. |Belge TTL ile = n saniye cinsinden aralık n sonra sona erecek. Diğer belgeler, koleksiyondan "n" aralığı devralır. |

## <a name="configuring-ttl"></a>TTL yapılandırma
Varsayılan olarak, yaşam süresi tüm Cosmos DB koleksiyonları ve tüm belgeler üzerinde varsayılan olarak devre dışıdır. TTL program aracılığıyla veya Azure portalını kullanarak ayarlayabilirsiniz. Azure Portalı'ndan TTL yapılandırmak için aşağıdaki adımları kullanın:

1. Oturum [Azure portalında](https://portal.azure.com/) ve Azure Cosmos DB hesabınıza gidin.  

2. Gezinme TTL değeri ayarlamak istediğiniz bir koleksiyonu açın **ölçek ve ayarlar** bölmesi. Yaşam süresi varsayılan olarak ayarlanmış olduğunu gördüğünüz **kapalı**. İçin değiştirebileceğiniz **(varsayılan) üzerinde** veya **üzerinde**.

   **Kapalı** -belgeleri otomatik olarak silinmez.  
   **(varsayılan) üzerinde** -bu seçenek TTL değer "belgeleri, varsayılan olarak sona yok anlamına gelen -1" (sınırsız) olarak ayarlar.  
   **üzerinde** -belgelerin süresi dolsun "n" saniye sonra son değiştirme.  

   ![Yaşam süresi ayarlayın](./media/time-to-live/set-ttl-in-portal.png)

## <a name="enabling-ttl"></a>TTL etkinleştirme
Bir koleksiyon veya bir koleksiyonu içindeki belgeler üzerinde TTL etkinleştirmek için bir koleksiyonun DefaultTTL özelliği -1 veya sıfır olmayan pozitif bir sayı olarak ayarlamak gerekir. DefaultTTL-1 koleksiyondaki tüm belgeler devamlı Canlı varsayılan ancak Cosmos DB hizmet ayarlandığında, bu koleksiyonu bu varsayılanı geçersiz kılınmış belgeler için izlemeniz gerekir.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a>Varsayılan TTL üzerindeki bir koleksiyona yapılandırma
Bir varsayılan koleksiyon düzeyinde yaşam süresi yapılandırabilirsiniz. TTL üzerindeki bir koleksiyona ayarlamak için koleksiyondaki tüm belgeler zaman damgası belgenin son değiştirildikten sonra süresi dolacak şekilde saniye cinsinden süre belirten sıfır olmayan pozitif bir sayı sağlamanız gerekir (`_ts`). Veya, varsayılan koleksiyona eklenen tüm belgeleri süresiz olarak varsayılan olarak live gelir-1 ayarlayabilirsiniz.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a>Bir belge TTL ayarı
Varsayılan TTL üzerindeki bir koleksiyona ayarlamanın yanı sıra, belge düzeyinde belirli TTL ayarlayabilirsiniz. Bunun yapılması, koleksiyonun varsayılan geçersiz kılar.

* Bir belgeyi TTL ayarlamak için zaman damgası belgenin son değiştirildikten sonra belgenin sona saniye cinsinden süre belirten sıfır olmayan pozitif bir sayı, sağlamanız gerekir (`_ts`).
* Bir belge TTL alan varsa, varsayılan koleksiyon geçerli olur.
* TTL koleksiyon düzeyinde devre dışı bırakılırsa, TTL koleksiyonunda yeniden etkinleştirilene kadar belge TTL alan yoksayılacak.

TTL sona erme bir belgeyi ayarlama işlemini gösteren bir kod parçacığı aşağıda verilmiştir:

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


## <a name="extending-ttl-on-an-existing-document"></a>Var olan bir belgeyi genişletme TTL
Belge üzerinde herhangi bir yazma işlemi yaparak, bir belge TTL sıfırlayabilirsiniz. Bu ayarlar yapmak `_ts` geçerli saat ve geri sayım'tarafından belirlenen belge sona ereceği `ttl`, yeniden başlar. Değiştirmek istiyorsanız `ttl` bir belgenin diğer ayarlanabilir alan yaptığınız gibi alanın güncelleştirebilirsiniz.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time to live
    
    response = await client.ReplaceDocumentAsync(readDocument);

## <a name="removing-ttl-from-a-document"></a>Bir belge TTL kaldırılıyor
Bir belgeyi bir TTL ayarlandı ve artık bu belgeyi süresi dolacak şekilde istediğiniz, ardından belge alabilir, TTL alanı kaldırın ve sunucuda belge değiştirin. Koleksiyonun varsayılan TTL alanı belgeden kaldırıldığında uygulanır. Süresi dolan bir belgeden durdurup koleksiyondan devralmayan sonra TTL değeri -1 olarak ayarlandığında gerekir.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit the default TTL of the collection
    
    response = await client.ReplaceDocumentAsync(readDocument);

## <a name="disabling-ttl"></a>TTL devre dışı bırakma
TTL üzerindeki bir koleksiyona tamamen devre dışı bırakın ve koleksiyon DefaultTTL özelliği süresi dolmuş belgeleri mi arıyorsunuz gelen arka plan işlemini durdurmak için silinmelidir. Bu özelliği silme -1 olarak ayarlama özelliğinden farklıdır. Ayar koleksiyona eklenen yeni belgelere-1 anlamına gelir her zaman canlı ancak, bu koleksiyondaki belirli belgelere kılabilirsiniz. Önceki bir varsayılan açıkça silmiş belgeleri olsa bile bu özellik tamamen koleksiyonundan kaldırılıyor belge dolacağını anlamına gelir.

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);

<a id="ttl-and-index-interaction"></a> 
## <a name="ttl-and-index-interaction"></a>TTL ve dizin etkileşimi
Temel dizin ekleyerek veya değiştirerek bir koleksiyon TTL ayarını değiştirir. TTL değeri Kapalı öğesinden açık olarak değiştirildiğinde, koleksiyon reindexed. Dizin oluşturma modu tutarlı olduğunda değişiklikler için dizin oluşturma ilkesini yaparken kullanıcıların dizine bir değişiklik olduğunu fark etmez. Dizin oluşturma moduna geç ayarlandığında, dizin her zaman yakalama ve TTL değeri değiştirilirse, dizini sıfırdan yeniden oluşturulur. TTL değeri değiştirildiğinde ve dizin moduna geç ayarlanır, dizinin yeniden oluşturulması sırasında yapılan sorguları tam veya doğru sonuçlar gitmez.

Döndürülen verileri tam gerekiyorsa, dizin oluşturma moduna geç ayarlandığında TTL değeri değiştirmeyin. İdeal olarak tutarlı dizini tutarlı sorgu sonuçları sağlamak için seçilmelidir. 

## <a name="faq"></a>SSS
**TTL bana maliyeti nedir?**

Bir belgeyi bir TTL ayarlamak için hiçbir ek ücret yoktur.

**Ne kadar my belge TTL çalışır duruma geldiğinde silmek sürer?**

Hemen TTL çalışır durumda ve CRUD veya sorgu API'leri aracılığıyla erişilemez belgeleri süresi dolmuş. 

**Bir belge TTL RU ücretlerdir herhangi bir etkisi olacak mı?**

Hayır, olacaktır herhangi bir etkisi üzerinde silme işlemleri aracılığıyla Cosmos DB'de TTL süresi dolmuş belgelerin RU ücretlendirir.

**TTL özelliği yalnızca tüm belgeler için geçerli veya tek tek belge özellik değerlerini süresinin dolmasını sağlayabilir mi?**

TTL belgenin tamamına uygulanır. Bir belgeyi yalnızca bir kısmını sona istiyorsanız, ardından bu bölümü asıl belgeden ayrı bir "bağlı" belgeye ayıklayın ve ardından ayıklanan belge TTL kullanmak önerilir.

**TTL özelliği özel dizin oluşturma gereksinimleri var mı?**

Evet. Koleksiyon olmalıdır [ilke kümesi dizin](indexing-policies.md) Consistent ya da Lazy. Hiçbiri kümesine dizin ile bir koleksiyon üzerinde DefaultTTL ayarlanmaya çalışılırken bir hata, önceden ayarlanmış bir DefaultTTL içeren bir koleksiyon üzerinde dizin oluşturmayı kapatmak çalışılırken olarak neden olur.

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB hakkında daha fazla bilgi için hizmete başvurmak [ *belgeleri* ](https://azure.microsoft.com/documentation/services/cosmos-db/) sayfası.

