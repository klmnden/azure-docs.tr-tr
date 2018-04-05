---
title: Azure Cosmos ilkeleri dizin DB | Microsoft Docs
description: Dizin oluşturma Azure Cosmos DB'de nasıl çalıştığını anlayın. Yapılandırma ve otomatik dizin oluşturma ve daha yüksek performans için dizin oluşturma ilkesini değiştirme hakkında bilgi edinin.
keywords: Dizin oluşturma, otomatik işleyişi dizin oluşturma, veritabanının dizin oluşturma
services: cosmos-db
documentationcenter: ''
author: rafats
manager: jhubbard
editor: monicar
ms.assetid: d5e8f338-605d-4dff-8a61-7505d5fc46d7
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/26/2018
ms.author: rafats
ms.openlocfilehash: 5610c5fdc6a04f9ef13d2e4592f0d7e5d8eba30c
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="how-does-azure-cosmos-db-index-data"></a>Azure Cosmos DB dizin verileri nasıl yapar?

Varsayılan olarak, tüm Azure Cosmos DB veri dizin. Birçok müşteri Azure Cosmos otomatik olarak tüm yönlerini dizin işler DB olanak mutluluk olsa da, özel bir belirtebilirsiniz *dizin oluşturma ilkesi* Azure Cosmos veritabanı oluşturma sırasında koleksiyonlar için. Daha esnek ve diğer veritabanı platformlarının ikincil dizinler daha güçlü Azure Cosmos veritabanı dizin oluşturma ilkelerdir. Azure Cosmos DB'de tasarlayın ve şema esnekliği ödün vermeden dizini şeklini özelleştirebilirsiniz. 

Azure Cosmos veritabanı nasıl dizin works öğrenmek için dizin oluşturma ilkesini yönetirken, hassas dengelemeler dizin depolama yükü, yazma ve sorgu işleme ve sorgu tutarlılık arasında yapabilirsiniz anlamak önemlidir.  

Aşağıdaki videoda Azure Cosmos DB Program Yöneticisi Barış Liu yetenekleri ve ayarlamak ve dizin oluşturma ilkesini, Azure Cosmos DB kapsayıcısında yapılandırma dizin otomatik Azure Cosmos DB gösterir. 

>[!VIDEO https://www.youtube.com/embed/uFu2D-GscG0]

Bu makalede, biz Kapat Azure Cosmos DB ilkeleri, dizin oluşturma ilkesini ve ilişkili dengelemeler nasıl özelleştirileceği dizin göz atın. 

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlayın mümkün olacaktır:

* Dahil etmek veya hariç dizine almasını özellikleri nasıl geçersiz kılabilirsiniz?
* Son güncelleştirmeleri dizini nasıl yapılandırabilir miyim?
* ORDER BY veya aralık sorguları gerçekleştirmek için dizin oluşturma nasıl yapılandırabilirsiniz?
* Bir koleksiyona ait dizin oluşturma ilkesi nasıl değişiklik yapıyor?
* Nasıl t depolama ve performans farklı dizin ilkelerinin karşılaştırması nedir?

## Bir dizin oluşturma ilkesini özelleştirme <a id="CustomizingIndexingPolicy"></a>  
Dizin oluşturma ilkesi Azure Cosmos DB koleksiyonunda varsayılan kılarak dengelemeler depolama, yazma ve sorgu performansı ve sorgu tutarlılık arasında özelleştirebilirsiniz. Aşağıdaki durumlar yapılandırabilirsiniz:

* **Dahil etmek veya belgeler ve dizin gelen ve yolları hariç tut**. Hariç tutma veya eklediğinizde veya koleksiyonundaki belgelere yerine belirli belgeleri dizine ekle. Da ekleyebilir ya da bilinen belirli JSON özellikleri hariç *yolları*, bir dizinde bulunan belgeler arasında dizine. Yollar joker karakter düzenleri içerir.
* **Dizin çeşitli yapılandırma**. Dahil edilen her yolu için dizin yolu bir koleksiyon için gerektirir türünü belirtebilirsiniz. Yolun veri, beklenen sorgu iş yükü ve sayısal/dizesi "duyarlık." göre dizin türünü belirtebilirsiniz
* **Dizin güncelleştirme modlarını yapılandırma**. Azure Cosmos DB üç dizin oluşturma modu destekler: tutarlı, yavaş ve yok. Bir Azure Cosmos DB koleksiyonunda dizin oluşturma ilkesi aracılığıyla dizin oluşturma modları yapılandırabilirsiniz. 

Aşağıdaki Microsoft .NET kod parçacığını bir koleksiyon oluşturduğunuzda, özel bir dizin oluşturma ilkesini ayarlamak nasıl gösterir. Bu örnekte en yüksek duyarlık dizeler ve sayılar için bir aralığı dizin ilkesiyle ayarlarız. Dizeleri ORDER BY sorguları yürütmek için bu ilkeyi kullanabilirsiniz.

    DocumentCollection collection = new DocumentCollection { Id = "myCollection" };

    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);   


> [!NOTE]
> JSON şema için dizin oluşturma ilkesi REST API sürümü 2015-06-03 sürümü değişti. Bu sürümle birlikte, ilke dizin oluşturma için JSON şeması aralığı dizinlerini dizeleri destekler. .NET SDK'sı 1.2.0 ve Java, Python ve Node.js SDK'ları 1.1.0 yeni ilke şeması destekler. SDK'ın önceki sürümleri, REST API sürümü 2015-04-08 kullanın. Bunlar, önceki şema İlkesi dizin oluşturma işlemi için destekler.
> 
> Varsayılan olarak, Azure Cosmos DB belgelerde tüm dize özellikleri bir karma dizinine ile tutarlı olarak dizinler. Bu belge içindeki tüm sayısal özellikleri tutarlı bir şekilde aralık dizinine dizinler.  
> 
> 

### <a name="customize-the-indexing-policy-in-the-portal"></a>Dizin oluşturma ilkesini portalında özelleştirme

Dizin oluşturma ilkesini Azure portalında bir koleksiyonun değiştirebilirsiniz: 

1. Portalda, Azure Cosmos DB hesabınıza gidin ve ardından koleksiyonunuzu seçin. 
2. Sol gezinti menüsünde seçin **ayarları**ve ardından **dizin oluşturma ilkesi**. 
3. Altında **dizin oluşturma ilkesi**dizin oluşturma ilkenizi değiştirin ve ardından **Tamam**. 

### Veritabanı dizin oluşturma modları <a id="indexing-modes"></a>  
Azure Cosmos DB destekleyen bir Azure Cosmos DB koleksiyonunda dizin oluşturma İlkesi yoluyla yapılandırabileceğiniz üç dizin oluşturma modu: tutarlı, yavaş ve yok.

**Tutarlı**: bir Azure Cosmos DB koleksiyonunun İlkesi CONSISTENT ise, belirli bir Azure Cosmos DB koleksiyon sorgulamaları noktası okuma için belirtildiği gibi aynı tutarlılık düzeyi izleyin (güçlü, sınırlanmış eskime durumu, oturum veya son). Dizin belge güncelleştirme (ekleme, değiştirme, update ve delete bir belge bir Azure Cosmos DB koleksiyonunda) bir parçası olarak eşzamanlı olarak güncelleştirilir.

Tutarlı dizin oluşturma olası azaltma, tutarlı sorguları yazma performansı destekler. Bu azaltma işlevi sıralanması gerekir benzersiz yol "tutarlılık düzeyi." ise Tutarlı dizin oluşturma modu "hızlı bir şekilde, hemen sorgu yazma" iş yükleri için tasarlanmıştır.

**Yavaş**: Azure Cosmos DB koleksiyon diğer bir deyişle, kullanıcı isteklere yanıt koleksiyonunun işleme kapasitesi tam olarak kullanılmaz, sessiz, olduğunda dizini zaman uyumsuz olarak güncelleştirilir. Yavaş dizin oluşturma modu belge alım gerektiren "sorgu daha sonra şimdi alma" iş yükleri için uygun olabilir. Unutmayın, çünkü veri alınan ve yavaş dizine tutarsız sonuçlar alabilirsiniz. COUNT sorgu veya özel bir sorgu sonuçları belirli bir zamanda tutarlı veya repeatable olmayabilir anlamına gelir. 

Genellikle alınan verilerle yakalama modunda dizinidir. Dizin oluşturma Lazy ile yaşam süresi (TTL) bırakılmalı ve yeniden oluşturulmalıdır dizin sonucunda değişir. Bu sayı ve sorgu sonuçları tutarsız bir süre sağlar. Bu nedenle, çoğu Azure Cosmos DB hesapları tutarlı dizin oluşturma modunu kullanmanız gerekir.

**Hiçbiri**: bir dizin moduna sahip kendisiyle ilişkilendirilmiş herhangi bir dizin yok bir koleksiyon. Bu, Azure Cosmos DB anahtar-değer deposu olarak kullanılıyorsa ve belgeleri yalnızca kimliği özelliği tarafından erişilen yaygın olarak kullanılır. 

> [!NOTE]
> Dizin oluşturma İlkesi'yle hiçbiri yapılandırma varolan bir dizini bırakma yan etkisi vardır. Erişim alışkanlıklarınıza yalnızca kimliği gerektiren ya da kendi bağlantı varsa bunu kullanın.
> 
> 

Aşağıdaki tabloda koleksiyon için yapılandırılan dizin oluşturma modunu (CONSISTENT ve Lazy) ve sorgu isteği için belirtilen tutarlılık düzeyi temel alan sorgular için tutarlılık gösterir. Bu, herhangi bir arabirim kullanılarak yapılan sorguları için geçerlidir: REST API, SDK'ları, ya da içinden yordamları ve Tetikleyicileri depolanabilir. 

|Tutarlılık|Dizin oluşturma modu: tutarlı|Dizin oluşturma modu: geç|
|---|---|---|
|Güçlü|Güçlü|Nihai|
|Sınırlanmış eskime durumu|Sınırlanmış eskime durumu|Nihai|
|Oturum|Oturum|Nihai|
|Nihai|Nihai|Nihai|

Azure Cosmos DB None modu dizin olan koleksiyonları üzerinde yapılan sorgular için bir hata döndürür. Sorguları hala yürütülebilir taramaları aracılığıyla açık olarak **x-ms-documentdb-enable-tarama** REST API üstbilgisinde veya **EnableScanInQuery** isteği seçeneği .NET SDK kullanarak. ORDER BY gibi bazı sorgu özellikleri ile taramaları olarak desteklenmez **EnableScanInQuery**.

Aşağıdaki tabloda, dizin oluşturma modunu (CONSISTENT Lazy ve hiçbiri) temel alan sorgular için tutarlılık gösterilmektedir, **EnableScanInQuery** belirtilir.

|Tutarlılık|Dizin oluşturma modu: tutarlı|Dizin oluşturma modu: geç|Dizin oluşturma modu: yok|
|---|---|---|---|
|Güçlü|Güçlü|Nihai|Güçlü|
|Sınırlanmış eskime durumu|Sınırlanmış eskime durumu|Nihai|Sınırlanmış eskime durumu|
|Oturum|Oturum|Nihai|Oturum|
|Nihai|Nihai|Nihai|Nihai|

Aşağıdaki kod örnek Göster nasıl oluşturacağınız bir Azure Cosmos DB koleksiyonu tutarlı tüm belge eklemeleri üzerinde dizin ile .NET SDK kullanarak.

     // Default collection creates a Hash index for all string fields and a Range index for all numeric    
     // fields. Hash indexes are compact and offer efficient performance for equality queries.

     var collection = new DocumentCollection { Id ="defaultCollection" };

     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

     collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("mydb"), collection);


### <a name="index-paths"></a>Dizin yolları
Azure Cosmos DB JSON belgeleri ve dizin ağaçları modeller. Ağaçtaki yollar için ilkeleri ayarlayabilirsiniz. Belgeler içinde dahil etmek veya hariç dizine almasını yolları seçebilirsiniz. Geliştirilmiş yazma performansı ve sorgu desenlerine önceden bilinir senaryoları için alt dizini Depolama olanağı sunabilirsiniz.

Dizin yolları (/) kök ile başlamalı ve genellikle ile bitmelidir? joker karakter işleci. Bu, birden fazla olası değerler önek olduğunu gösterir. Örneğin, SELECT hizmet için * aileleri F NEREYE F.familyName = "Andersen" /familyName/ için bir dizin yolu içermelidir? koleksiyonun dizini ilkesinde.

Dizin yolları de kullanabilir \* yolları özyinelemeli ön ek altındaki davranışını belirtmek için joker karakter işleci. Örneğin, / yükü / * yükü özellik altında her şeyin dizine almasını dışlamak için kullanılır.

Dizin yolları belirtmek için ortak desenler şunlardır:

| Yol                | Açıklama/kullanım örneği                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /                   | Koleksiyon için varsayılan yolu. Özyinelemeli ve tüm belgeyi ağaca uygular.                                                                                                                                                                                                                                   |
| / prop /?             | Dizin yolu aşağıdaki gibi sorguları sunmak için gerekli (türleriyle, karma veya aralık sırasıyla):<br><br>SELECT FROM koleksiyonu c WHERE c.prop = "değeri"<br><br>SELECT FROM koleksiyonu c WHERE c.prop > 5<br><br>SELECT FROM koleksiyonu c ORDER BY c.prop                                                                       |
| / prop / *             | Belirtilen etiket altındaki tüm yolları için dizin yolu. Aşağıdaki sorgularda ile çalışır<br><br>SELECT FROM koleksiyonu c WHERE c.prop = "değeri"<br><br>SELECT FROM koleksiyonu c WHERE c.prop.subprop > 5<br><br>SELECT FROM koleksiyonu c WHERE c.prop.subprop.nextprop = "değeri"<br><br>SELECT FROM koleksiyonu c ORDER BY c.prop         |
| /props/[]/?         | Dizin yolu gerekli yineleme hizmet ve skalerler ["a", "b", "c"] gibi bir dizi sorguları katılmak için:<br><br>SEÇİN etiket etiket IN collection.props WHERE etiketi = "değeri"<br><br>Koleksiyon c birleştirme etiketi IN c.props SELECT etiketinden burada > 5 etiketi                                                                         |
| /props/ [] /subprop/? | Nesne dizileri birleşim sorguları gibi ve dizin yolu gerekli yineleme sunmak için [{subprop: "a"}, {subprop: "b"}]:<br><br>SEÇİN etiket etiket IN collection.props WHERE tag.subprop = "değeri"<br><br>SEÇİN etiket koleksiyonu c birleştirme etiketi IN c.props WHERE tag.subprop = "değeri"                                  |
| / prop/subprop /?     | Dizin yolu sorguları sunmak için gerekli (türleriyle, karma veya aralık sırasıyla):<br><br>SELECT FROM koleksiyonu c WHERE c.prop.subprop = "değeri"<br><br>SELECT FROM koleksiyonu c WHERE c.prop.subprop > 5                                                                                                                    |

> [!NOTE]
> Özel dizin yolları ayarladığınızda, özel yol tarafından gösterilen tüm belge ağacı için varsayılan dizin oluşturma kuralı belirtmek için gerekli olan "/ *". 
> 
> 

Aşağıdaki örnekte, aralık dizinine ve özel duyarlılık değeri 20 bayt ile belirli bir yola yapılandırır:

    var collection = new DocumentCollection { Id = "rangeSinglePathCollection" };    

    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = 20 } } 
            });

    // Default for everything else
    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/*" ,
            Indexes = new Collection<Index> {
                new HashIndex(DataType.String) { Precision = 3 }, 
                new RangeIndex(DataType.Number) { Precision = -1 } 
            }
        });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), pathRange);


### <a name="index-data-types-kinds-and-precisions"></a>Dizin veri türleri, tür ve Precision bilgisayarlar
Yolu için dizin oluşturma ilkesi yapılandırdığınızda birden fazla seçeneği vardır. Her yol için bir veya daha fazla dizin tanımları belirtebilirsiniz:

* **Veri türü**: dize, sayı, nokta, çokgen veya LineString (yalnızca bir giriş veri türü yolu başına başına içerebilir).
* **Dizin türü**: karma (eşitlik sorguları), aralığı (eşitlik, aralığı veya ORDER BY sorguları) veya Spatial (uzamsal sorguları).
* **Duyarlık**: için bir karma dizinine 8 dizeler ve sayı 1'den değişir bu. Varsayılan değer 3'tür. İçin bir aralığı dizin, bu değer -1 (en yüksek duyarlık) olabilir. 1 ile 100 (en yüksek duyarlık) dize veya sayı değerlerini arasındaki farklılık gösterebilir.

#### <a name="index-kind"></a>Dizin türü
Azure Cosmos DB karma dizin ve aralığı dizin tür dize veya sayı veri türleri için yapılandırılmış her yol veya her ikisi için destekler.

* **Karma** verimli eşitlik ve JOIN sorguları destekler. Çoğu kullanım durumları için varsayılan değer olan 3 bayt daha yüksek duyarlık karma dizinleri gerekmez. Veri türü dize veya sayı olabilir.
* **Aralık** verimli eşitlik sorguları, aralık sorguları destekler (kullanarak >, <>, =, < =,! =) ve ORDER BY sorgular. ORDER By sorguları varsayılan olarak, ayrıca dizin üst sınırından duyarlık (-1) gerektirir. Veri türü dize veya sayı olabilir.

Azure Cosmos DB uzamsal dizin türü noktası, çokgen veya LineString veri türleri için belirtilebilir her yolu için de destekler. Değer belirtilen yolda gibi geçerli bir GeoJSON parçası olmalıdır `{"type": "Point", "coordinates": [0.0, 10.0]}`.

* **Uzamsal** destekler verimli uzamsal (içinde ve uzaklık) sorgular. Veri türü, nokta, çokgen veya LineString olabilir.

> [!NOTE]
> Azure Cosmos DB otomatik dizin oluşturma noktası, Çokgen ve LineString veri türlerini destekler.
> 
> 

Desteklenen dizin türleri ve olabilirler sorguları örnekleri aşağıda verilmiştir sunmak için kullanılır:

| Dizin türü | Açıklama/kullanım örneği                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Karma       | / Prop / karma? (veya /) aşağıdaki sorguları etkili bir şekilde hizmet için kullanılabilir:<br><br>SELECT FROM koleksiyonu c WHERE c.prop = "değeri"<br><br>Karma/özellik / [] üzerinden /? (veya / veya/özellik /) aşağıdaki sorguları etkili bir şekilde hizmet için kullanılabilir:<br><br>WHERE etiketi seçin etiket koleksiyonu c birleştirme etiketi IN c.props = 5                                                                                                                       |
| Aralık      | / Prop / aralığı? (veya /) aşağıdaki sorguları etkili bir şekilde hizmet için kullanılabilir:<br><br>SELECT FROM koleksiyonu c WHERE c.prop = "değeri"<br><br>SELECT FROM koleksiyonu c WHERE c.prop > 5<br><br>SELECT FROM koleksiyonu c ORDER BY c.prop                                                                                                                                                                                                              |
| Uzamsal     | / Prop / aralığı? (veya /) aşağıdaki sorguları etkili bir şekilde hizmet için kullanılabilir:<br><br>SELECT FROM koleksiyonu c<br><br>WHERE st_dıstance (c.prop, {"tür": "Noktası", "coordinates": [0.0, 10.0]}) < 40<br><br>Etkin noktalarında dizin ile SELECT FROM koleksiyonu c nerede ST_WITHIN(c.prop, {"type": "Polygon",...})--<br><br>Etkin çokgenler üzerinde dizin ile SELECT FROM koleksiyonu c nerede ST_WITHIN({"type": "Point",...}, c.prop)--              |

Sorgularla işleçleri gibi aralığı için varsayılan olarak, bir hata döndürülür > = tarama sorgu hizmet gerekebilir göstermek için hiçbir aralık dizini (tüm duyarlık) olduğunda. Aralık sorguları yapılabilir bir aralık dizin olmadan kullanarak **x-ms-documentdb-enable-tarama** REST API üstbilgisinde veya **EnableScanInQuery** isteği seçeneği .NET SDK kullanarak. Azure Cosmos DB dizini göre filtre uygulamak için kullanabileceğiniz sorgu herhangi bir filtre varsa herhangi bir hata döndürülür.

Aynı kuralları uzamsal sorguları için geçerlidir. Varsayılan olarak, uzamsal dizin yok ve dizinden sunulabilecek hiçbir bir filtre uzamsal sorguları için bir hata döndürülür. Bir tarama bunlar gerçekleştirilebilir kullanarak **x-ms-documentdb-enable-tarama** veya **EnableScanInQuery**.

#### <a name="index-precision"></a>Dizin duyarlık
Dizin duyarlık dengelemeler dizin depolama ek yükü ve sorgu performansı arasında yapmak için kullanabilirsiniz. Numaraları için -1 (en çok) varsayılan duyarlık yapılandırmasını kullanmanızı öneririz. Sayı JSON 8 bayt olduğundan, bu 8 bayt bir yapılandırmaya eşdeğerdir. Kesinlik, 1 ile 7 arasında gibi daha düşük bir değer seçmesini bazı aralıklarında değerleri aynı eşleyin anlamına gelir giriş dizini. Bu nedenle, dizin depolama alanını azaltmak, ancak daha fazla belgeleri işlemek üzere sorgu yürütme olabilir. Sonuç olarak, daha fazla verimlilik isteği birimlerindeki tüketir.

Dizin duyarlık yapılandırma dizesi aralıklarıyla daha pratik uygulama vardır. Dizeleri herhangi bir rastgele uzunlukta olabileceğinden, dizin duyarlık seçimi dize aralığı sorguların performansını etkileyebilir. Ayrıca, gerekli olan dizin depolama alanı miktarı da etkileyebilir. Dizinleri dizesi aralık 1 ile 100 veya -1 (en çok) ile yapılandırılabilir. Dize özellikleri ORDER BY sorguları gerçekleştirmek istiyorsanız, karşılık gelen yollar için -1 kesinliğini belirtmeniz gerekir.

Uzamsal dizinler, her zaman varsayılan dizin duyarlık tüm türleri için (Point, LineString ve Çokgen) kullanın. Uzamsal dizinler için varsayılan dizin Duyarlığı geçersiz kılınamaz. 

Aşağıdaki örnek, .NET SDK kullanarak bir koleksiyondaki aralığı dizinler için duyarlık artırmak gösterilmektedir. 

**Özel dizin duyarlık ile bir koleksiyon oluşturma**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };

    // Override the default policy for strings to Range indexing and "max" (-1) precision
    rangeDefault.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), rangeDefault);   


> [!NOTE]
> Azure Cosmos DB bir sorgu ORDER BY kullanıyor, ancak en yüksek duyarlık sorgulanan yolu karşı aralık dizinine sahip olmayan bir hata döndürür. 
> 
> 

Benzer şekilde, dizin oluşturma gelen yollar tamamen dışlayabilirsiniz. Sonraki örnek belgeleri bölümünün tamamını dışlama gösterir (bir *alt ağacı*) kullanarak dizin gelen \* joker işleci.

    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*" });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);



## <a name="opt-in-and-opt-out-of-indexing"></a>Kabul ve dizin oluşturma dışında iptal et
Otomatik olarak tüm belge dizini için koleksiyon isteyip istemediğinizi seçebilirsiniz. Varsayılan olarak, tüm belgeler otomatik olarak dizine alınır, ancak otomatik dizin oluşturma devre dışı bırakabilir. Dizin oluşturma devre dışı bırakıldığında, belgeleri yalnızca aracılığıyla erişilebilen kendi kendine bağlantılar veya belge kullanarak sorgular tarafından kimliği

Otomatik kapalı ile dizin oluşturma, yalnızca belirli belgeler için dizin hala seçmeli olarak ekleyebilirsiniz. Buna karşılık, üzerinde dizin otomatik bırakabilir ve belirli belgeleri dışlamak seçmeli olarak seçin. Açık/kapalı yapılandırmaları dizin yararlı sorgulanması gereken belgeleri yalnızca bir kısmı sahip olduğunuzda.

Aşağıdaki örnek bir belge açık kullanarak içerecek şekilde gösterilmektedir [SQL API .NET SDK'sını](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet) ve [RequestOptions.IndexingDirective](http://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx) özelliği.

    // If you want to override the default collection behavior to either
    // exclude (or include) a document in indexing,
    // use the RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## <a name="modify-the-indexing-policy-of-a-collection"></a>Bir dizin oluşturma ilkesini değiştirme
Azure Cosmos DB'de anında koleksiyonunun dizin oluşturma ilkesi değişiklikleri yapabilirsiniz. Dizin oluşturma ilkesi Azure Cosmos DB koleksiyonunda bir değişikliğin dizini şeklinde bir değişiklik neden olabilir. Değişiklik sıralanabilir yolları, kendi duyarlık ve dizin tutarlılık modelinin etkiler. İlke etkili bir şekilde dizin oluşturma bir değişikliğin eski dizin dönüştürme içine yeni bir dizin gerektirir. 

**Çevrimiçi dizin dönüşümleri**

![Dizin oluşturma şeklini – Azure Cosmos DB çevrimiçi dizin dönüşümleri](./media/indexing-policies/index-transformations.png)

Dizin dönüşümleri çevrimiçi hale getirilir. Bu eski İlkesi dizine belgeleri yeni ilke verimli bir şekilde dönüştürülmeden anlamına gelir *yazma kullanılabilirlik veya sağlanan işleme etkilemeden* koleksiyonu. Tutarlılığını okuma ve yazma işlemlerini SDK'ları, REST API kullanarak yapılan veya içinden saklı yordamları ve Tetikleyicileri etkilenmez dizin dönüştürme sırasında. Performans düşüşünü veya yoktur, uygulamalarınız için kapalı kalma süresi bir dizin oluşturma ilkesi değişiklik yaptığınızda.

Ancak, dizin dönüştürme ilerleme olduğu süre boyunca, sorguları dizin oluşturma modu yapılandırma bağımsız olarak (CONSISTENT veya Lazy) sonunda tutarlı değil. Bu aynı zamanda sorguları tüm arabirimlerinden geçerlidir: REST API, SDK'ları ve yordamları ve Tetikleyicileri içinden depolanır. Gibi dizin oluşturma, Lazy ile dizin dönüştürme zaman uyumsuz olarak arka planda çoğaltmalar üzerinde belirli bir çoğaltma için kullanılabilir yedek kaynakları kullanılarak gerçekleştirilir. 

Dizin dönüşümleri yerinde da yapılır. Azure Cosmos DB dizin ve eski dizin çıkışı takas iki kopyasını yeni bir bilgisini korumaz. Bu, hiçbir ek disk alanı gerekli veya dizin dönüşümleri meydana gelirken, koleksiyonlarda tüketilen anlamına gelir.

Dizin oluşturma ilkesini değiştirdiğinizde, değişiklikleri birincil dizin oluşturma modu yapılandırmalarını temel alan yeni eski dizinden taşımak için uygulanır. Dizin oluşturma modu yapılandırmaları dahil ve Dışlanan yollar, dizin türleri ve Precision bilgisayarlar gibi diğer değerleri daha büyük bir rol oynar. 

Eski ve yeni ilkelerinizi hem tutarlı dizin kullanırsanız, bir çevrimiçi dizin dönüşümü Azure Cosmos DB gerçekleştirir. Dönüştürme işlemi devam ederken tutarlı dizin oluşturma modu olan başka bir dizin oluşturma ilkesi değişikliği uygulanamıyor. Ancak, Lazy veya hiçbiri dönüştürme sırasında dizin oluşturma modu sürüyor geçebilirsiniz: 

* Lazy için taşıdığınızda, dizin ilke değişikliği hemen etkili olur. Azure Cosmos DB dizin oluşturma zaman uyumsuz olarak yeniden başlatır. 
* None taşıdığınızda, dizin hemen bırakılır. None taşıma sürüyor dönüştürme iptal edin ve farklı bir dizin oluşturma ilkesi ile yeni başlatmak istediğinizde kullanışlıdır. 

Aşağıdaki kod parçacığını bir koleksiyona ait yavaş dizin oluşturma moduna tutarlı dizin modundan dizin oluşturma ilkesini değiştirmek nasıl gösterir. .NET SDK'sı kullanıyorsanız, yeni kullanarak bir dizin oluşturma ilkesi değişikliği kazandırın **ReplaceDocumentCollectionAsync** yöntemi.

**Dizin oluşturma CONSISTENT ilkesinden Lazy değiştirme**

    // Switch to Lazy indexing mode.
    Console.WriteLine("Changing from Default to Lazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);

**Dizin dönüştürme ilerlemesini izlemek**

Kullanarak tutarlı bir dizin için dizin dönüştürme yüzdesi ilerlemesini izleyebilirsiniz **IndexTransformationProgress** yanıt özelliğinden bir **ReadDocumentCollectionAsync** çağırın. Diğer SDK ve REST API dizin oluşturma ilkesi değişiklikleri yapmak için eşdeğer özellikleri ve yöntemleri destekler. Tutarlı bir dizin için bir dizin dönüşümü ilerlemesini çağırarak denetleyebilirsiniz **ReadDocumentCollectionAsync**: 

    long smallWaitTimeMilliseconds = 1000;
    long progress = 0;

    while (progress < 100)
    {
        ResourceResponse<DocumentCollection> collectionReadResponse = await client.ReadDocumentCollectionAsync(
            UriFactory.CreateDocumentCollectionUri("db", "coll"));

        progress = collectionReadResponse.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromMilliseconds(smallWaitTimeMilliseconds));
    }

> [!NOTE]
> * **IndexTransformationProgress** tutarlı bir dizine dönüştürülürken özelliği uygulanabilir. Kullanım **ResourceResponse.LazyIndexingProgress** yavaş bir dizin dönüştürmeleri izleme özelliği.
> * **IndexTransformationProgress** ve **LazyIndexingProgress** özellikleri yalnızca bölümlenmemiş koleksiyonu, diğer bir deyişle, bir bölüm anahtarı oluşturulan bir koleksiyon için doldurulur.
>

Bir koleksiyon için dizin hiçbiri modu dizin taşıyarak düşürebilir. Devam eden dönüştürmeyi iptal etmek ve yeni bir tane hemen başlatmak istiyorsanız bu yararlı bir işlemsel aracı olabilir.

**Bir koleksiyonun dizini bırakın**

    // Switch to Lazy indexing mode.
    Console.WriteLine("Dropping index by changing to to the None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

Dizin oluşturma ilkesi değişiklikleri Azure Cosmos DB koleksiyonlarınızı yaptığınızda? En yaygın kullanım örnekleri şunlardır:

* Normal işlem sırasında tutarlı sonuçlar sunar, ancak toplu veri içeri aktarmaları sırasında yavaş dizin oluşturma moduna geri döner.
* Yeni dizin oluşturma özellikleri geçerli Azure Cosmos DB koleksiyonlarınızı kullanmaya başlayın. Örneğin, Jeo-uzamsal sorgulama, uzamsal dizin türü gerektiren kullanın / dize aralığı dizin türü gerektiren aralığı sorguları dize BY sipariş.
* Elle-dizine özellikleri seçin ve zamanla değiştirin.
* Sorgu performansını artırmak için veya kapladığı depolama alanını azaltmak için dizin oluşturma duyarlık ayarlayın.

> [!NOTE]
> Dizin oluşturma ilkesini kullanarak değiştirmek için **ReplaceDocumentCollectionAsync**, sürüm 1.3.0 veya .NET SDK'sı daha sonraki bir sürümünü kullanmanız gerekir.
> 
> Dizin dönüştürme başarıyla tamamlamak yeterli boş depolama alanı kullanılabilir olduğunu koleksiyonda emin olun. Toplama, depolama kotasını ulaşırsa, dizin dönüştürme duraklatıldı. Örneğin, depolama alanı kullanılabilir olduğunda bazı belgeleri silerseniz dizin dönüştürme otomatik olarak devam ettirir.
> 
> 

## <a name="performance-tuning"></a>Performans ayarı
SQL API'leri kullanılan dizin depolama alanı ve her işlem için işleme maliyeti (istek birimleri) gibi performans ölçümleri hakkında bilgi sağlar. Çeşitli dizin oluşturma ilkeleri karşılaştırmak için bu bilgileri kullanın ve performans ayarlama.

Depolama kotası ve bir koleksiyon kullanımını denetlemek için çalıştırın bir **HEAD** veya **almak** koleksiyon kaynağı isteği. Ardından, inceleme **x-ms-istek-quota** ve **x-ms-istek kullanım** üstbilgileri. .NET SDK'sındaki [DocumentSizeQuota](http://msdn.microsoft.com/library/dn850325.aspx) ve [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) özelliklerinde [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) bu karşılık gelen değerler içeriyor.

     // Measure the document size usage (which includes the index size) against   
     // different policies.
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


Her yazma işlemi dizin yükünü ölçmek için (oluşturma, güncelleştirme veya silme) incelemek **x-ms-istek-ücret** üstbilgi (veya eşdeğer [RequestCharge](http://msdn.microsoft.com/library/dn799099.aspx) özelliğinde [ ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) .NET SDK'sındaki) bu işlemler tarafından tüketilen isteği birim sayısını ölçmek için.

     // Measure the performance (request units) of writes.     
     ResourceResponse<Document> response = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), myDocument);              
     Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);

     // Measure the performance (request units) of queries.    
     IDocumentQuery<dynamic> queryable =  client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"), queryString).AsDocumentQuery();

     double totalRequestCharge = 0;
     while (queryable.HasMoreResults)
     {
        FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>(); 
        Console.WriteLine("Query batch consumed {0} request units",queryResponse.RequestCharge);
        totalRequestCharge += queryResponse.RequestCharge;
     }

     Console.WriteLine("Query consumed {0} request units in total", totalRequestCharge);

## <a name="changes-to-the-indexing-policy-specification"></a>Dizin oluşturma ilkesi belirtimi yapılan değişiklikler
İlke dizin oluşturma için şema değişikliği 7 Temmuz 2015 REST API sürümü 2015-06-03 ile sunulmuştur. SDK sürümlerinde karşılık gelen sınıfları şemayla eşleşecek şekilde yeni uygulamalar vardır. 

Aşağıdaki değişiklikleri JSON belirtiminde uygulanan:

* Dizin oluşturma ilkesi aralığı dizinler için dizeleri destekler.
* Her yol, birden fazla dizin tanımı olabilir. Her bir veri türü için bir tane sağlayabilirsiniz.
* Duyarlık dizin oluşturma, 1 ile 8 numaraları için 1 ile 100 dizeleri ve -1 (en yüksek duyarlık) destekler.
* Yol kesimleri her yol kaçınmak için çift tırnak gerekmez. Örneğin, bir yolu ekleyebilirsiniz   **/başlık /?** yerine **/ "title" /?**.
* "Tüm yolları" temsil eden kök yolu olarak temsil edilebilir **/ \*** (ek olarak **/**).

SDK'sı sürüm 1.2.0, taşımak için .NET SDK'sı 1.1.0 sürümü veya önceki bir sürümü ile yazılmış özel bir dizin oluşturma ilkesi ile bu hükümleri koleksiyonları kodu varsa, bu değişiklikler işlenecek uygulama kodunuz değiştirmeniz gerekir. SDK'ın önceki bir sürümünü kullanmaya devam etmeyi planlıyorsanız, değişiklik gerekmez veya kodu yoksa, dizin oluşturma ilkesini yapılandırır.

Pratik karşılaştırması için önceki REST API sürümü 2015-04-08 kullanılarak yazılan aynı dizin oluşturma ilkesini arkasından REST API sürümü 2015-06-03 kullanılarak yazılan özel bir dizin oluşturma ilkesini bir örneği burada verilmiştir.

**Geçerli JSON (REST API sürümü 2015-06-03) ilkesi dizin oluşturma**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Hash",
                   "dataType":"String",
                   "precision":3
                },
                {
                   "kind":"Hash",
                   "dataType":"Number",
                   "precision":7
                }
             ]
          }
       ],
       "ExcludedPaths":[
          {
             "path":"/nonIndexedContent/*"
          }
       ]
    }


**Daha önce ilke JSON (REST API sürümü 2015-04-08) dizin oluşturma**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "IncludedPaths":[
          {
             "IndexType":"Hash",
             "Path":"/",
             "NumericPrecision":7,
             "StringPrecision":3
          }
       ],
       "ExcludedPaths":[
          "/\"nonIndexedContent\"/*"
       ]
    }


## <a name="next-steps"></a>Sonraki adımlar
Dizin ilke yönetimi örnekleri ve Azure Cosmos DB sorgu dili hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

* [SQL API .NET dizin yönetimi kod örnekleri](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
* [SQL API REST toplama işlemleri](https://msdn.microsoft.com/library/azure/dn782195.aspx)
* [SQL sorgusu](sql-api-sql-query.md)

