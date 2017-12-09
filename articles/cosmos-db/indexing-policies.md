---
title: Azure Cosmos ilkeleri dizin DB | Microsoft Docs
description: "Dizin oluşturma Azure Cosmos DB'de nasıl çalıştığını anlayın. Yapılandırma ve otomatik dizin oluşturma ve daha yüksek performans için dizin oluşturma ilkesini değiştirme hakkında bilgi edinin."
keywords: "Dizin oluşturma, otomatik işleyişi dizin oluşturma, veritabanının dizin oluşturma"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: d5e8f338-605d-4dff-8a61-7505d5fc46d7
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/17/2017
ms.author: arramac
ms.openlocfilehash: 53bf756963c305b8b31ac1a90d219f143522d051
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="how-does-azure-cosmos-db-index-data"></a>Azure Cosmos DB dizin verileri nasıl yapar?

Varsayılan olarak, tüm Azure Cosmos DB veri dizin. Ve birçok müşteri Azure Cosmos otomatik olarak tüm yönlerini dizin işler DB olanak mutluluk olsa da, özel bir belirtme Azure Cosmos DB de destekler **dizin oluşturma ilkesi** oluşturma sırasında koleksiyonlar için. Tasarlama ve şema esnekliği ödün vermeden dizini şeklini özelleştirme izin vermek için dizin oluşturma Azure Cosmos veritabanı daha esnek ve diğer veritabanı platformlar sunulan ikincil dizinler daha güçlü ilkelerdir. Azure Cosmos veritabanı nasıl dizin works öğrenmek için dizin oluşturma ilkesini yöneterek dizin depolama yükü, yazma ve sorgu işleme ve sorgu tutarlılık arasında hassas bileşim hale getirebilir anlamanız gerekir.  

Bu makalede, biz Kapat Azure Cosmos DB ilkeleri, dizin oluşturma göz atın nasıl dizin oluşturma ilkesini ve ilişkili dengelemeler özelleştirebilirsiniz. 

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlayın mümkün olacaktır:

* Dahil etmek veya hariç dizine almasını özellikleri nasıl geçersiz kılabilirsiniz?
* Son güncelleştirmeleri dizini nasıl yapılandırabilir miyim?
* Order By veya aralık sorguları gerçekleştirmek için dizin oluşturma nasıl yapılandırabilirsiniz?
* Bir koleksiyona ait dizin oluşturma ilkesi nasıl değişiklik yapıyor?
* Nasıl t depolama ve performans farklı dizin ilkelerinin karşılaştırması nedir?

## <a id="CustomizingIndexingPolicy"></a>Bir dizin oluşturma ilkesini özelleştirme
Geliştiriciler, bir Azure Cosmos DB koleksiyonu için varsayılan dizin oluşturma ilkesini geçersiz kılma ve aşağıdaki durumlar yapılandırma dengelemeler depolama, yazma/sorgu performansını ve sorgu tutarlılık arasında özelleştirebilirsiniz.

* **Dahil olmak üzere/belgeler ve yolları/dizinden hariç**. Geliştiriciler, belirli belgeleri dışlanan veya ekleme veya koleksiyona değiştirme zamanında dizininde yer seçebilirsiniz. Geliştiriciler ayrıca dahil etmek veya belirli JSON özellikleri paketini hariç seçebilirsiniz bir dizinde bulunan belgeler arasında dizine (dahil olmak üzere joker karakter düzenleri) yolları.
* **Dizin türleri çeşitli yapılandırma**. İçinde bir koleksiyon gereksinim duydukları dizin türünü kendi verilerine dayalı ve sorgu iş yükü ve her yolu için "duyarlık" sayısal/dizesi beklenen her kapsanan yolları için geliştiriciler de belirtebilirsiniz.
* **Dizin güncelleştirme modlarını yapılandırma**. Azure Cosmos DB destekleyen bir Azure Cosmos DB koleksiyonunda dizin oluşturma ilkesi aracılığıyla yapılandırılabilir üç dizin oluşturma modu: tutarlı, Lazy ve yok. 

Aşağıdaki .NET kod parçacığını bir koleksiyon oluşturma sırasında özel bir dizin oluşturma ilkesini ayarlamak nasıl gösterir. Burada dizeler ve sayılar için aralık dizinine ilkesiyle en yüksek duyarlık ayarlarız. Bu ilke bize Order By dizeleri sorgu yürütebilir sağlar.

    DocumentCollection collection = new DocumentCollection { Id = "myCollection" };

    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);   


> [!NOTE]
> Dizin oluşturma ilkesi için JSON şeması aralığı dizinleri dizelere desteklemek için REST API sürümü 2015-06-03 sürümü ile değiştirildi. .NET SDK'sı 1.2.0 ve Java, Python ve Node.js SDK'ları 1.1.0 yeni ilke şeması destekler. Eski SDK'ları REST API sürümü 2015-04-08 kullanan ve dizin oluşturma ilkesi eski şeması destekler.
> 
> Varsayılan olarak, Azure Cosmos DB aralık dizinine tüm dize özellikleri tutarlı bir karma dizinine sahip belgelerde ve sayısal özellikleri dizin oluşturur.  
> 
> 

### <a name="customizing-the-indexing-policy-using-the-portal"></a>Portalı kullanarak dizin oluşturma ilkesini özelleştirme

Azure Portalı'nı kullanarak bir koleksiyonun dizin oluşturma ilkesini değiştirebilirsiniz. Açık Azure portalında Azure Cosmos DB hesabınızı seçin, koleksiyonunuzu sol gezinti menüsünde tıklatın **ayarları**ve ardından **dizin oluşturma ilkesi**. İçinde **dizin oluşturma ilkesi** dikey penceresinde, dizin oluşturma ilkenizi değiştirin ve ardından **Tamam** yaptığınız değişiklikleri kaydetmek için. 

### <a id="indexing-modes"></a>Veritabanı dizin oluşturma modları
Azure Cosmos DB, bir Azure Cosmos DB koleksiyonunda – dizin oluşturma ilkesi aracılığıyla yapılandırılabilir üç dizin oluşturma modu CONSISTENT, Lazy ve hiçbiri destekler.

**Tutarlı**: bir Azure Cosmos DB koleksiyonunun İlkesi "olarak tutarlı" olarak belirlendiyse, belirli bir Azure Cosmos DB koleksiyon sorgulamaları noktası okuma için belirtildiği gibi aynı tutarlılık düzeyi izleyin (yani güçlü, sınırlanmış eskime durumu, oturum veya son). Dizin belge güncelleştirme (yani Ekle, Değiştir, güncelleştirme ve silme Azure Cosmos DB koleksiyonunda belgenin) bir parçası olarak eşzamanlı olarak güncelleştirilir.  Tutarlı dizin oluşturma olası azaltma, tutarlı sorguları yazma performansı destekler. Bu azaltma işlevi sıralanması gerekir benzersiz yolların ve "tutarlılık düzeyi" dir. Tutarlı dizin oluşturma modu "hızlı bir şekilde, hemen sorgu yazma" iş yükleri için tasarlanmıştır.

**Yavaş**: Azure Cosmos DB koleksiyon yani koleksiyonunun işleme kapasitesi kullanıcı isteklere yanıt tam olarak kullanılmaz, sessiz olduğunda dizini zaman uyumsuz olarak güncelleştirilir. Belge alım gerektiren "sorgu daha sonra şimdi alma" iş yükleri için "yavaş" dizin oluşturma modu uygun olabilir. Lütfen verileri ve alınan yavaş dizine tutarsız sonuçlar alabilirsiniz unutmayın. Bu sayı sorgular veya özel bir sorgu sonuçları veri dizine kadar doğru veya repeatable olması garanti edilmez anlamına gelir. Dizini genellikle catch modu ayarlamak içindir. Yavaş dizin - TTL değişikliği dizin alınırken sonuçları bırakılan ve yeniden, böylece bu etkinlik alanlarında beklenmeyen sonuçlara neden olabilir. Müşterilerin çoğu, tutarlı dizin kullanmanız gerekir.

**Hiçbiri**: kendisiyle ilişkilendirilmiş herhangi bir dizin dizini modu "Hiçbiri" ile işaretlenmiş bir koleksiyonu vardır. Bu, Azure Cosmos DB anahtar-değer depolama alanı olarak kullanılan ve belgeleri yalnızca kimliği özelliği tarafından erişilen yaygın olarak kullanılır. 

> [!NOTE]
> Dizin oluşturma ilkesi "Hiçbiri" ile yapılandırma varolan bir dizini bırakma yan etkisi vardır. Erişim alışkanlıklarınıza yalnızca gereksinim duyuyorsanız "id" ve/veya "kendi bağlantı" bunu kullanın.
> 
> 

Aşağıdaki tabloda koleksiyon için yapılandırılan dizin oluşturma modunu (CONSISTENT ve Lazy) ve sorgu isteği için belirtilen tutarlılık düzeyi temel alan sorgular için tutarlılık gösterir. Bu herhangi arabirimi - REST API SDK'ları kullanarak yapılan sorguları için geçerlidir ya da içinden yordamları ve Tetikleyicileri depolanabilir. 

|Tutarlılık|Dizin oluşturma modu: tutarlı|Dizin oluşturma modu: geç|
|---|---|---|
|Güçlü|Güçlü|Nihai|
|Sınırlanmış Eskime Durumu|Sınırlanmış Eskime Durumu|Nihai|
|Oturum|Oturum|Nihai|
|Nihai|Nihai|Nihai|

Azure Cosmos DB koleksiyonunda yok modu dizin ile yapılan sorgular için bir hata döndürür. Sorguları hala yürütülebilir taramaları aracılığıyla açık olarak `x-ms-documentdb-enable-scan` REST API üstbilgisinde veya `EnableScanInQuery` isteği .NET SDK kullanarak seçeneği. ORDER BY gibi bazı sorgu özellikleri ile taramaları olarak desteklenmez `EnableScanInQuery`.

Aşağıdaki tabloda, dizin oluşturma modunu (CONSISTENT Lazy ve hiçbiri) temel alan sorgular için tutarlılık gösterilmektedir EnableScanInQuery belirtildiğinde.

|Tutarlılık|Dizin oluşturma modu: tutarlı|Dizin oluşturma modu: geç|Dizin oluşturma modu: yok|
|---|---|---|---|
|Güçlü|Güçlü|Nihai|Güçlü|
|Sınırlanmış Eskime Durumu|Sınırlanmış Eskime Durumu|Nihai|Sınırlanmış Eskime Durumu|
|Oturum|Oturum|Nihai|Oturum|
|Nihai|Nihai|Nihai|Nihai|

Aşağıdaki kod örnek Göster nasıl tutarlı tüm belge eklemeleri üzerinde dizin ile .NET SDK kullanarak Azure Cosmos DB koleksiyonu oluşturun.

     // Default collection creates a hash index for all string fields and a range index for all numeric    
     // fields. Hash indexes are compact and offer efficient performance for equality queries.

     var collection = new DocumentCollection { Id ="defaultCollection" };

     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

     collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("mydb"), collection);


### <a name="index-paths"></a>Dizin yolları
Azure Cosmos DB JSON belgeleri ve dizin ağaçları modeller ve ilkeleri ağaçtaki yollar için ince ayar olanak tanır. Belgeler içinde hangi yolların dahil veya dizine almasını hariç seçebilirsiniz. Sorgu desenlerine önceden bilindiğinde Bu geliştirilmiş yazma performansı ve senaryolar için alt dizini depolama sunabilir.

Dizin yolları (/) kök ile başlamalı ve genellikle ile bitmelidir? birden fazla olası değerler önek belirten joker işleci. Örneğin, SELECT hizmet için * aileleri F NEREYE F.familyName = "Andersen" /familyName/ için bir dizin yolu içermelidir? koleksiyonun dizini ilkesinde.

Dizin yolları da kullanabilirsiniz * yolları özyinelemeli ön ek altındaki davranışını belirtmek için joker karakter işleci. Örneğin, / yükü / * yükü özellik altında her şeyin dizine almasını dışlamak için kullanılır.

Dizin yolları belirtmek için ortak desenler şunlardır:

| Yol                | Açıklama/kullanım örneği                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /                   | Koleksiyon için varsayılan yolu. Özyinelemeli ve tüm belge ağaca uygular.                                                                                                                                                                                                                                   |
| / prop /?             | Dizin yolu aşağıdaki gibi sorguları sunmak için gerekli (karma veya aralık türleri sırasıyla):<br><br>SELECT FROM koleksiyonu c WHERE c.prop = "değeri"<br><br>SELECT FROM koleksiyonu c WHERE c.prop > 5<br><br>SELECT FROM koleksiyonu c ORDER BY c.prop                                                                       |
| / prop / *             | Belirtilen etiket altındaki tüm yolları için dizin yolu. Aşağıdaki sorgularda ile çalışır<br><br>SELECT FROM koleksiyonu c WHERE c.prop = "değeri"<br><br>SELECT FROM koleksiyonu c WHERE c.prop.subprop > 5<br><br>SELECT FROM koleksiyonu c WHERE c.prop.subprop.nextprop = "değeri"<br><br>SELECT FROM koleksiyonu c ORDER BY c.prop         |
| / özellik / [] /?         | Dizin yolu gerekli yineleme hizmet ve skalerler ["a", "b", "c"] gibi bir dizi sorguları katılmak için:<br><br>SEÇİN etiket etiket IN collection.props WHERE etiketi = "değeri"<br><br>Koleksiyon c birleştirme etiketi IN c.props SELECT etiketinden burada > 5 etiketi                                                                         |
| /props/ [] /subprop/? | Nesne dizileri birleşim sorguları gibi ve dizin yolu gerekli yineleme sunmak için [{subprop: "a"}, {subprop: "b"}]:<br><br>SEÇİN etiket etiket IN collection.props WHERE tag.subprop = "değeri"<br><br>SEÇİN etiket koleksiyonu c birleştirme etiketi IN c.props WHERE tag.subprop = "değeri"                                  |
| / prop/subprop /?     | Dizin yolu sorguları sunmak için gerekli (karma veya aralık türleri sırasıyla):<br><br>SELECT FROM koleksiyonu c WHERE c.prop.subprop = "değeri"<br><br>SELECT FROM koleksiyonu c WHERE c.prop.subprop > 5                                                                                                                    |

> [!NOTE]
> Özel dizin yolları ayarlanırken, size özel yol tarafından gösterilen tüm belge ağacı için varsayılan dizin oluşturma kuralı belirtmek için gerekli olan "/ *". 
> 
> 

Aşağıdaki örnekte, aralık dizin ile belirli bir yol ve özel duyarlılık değeri 20 bayt yapılandırır:

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
Biz yolları belirtme göz ayırdıktan, bir yolu için dizin oluşturma ilkesini yapılandırmak için kullanabileceğiniz seçenekler bakalım. Her yol için bir veya daha fazla dizin tanımları belirtebilirsiniz:

* Veri türü: **dize**, **numarası**, **noktası**, **Çokgen**, veya **LineString** (yalnızca bir giriş veri türü yolu başına başına içerebilir)
* Dizin türü: **karma** (eşitlik sorguları) **aralığı** (eşitlik, aralığı veya Order By sorguları) veya **Spatial** (uzamsal sorguları) 
* Duyarlık: karma dizini için bu 1'den 8 dizeler ve varsayılan numaralarıyla için 3 olarak farklılık gösterir. Bu değer aralığı dizini için -1 (en yüksek duyarlık) olması ve dize veya sayı değerleri için 1-100 (en yüksek duyarlık) arasında farklılık gösterir.

#### <a name="index-kind"></a>Dizin türü
Azure Cosmos DB karma ve aralık dizini (dizeler, sayılar veya her ikisi için yapılandırılmış) her yol için destekler.

* **Karma** verimli eşitlik ve JOIN sorguları destekler. Çoğu kullanım durumları için varsayılan değer olan 3 bayt daha yüksek duyarlık karma dizinleri gerekmez. Veri türü dize veya sayı olabilir.
* **Aralık** verimli eşitlik sorguları, aralık sorguları destekler (kullanarak >, <>, =, < =,! =) ve Order By sorgular. Order By sorguları varsayılan olarak, ayrıca dizin üst sınırından duyarlık (-1) gerektirir. Veri türü dize veya sayı olabilir.

Azure Cosmos DB noktası, çokgen veya LineString veri türleri için belirtilebilir her yol, uzamsal dizin türünü de destekler. Değer belirtilen yolda gibi geçerli bir GeoJSON parçası olmalıdır `{"type": "Point", "coordinates": [0.0, 10.0]}`.

* **Uzamsal** destekler verimli uzamsal (içinde ve uzaklık) sorgular. Veri noktası, çokgen veya LineString olabilir.

> [!NOTE]
> Azure Cosmos DB noktaları, çokgenler ve MultiPoint otomatik dizin oluşturma destekler.
> 
> 

Desteklenen dizin türleri ve olabilirler sorguları örnekleri aşağıda verilmiştir sunmak için kullanılır:

| Dizin türü | Açıklama/kullanım örneği                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Karma       | / Prop / karma? (veya /) aşağıdaki sorguları etkili bir şekilde hizmet için kullanılabilir:<br><br>SELECT FROM koleksiyonu c WHERE c.prop = "değeri"<br><br>Karma/özellik / [] üzerinden /? (veya / veya/özellik /) aşağıdaki sorguları etkili bir şekilde hizmet için kullanılabilir:<br><br>WHERE etiketi seçin etiket koleksiyonu c birleştirme etiketi IN c.props = 5                                                                                                                       |
| Aralık      | / Prop / aralığı? (veya /) aşağıdaki sorguları etkili bir şekilde hizmet için kullanılabilir:<br><br>SELECT FROM koleksiyonu c WHERE c.prop = "değeri"<br><br>SELECT FROM koleksiyonu c WHERE c.prop > 5<br><br>SELECT FROM koleksiyonu c ORDER BY c.prop                                                                                                                                                                                                              |
| Uzamsal     | / Prop / aralığı? (veya /) aşağıdaki sorguları etkili bir şekilde hizmet için kullanılabilir:<br><br>SELECT FROM koleksiyonu c<br><br>WHERE st_dıstance (c.prop, {"tür": "Noktası", "coordinates": [0.0, 10.0]}) < 40<br><br>Etkin noktalarında dizin ile SELECT FROM koleksiyonu c nerede ST_WITHIN(c.prop, {"type": "Polygon",...})--<br><br>Etkin çokgenler üzerinde dizin ile SELECT FROM koleksiyonu c nerede ST_WITHIN({"type": "Point",...}, c.prop)--              |

Sorgularla işleçleri gibi aralığı için varsayılan olarak, bir hata döndürülür > = tarama sorgu hizmet gerekebilir sinyal için hiçbir aralık dizini (tüm duyarlık) olduğunda. Aralık sorguları x-ms-documentdb-enable-tarama üstbilgisinde REST API veya .NET SDK kullanarak EnableScanInQuery isteği seçeneğini kullanarak bir aralık dizin olmadan gerçekleştirilebilir. Sorguda herhangi bir filtre varsa bu Azure Cosmos DB göre filtre uygulamak için dizini kullanın ve sonra herhangi bir hata döndürülür.

Aynı kuralları uzamsal sorguları için geçerlidir. Varsayılan olarak, uzamsal dizin yok ve dizinden sunulabilecek hiçbir bir filtre uzamsal sorguları için bir hata döndürülür. X-ms-documentdb-enable-tarama/EnableScanInQuery kullanarak bir tarama gerçekleştirilebilir.

#### <a name="index-precision"></a>Dizin duyarlık
Dizin duyarlık dizin depolama ek yükü ve sorgu performansı karşılaştırmasını olanak sağlar. Numaraları için -1 ("en") varsayılan duyarlık yapılandırmasını kullanmanızı öneririz. Sayı JSON 8 bayt olduğundan, bu 8 bayt bir yapılandırmaya eşdeğerdir. Kesinlik, 1-7 gibi daha düşük bir değere çekme bazı aralıklarında değerleri aynı dizin girişi eşleyin anlamına gelir. Bu nedenle dizin depolama alanı azaltır, ancak sorgu yürütme daha fazla belgeleri işlemek ve sonuç olarak daha fazla verimlilik yani kullanmak zorunda kalabilirsiniz birimleri isteyin.

Dizin duyarlık yapılandırma dizesi aralıklarıyla daha pratik uygulama vardır. Dizeleri herhangi bir rastgele uzunlukta olabileceği için dizin duyarlık seçimi dize aralığı sorguların performansını etkileyebilir ve gerekli dizin depolama alanı miktarı etkileyebilir. Dize aralığı dizinleri, 1-100 veya -1 ("en") ile yapılandırılabilir. Dize özellikleri Order By sorguları gerçekleştirmek istiyorsanız, karşılık gelen yollar için -1 kesinliğini belirtmeniz gerekir.

Uzamsal dizinler her zaman varsayılan dizin duyarlık tüm türleri için (noktaları, MultiPoint ve çokgenler) kullanın ve kılınmadı olamaz. 

Aşağıdaki örnek, .NET SDK kullanarak bir koleksiyondaki aralığı dizinler için duyarlık artırmak gösterilmektedir. 

**Özel dizin duyarlık ile bir koleksiyon oluşturma**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };

    // Override the default policy for Strings to range indexing and "max" (-1) precision
    rangeDefault.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), rangeDefault);   


> [!NOTE]
> Bir sorgu Order By kullanır ancak aralık dizinine karşı en yüksek duyarlık sorgulanan yolu yok azure Cosmos DB bir hata döndürür. 
> 
> 

Benzer şekilde, yolları tamamen dizine almasını tutulabilir. Sonraki örnek belgeleri bölümünün tamamını (paketini dışlama gösterir bir alt ağacı) dizin kullanarak "*" joker karakter.

    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*" });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);



## <a name="opting-in-and-opting-out-of-indexing"></a>Seçim ve dizin oluşturma dışında kullanmama
Otomatik olarak tüm belge dizini için koleksiyon isteyip istemediğinizi seçebilirsiniz. Varsayılan olarak, tüm belgeler otomatik olarak dizine alınır, ancak devre dışı bırakmak seçebilirsiniz. Dizin oluşturma devre dışı bırakıldığında, belgeleri yalnızca aracılığıyla erişilebilen kendi kendine bağlantılar veya kullanarak sorgular tarafından kimliği

Otomatik kapalı ile dizin oluşturma, yalnızca belirli belgeler için dizin hala seçmeli olarak ekleyebilirsiniz. Buna karşılık, üzerinde dizin otomatik bırakın ve yalnızca belirli belgeleri dışlamak seçmeli olarak seçin. Açık/kapalı yapılandırmaları dizin yararlı yalnızca bir alt belgelerin sorgulanmasını gerek sahip olduğunuzda.

Örneğin, aşağıdaki örnek açıkça kullanarak belge dahil gösterilmektedir [DocumentDB API .NET SDK'sını](https://docs.microsoft.com/en-us/azure/cosmos-db/documentdb-sdk-dotnet) ve [RequestOptions.IndexingDirective](http://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx) özelliği.

    // If you want to override the default collection behavior to either
    // exclude (or include) a Document from indexing,
    // use the RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## <a name="modifying-the-indexing-policy-of-a-collection"></a>Bir dizin oluşturma ilkesini değiştirme
Azure Cosmos DB anında koleksiyonunun dizin oluşturma ilkesi değişiklik yapmasına olanak sağlar. Dizin oluşturma ilkesi Azure Cosmos DB koleksiyonunda bir değişikliğin dizin yolları dizine dahil olmak üzere, şekil, kendi duyarlık yanı sıra, dizin tutarlılık modelinin bir değişiklik neden olabilir. Böylece dizin oluşturma ilkesi değişikliği etkili bir şekilde gerektirir bir eski dizin dönüştürmesi içine yeni bir tane. 

**Çevrimiçi dizin dönüşümleri**

![Dizin oluşturma şeklini – Azure Cosmos DB çevrimiçi dizin dönüşümleri](./media/indexing-policies/index-transformations.png)

Dizin dönüşümleri eski İlkesi dizine belgeleri yeni ilke verimli bir şekilde dönüştürülmeden anlamı çevrimiçi yapılma **yazma kullanılabilirlik veya sağlanan işleme etkilemeden** koleksiyonu. Tutarlılığını okuma ve yazma işlemlerini REST API SDK kullanılarak yapılan veya içinden saklı yordamları ve Tetikleyicileri değil etkilenir dizin dönüştürme sırasında. Bu, olduğunu herhangi bir performans düşüşü veya kapalı kalma süresi uygulamalarınızı değiştirme bir dizin oluşturma ilkesi yaptığınızda anlamına gelir.

Ancak, dizin dönüştürme ilerleme olduğu süre boyunca, sorguları dizin oluşturma modu yapılandırma bağımsız olarak (CONSISTENT veya Lazy) sonunda tutarlı değil. Bu ayrıca tüm arabirimlerinden – REST API SDK'ları, sorgulara uygulanır ve yordamları ve Tetikleyicileri içinden depolanır. Gibi dizin oluşturma, Lazy ile dizin dönüştürme zaman uyumsuz olarak kullanılabilir yedek kaynakları için belirli bir çoğaltma kullanarak çoğaltmalar üzerinde arka planda gerçekleştirilir. 

Dizin dönüşümleri ayrıca yapılan **situ** (yerinde), yani Azure Cosmos DB değil dizin iki kopyasını korumak ve eski diziniyle çıkışı yeni bir değiştirme. Bu, hiçbir ek disk alanı gerekli veya dizin dönüşümleri gerçekleştirirken, koleksiyonlarda tüketilen anlamına gelir.

Dizin oluşturma ilkesini değiştirdiğinizde, eski dizinden yeni bir bağımlı öncelikle dizin oluşturma modu yapılandırmalarında diğer değerler gibi çok şekilde taşımak için değişiklikleri nasıl uygulandığını dahil/yolları, dizin türleri ve Precision bilgisayarlar dışlandı. Her iki eski ve yeni ilkelerinizi tutarlı dizin kullanırsanız, Azure Cosmos DB bir çevrimiçi dizin dönüşümü gerçekleştirir. Dönüştürme işlemi devam ederken, başka bir dizin oluşturma ilkesi değişikliği tutarlı dizin oluşturma modu ile uygulayamazsınız.

Lazy veya hiçbiri dönüştürme sırasında dizin oluşturma modu devam ediyor ancak taşıyabilirsiniz. 

* Lazy için taşıdığınızda, dizin ilke hemen etkili değişiklik ve Azure Cosmos DB dizini zaman uyumsuz olarak yeniden başlatır. 
* None taşıdığınızda, ardından dizin etkili hemen bırakılır. None taşıma yararlı etmekte iptal etmek istediğinizde dönüştürme ve farklı bir dizin oluşturma ilkesi temiz başlat. 

.NET SDK'sı kullanıyorsanız, kullanarak yeni bir dizin oluşturma ilkesi değişikliği kazandırın **ReplaceDocumentCollectionAsync** yöntemi ve kullanarak dizin dönüştürme yüzdesi ilerleme durumunu **IndexTransformationProgress** yanıt özelliğinden bir **ReadDocumentCollectionAsync** çağırın. Diğer SDK ve REST API dizin oluşturma ilkesi değişiklikleri yapmak için eşdeğer özellikleri ve yöntemleri destekler.

Bir koleksiyona ait Lazy tutarlı dizin modundan dizin oluşturma ilkesini değiştirmek nasıl oluşturulduğunu gösteren bir kod parçacığı aşağıda verilmiştir.

**Dizin oluşturma ilkesinden tutarlı yavaş için değiştirme**

    // Switch to lazy indexing.
    Console.WriteLine("Changing from Default to Lazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);


Aşağıda gösterildiği gibi örneğin ReadDocumentCollectionAsync, çağırarak bir dizin dönüşümü ilerlemesini denetleyebilirsiniz.

**Dizin dönüştürme ilerlemesini izlemek**

    long smallWaitTimeMilliseconds = 1000;
    long progress = 0;

    while (progress < 100)
    {
        ResourceResponse<DocumentCollection> collectionReadResponse = await client.ReadDocumentCollectionAsync(
            UriFactory.CreateDocumentCollectionUri("db", "coll"));

        progress = collectionReadResponse.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromMilliseconds(smallWaitTimeMilliseconds));
    }

Bir koleksiyon için dizin hiçbiri modu dizin taşıyarak düşürebilir. Devam eden dönüştürme iptal edip yeni bir tane hemen başlatmak istiyorsanız bu yararlı bir işlemsel aracı olabilir.

**Bir koleksiyon için dizin bırakılıyor**

    // Switch to lazy indexing.
    Console.WriteLine("Dropping index by changing to to the None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

Dizin oluşturma ilkesi değişiklikleri Azure Cosmos DB koleksiyonlarınızı yaptığınızda? En yaygın kullanım örnekleri şunlardır:

* Normal işlem ancak yavaş toplu veri içeri aktarmaları sırasında dizin oluşturma için geri sonbaharda sırasında tutarlı sonuçlar sunar
* Başlangıç yeni kullanarak geçerli Azure Cosmos DB koleksiyonlarınızı özellikleri dizin örn., hangi uzamsal dizin türü gerektiren veya sıralama ölçütü sorgulama Jeo-uzamsal ister / dize aralığı dizin türü gerektiren aralığı sorguları dize
* Elle sıralanması ve zaman içinde değiştirmek için özellikleri seçin
* Sorgu performansı veya depolama tüketilen düşürmek için dizin oluşturma duyarlık ayarlama

> [!NOTE]
> Sürüm ihtiyacınız ReplaceDocumentCollectionAsync kullanarak dizin oluşturma ilkesini değiştirmek için > .NET SDK'ın 1.3.0 =
> 
> Dizin dönüştürme başarıyla tamamlamak yeterli boş depolama alanı kullanılabilir olduğunu koleksiyonda emin olmalısınız. Toplama, depolama kotasını ulaşırsa, dizin dönüştürme duraklatılır. Dizin dönüştürme otomatik olarak devam edecek örneğin depolama alanı kullanılabilir sonra bazı belgeleri silerseniz.
> 
> 

## <a name="performance-tuning"></a>Performans ayarı
DocumentDB API'ları, her işlem için kullanılan dizin depolama alanı gibi performans ölçümleri hakkında bilgi ve verimlilik maliyeti (istek birimleri) sağlar. Bu bilgiler çeşitli dizin oluşturma ilkeleri karşılaştırmak için kullanılabilir ve performans ayarlama.

Depolama kotası ve bir koleksiyon kullanımını denetlemek için HEAD veya GET isteği karşı koleksiyon kaynağı çalıştırın ve x-ms-istek-quota ve x-ms-istek kullanım üstbilgileri inceleyin. .NET SDK'sındaki [DocumentSizeQuota](http://msdn.microsoft.com/library/dn850325.aspx) ve [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) özelliklerinde [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) bu karşılık gelen değerler içeriyor.

     // Measure the document size usage (which includes the index size) against   
     // different policies.
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


Her yazma işlemi dizin yükünü ölçmek için (oluşturma, güncelleştirme veya silme), x-ms-istek-ücret üstbilgi inceleyin (veya eşdeğer [RequestCharge](http://msdn.microsoft.com/library/dn799099.aspx) özelliğinde [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) .NET SDK'sındaki) bu işlemler tarafından tüketilen isteği birim sayısını ölçmek için.

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
Dizin oluşturma ilkesi için şema değişikliği 7 Temmuz 2015 REST API sürümü 2015-06-03 ile kullanılmaya başlanmıştır. SDK sürümlerinde karşılık gelen sınıfları şemayla eşleşecek şekilde yeni uygulamalar vardır. 

Aşağıdaki değişiklikleri JSON belirtiminde uygulanan:

* Dizin oluşturma ilkesi aralığı dizinler için dizeleri destekler
* Her yol birden çok dizin tanımları, her bir veri türü için bir tane olabilir
* Duyarlık dizin 1-8 numaraları, 1-100 dizeleri ve -1 (en yüksek duyarlık) destekler
* Yol kesimleri her yol kaçınmak için çift tırnak gerekmez. Örneğin, / başlık/için bir yol ekleyebilir miyim? yerine / "title" /?
* "Tüm yolları" temsil eden kök yolu olarak temsil edilebilir / * (ek olarak /)

.NET SDK veya sürümünü daha eski 1.1.0 yazılmış özel bir dizin oluşturma ilkesi ile bu hükümleri koleksiyonları kodu varsa, SDK'sı sürüm 1.2.0 taşımak için bu değişiklikler işlenecek uygulama kodunu değiştirmeniz gerekir. Değil, dizin oluşturma ilkesini yapılandıran koduna sahip veya eski bir SDK sürümü kullanmaya devam etmeyi planladığınız, değişiklik gerekmez.

Pratik karşılaştırması için burada REST API sürümü 2015-06-03 kullanılarak yazılmış bir örnek özel dizin oluşturma ilkesini ve bunun yanı sıra önceki sürümü 2015-04-08

**JSON önceki dizin oluşturma ilkesi**

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

**Geçerli dizin oluşturma ilkesi JSON**

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

## <a name="next-steps"></a>Sonraki Adımlar
Dizin ilke yönetimi örnekleri ve Azure Cosmos veritabanı sorgu dili hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin.

1. [DocumentDB API .NET dizin yönetimi kod örnekleri](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
2. [DocumentDB API REST toplama işlemleri](https://msdn.microsoft.com/library/azure/dn782195.aspx)
3. [SQL sorgusu](documentdb-sql-query.md)

