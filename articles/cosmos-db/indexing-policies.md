---
title: Azure Cosmos DB dizinleme ilkeleri | Microsoft Docs
description: Azure Cosmos DB'de dizinleme nasıl çalıştığını anlayın. Yapılandırma ve otomatik dizin oluşturma ve daha yüksek performans için dizin oluşturma ilkesini değiştirme hakkında bilgi edinin.
keywords: Dizin oluşturma, otomatik işleyişi dizin oluşturma, veritabanı dizini oluşturma
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: rafats
ms.openlocfilehash: 240c0e1f39833e4dc4c4ad410f50ff03df0b5734
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072172"
---
# <a name="how-does-azure-cosmos-db-index-data"></a>Azure Cosmos DB dizin verileri nasıl yapar?

Varsayılan olarak, tüm Azure Cosmos DB veri tarihine. Birçok müşteri Azure Cosmos DB, otomatik olarak dizinleme tüm yönlerini işleme izin mutluluk olsa da, özel bir belirtebilirsiniz *dizin oluşturma ilkesi* Azure Cosmos DB'de oluşturma sırasında koleksiyonlar için. Azure Cosmos DB'de dizinleme ilkeleri, daha esnek ve diğer veritabanı platformlarda sunulan ikincil dizinler daha güçlü. Azure Cosmos DB'de tasarlayın ve şema esnekliği ödün vermeden dizini şeklini özelleştirebilirsiniz. 

Azure Cosmos DB'de dizin oluşturmanın nasıl çalıştığını öğrenmek için dizin oluşturma ilkesini yönetirken, ayrıntılı gereksinimlerimi karşılamama dizin depolama yükü, yazma ve sorgu aktarım hızı ve tutarlılık sorgu yapabilirsiniz anlamak önemlidir.  

Aşağıdaki videoda, Azure Cosmos DB Program Yöneticisi Manager Andrew Liu Azure Cosmos DB özelliklerini ve ayarlama ve Azure Cosmos DB kapsayıcınız dizin oluşturma ilkesini yapılandırma dizin otomatik gösterir. 

>[!VIDEO https://www.youtube.com/embed/uFu2D-GscG0]

Bu makalede, biz Kapat Azure Cosmos DB dizinleme ilkeleri, dizin oluşturma ilkesini ve ilişkili dezavantajlarına özelleştirmek nasıl göz atın. 

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlamak mümkün olacaktır:

* Özelliklerini dahil etmek veya dizine elmadan hariç tutmak için nasıl geçersiz?
* Dizin son güncelleştirmeler için nasıl yapılandırabilirim?
* ORDER BY veya aralık sorguları gerçekleştirmek üzere nasıl yapılandırabilirim?
* Bir koleksiyonun dizin oluşturma ilkesini için nasıl bir değişiklik yapıyor?
* Nasıl miyim depolama ve farklı bir dizin oluşturma ilkeleri performansını karşılaştırmak?

## Bir koleksiyonun dizin oluşturma ilkesini özelleştirme <a id="CustomizingIndexingPolicy"></a>  
Varsayılan dizinleme ilkesinin bir Azure Cosmos DB koleksiyonu geçersiz kılarak, depolama, yazma ve sorgu performansı ve sorgu tutarlılık arasındaki dengelemeler özelleştirebilirsiniz. Aşağıdaki durumlara yapılandırabilirsiniz:

* **Dahil etmek veya belgeler ve dizin gelen ve yolları hariç tut**. Hariç tutma veya eklediğinizde veya koleksiyondaki belgeleri değiştirin belirli belgelere dizin içerir. Da ekleyebilir ya da bilinen belirli JSON özellikleri hariç *yolları*, arasında bir dizine dahil belgeler listelenecek. Yollar joker karakter düzenleri içerir.
* **Dizin çeşitli yapılandırma**. Dahil edilen her yol için dizin yolu bir koleksiyon için gerektirir türünü belirtebilirsiniz. Yolun veri, beklenen sorgu iş yükü ve sayısal/dizesi "duyarlık." bağlı dizin türünü belirtebilirsiniz.
* **Dizin güncelleştirme modlarını yapılandırma**. Azure Cosmos DB, üç dizin oluşturma modu destekler: tutarlı, yavaş ve yok. Bir Azure Cosmos DB koleksiyonunda dizin oluşturma ilkesi aracılığıyla dizin oluşturma modları yapılandırabilirsiniz. 

Aşağıdaki Microsoft .NET kod parçacığı, bir koleksiyon oluşturduğunuzda özel bir dizin oluşturma ilkesini ayarlama işlemi gösterilmektedir. Bu örnekte en yüksek duyarlık dizeleri ve sayılar için bir aralık diziniyle ilke ayarladık. Bu ilke, dizeleri ORDER BY sorguları yürütmek için kullanabilirsiniz.

    DocumentCollection collection = new DocumentCollection { Id = "myCollection" };

    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);   


> [!NOTE]
> REST API sürümü 2015-06-03 sürümle birlikte değişen dizin oluşturma ilkesini için JSON şeması. Bu sürümle birlikte, dizin oluşturma ilkesi için JSON şemasını aralığı dizinlerini dizeleri destekler. .NET SDK'sı 1.2.0 ve Java, Python ve Node.js SDK'ları 1.1.0 yeni ilke şemayı destekler. REST API sürümü 2015-04-08 SDK'ın önceki sürümlerini kullanın. Bunlar, önceki şemanın dizin oluşturma ilkesi destekler.
> 
> Varsayılan olarak, Azure Cosmos DB belge içindeki tüm dize özellikleri tutarlı bir karma dizine ile dizinler. Bu belge içindeki tüm sayısal özellik tutarlı bir şekilde bir aralık diziniyle dizinler.  
> 
> 

### <a name="customize-the-indexing-policy-in-the-portal"></a>Portalda dizin oluşturma ilkesini özelleştirme

Azure portalında bir koleksiyonun dizin oluşturma ilkesini değiştirebilirsiniz: 

1. Portalında, Azure Cosmos DB hesabınıza gidin ve ardından, koleksiyonu seçin. 
2. Sol gezinti menüsünde seçin **ayarları**ve ardından **dizin oluşturma ilkesi**. 
3. Altında **dizin oluşturma ilkesi**, dizin oluşturma ilkenizi değiştirin ve ardından **Tamam**. 

### Veritabanı dizini oluşturma modları <a id="indexing-modes"></a>  
Azure Cosmos DB, Azure Cosmos DB koleksiyonunda dizin oluşturma ilkesi aracılığıyla yapılandırabileceğiniz üç dizin oluşturma modu destekler: tutarlı, yavaş ve yok.

**Tutarlı**: bir Azure Cosmos DB koleksiyonunun ilke Consistent, belirli bir Azure Cosmos DB koleksiyonunda sorguları noktası okuma için belirtildiği gibi aynı tutarlılık düzeyi izleyin (güçlü, bağımlı eskime, oturum veya son). Dizin belge Güncelleştirmesi (ekleme, değiştirme, update ve delete belgeye bir Azure Cosmos DB koleksiyonu) bir parçası olarak zaman uyumlu olarak güncelleştirilir.

Tutarlı dizin olası bir azalma, tutarlı sorguları yazma iş çıkarma yeteneğini destekler. Hacmin sıralanması gereken benzersiz yollara işlevi "tutarlılık düzeyi." ise Tutarlı bir dizin oluşturma modu "hızlı bir şekilde hemen sorgu yazma" iş yükleri için tasarlanmıştır.

**Yavaş**: dizini bir Azure Cosmos DB koleksiyonu diğer bir deyişle koleksiyonun aktarım hızı kapasitesine kullanıcı isteklerine hizmet vermek için tam olarak kullanılmaz, deneniyor, olduğunda zaman uyumsuz olarak güncelleştirilir.  Unutmayın, çünkü veri alınan ve yavaş dizini tutarsız sonuçlar alabilirsiniz. Başka bir deyişle, sayısı sorguları veya belirli bir sorgu sonuçları tutarlı veya belirli bir zaman, tekrarlanabilir olmayabilir. 

Genel olarak alınan verilerle oldukça modunda dizinidir. Dizin oluşturma Lazy ile bırakılmalı ve yeniden oluşturulmalıdır dizindeki sonucu yaşam süresi (TTL) değiştirir. Bu sayı ve sorgu sonuçları tutarsız bir süre sağlar. Çoğu Azure Cosmos DB hesapları tutarlı dizin oluşturma modunu kullanmanız gerekir.

**Hiçbiri**: None dizin modu olan ilişkili hiçbir dizin içeren bir koleksiyon. Bu, Azure Cosmos DB anahtar-değer depolama olarak kullanılan ve belgelerin yalnızca kimliği özelliği tarafından erişilen yaygın olarak kullanılır. 

> [!NOTE]
> Dizin oluşturma İlkesi'yle hiçbiri yapılandırma, varolan bir dizini bırakma yan etkisi vardır. Erişim desenleri yalnızca ID gerektir ya da kendi bağlantı varsa bunu kullanın.
> 
> 

Aşağıdaki tabloda, koleksiyon için yapılandırılan dizin oluşturma modunun (Consistent ve Lazy) ve sorgu isteği için belirtilen tutarlılık düzeyini temel alan sorgular için tutarlılık gösterilmektedir. Bu herhangi bir arabirim kullanarak yapılan sorgular için geçerlidir: REST API, SDK'ları, veya içinden saklı yordamlar ve tetikleyiciler. 

|Tutarlılık|Dizin oluşturma modu: tutarlı|Dizin oluşturma modu: yavaş|
|---|---|---|
|Güçlü|Güçlü|Nihai|
|Sınırlanmış eskime durumu|Sınırlanmış eskime durumu|Nihai|
|Oturum|Oturum|Nihai|
|Nihai|Nihai|Nihai|

Azure Cosmos DB dizinleme modu None olan koleksiyonlarda yapılan sorgular için bir hata döndürür. Sorguları hala çalıştırılabilir taramalar aracılığıyla açık olarak **x-ms-documentdb-enable-tarama** üst bilgisinde REST API veya **EnableScanInQuery** seçeneği .NET SDK kullanarak istek. ORDER BY gibi bazı sorgu özellikleri ile taramalar olarak desteklenmez **EnableScanInQuery**.

Tutarlılık dizin oluşturma modunu (Consistent, Lazy ve hiçbiri) temel alan sorguları için aşağıdaki tabloda gösterilmektedir, **EnableScanInQuery** belirtilir.

|Tutarlılık|Dizin oluşturma modu: tutarlı|Dizin oluşturma modu: yavaş|Dizin oluşturma modu: yok|
|---|---|---|---|
|Güçlü|Güçlü|Nihai|Güçlü|
|Sınırlanmış eskime durumu|Sınırlanmış eskime durumu|Nihai|Sınırlanmış eskime durumu|
|Oturum|Oturum|Nihai|Oturum|
|Nihai|Nihai|Nihai|Nihai|

Aşağıdaki kod örneği Göster nasıl oluşturacağınız bir Azure Cosmos DB koleksiyonu üzerinde tüm belgeyi eklemeler tutarlı dizin oluşturma ile .NET SDK kullanarak.

     // Default collection creates a Hash index for all string fields and a Range index for all numeric    
     // fields. Hash indexes are compact and offer efficient performance for equality queries.

     var collection = new DocumentCollection { Id ="defaultCollection" };

     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

     collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("mydb"), collection);


### <a name="index-paths"></a>Dizin yolları
Azure Cosmos DB, JSON belgelerinin ve dizin ağaçları modeller. Ağaçtaki yolları için ilkeleri ayarlayabilirsiniz. İçindeki belgeler, dahil etmek veya dizine elmadan hariç tutmak için yollar seçebilirsiniz. Bu geliştirilmiş yazma performansını ve sorgu desenleri önceden bilinmektedir senaryoları için daha düşük dizin depolaması sunabilir.

Dizin yolları (/) kök ile başlayın ve genellikle ile biter? joker karakter işleci. Bu, birden çok olası değerler önek olduğunu gösterir. Örneğin, SELECT hizmet için * aileleri F nerede F.familyName = "Andersen" /familyName/ için farklı bir dizin yolu içermesi gerekir? koleksiyonun dizini ilkesinde.

Dizin yolları da kullanabilirsiniz \* yolları yinelemeli olarak ön ek altındaki davranışını belirtmek için joker karakter işleci. Örneğin, / yükünün / * yükü özelliği altında her şeyi dizine elmadan hariç tutmak için kullanılabilir.

Dizin yolları belirtmek için ortak desenler şunlardır:

| Yol                | Açıklama/kullanım örneği                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /                   | Koleksiyon için varsayılan yolu. Özyinelemeli ve tüm belgeyi ağaca uygular.                                                                                                                                                                                                                                   |
| / prop /?             | Dizin yolu sorguları aşağıdaki gibi gerekli (türleriyle, karma veya aralığı sırasıyla):<br><br>SELECT FROM koleksiyon c WHERE c.prop = "değer"<br><br>SELECT FROM koleksiyon c WHERE c.prop > 5<br><br>Koleksiyon c ORDER BY c.prop seçin                                                                       |
| / prop / *             | Belirtilen etiket altında tüm yolları için dizin yolu. Aşağıdaki sorgular ile çalışır<br><br>SELECT FROM koleksiyon c WHERE c.prop = "değer"<br><br>SELECT FROM koleksiyon c WHERE c.prop.subprop > 5<br><br>SELECT FROM koleksiyon c WHERE c.prop.subprop.nextprop = "değer"<br><br>Koleksiyon c ORDER BY c.prop seçin         |
| / Özellikler / [] /?         | Dizin yolu, yineleme sunmak ve skalerler ["a", "b", "c"] gibi bir dizi sorguları katılmak için gerekli:<br><br>WHERE etiketi seçin etiketi etiketi IN collection.props = "değer"<br><br>Koleksiyon c birleşim etiketi IN c.props etiketini seçin > 5 burada etiketi                                                                         |
| [] /subprop/ /props/? | Dizin yolu gerekli yineleme yapacak ve nesne dizileri sorguları birleştirme gibi [{subprop: "a"}, {subprop: "b"}]:<br><br>WHERE tag.subprop seçin etiketi etiketi IN collection.props = "değer"<br><br>WHERE tag.subprop seçin etiketi koleksiyon c birleşim etiketi IN c.props = "değer"                                  |
| / prop/subprop /?     | Dizin yolu sorguları gerekli (türleriyle, karma veya aralığı sırasıyla):<br><br>SELECT FROM koleksiyon c WHERE c.prop.subprop = "değer"<br><br>SELECT FROM koleksiyon c WHERE c.prop.subprop > 5                                                                                                                    |

> [!NOTE]
> Özel dizin yolları ayarladığınızda, özel yolu tarafından belirtilen tüm belge ağacı için varsayılan dizin oluşturma kuralı belirtmeniz gerekli "/ *". 
> 
> 

Aşağıdaki örnek, bir özel duyarlık değeri 20 bayt aralığı dizin ile belirli bir yol yapılandırır:

```
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
```

Dizin oluşturma işlemi için bir yol eklendiğinde, rakam ve bu yolların içinde dizeleri dizine eklenir. Bu nedenle yalnızca dize dizin oluşturma tanımladığınız olsa da, Azure Cosmos DB de sayılar için varsayılan tanımı ekler. Diğer bir deyişle, Azure Cosmos DB için dizin oluşturma ilkesini yolu çıkarma özelliğine sahiptir, ancak belirli bir yoldan dışlama türünde değil. Her iki yol için belirtilen bir dizin örneği, yalnızca not verilmiştir (yolu = "/ *" ve yol = "/\"attr1\"/?") ancak sayı veri türü sonucu de eklenir.

```
var indices = new[]{
                new IncludedPath  {
                    Indexes = new Collection<Index>
                    {
                        new RangeIndex(DataType.String) { Precision = 3 }// <- note: only 1 index specified
                    },
                    Path =  "/*"
                },
                new IncludedPath  {
                    Indexes = new Collection<Index>
                    {
                        new RangeIndex(DataType.String) { Precision = 3 } // <- note: only 1 index specified
                    },
                    Path =  "/\"attr1\"/?"
                }
            };...

            foreach (var index in indices)
            {
                documentCollection.IndexingPolicy.IncludedPaths.Add(index);
            }
```

Dizin oluşturma sonucu:

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*",
            "indexes": [
                {
                    "kind": "Range",
                    "dataType": "String",
                    "precision": 3
                },
                {
                    "kind": "Range",
                    "dataType": "Number",
                    "precision": -1
                }
            ]
        },
        {
            "path": "/\"attr\"/?",
            "indexes": [
                {
                    "kind": "Range",
                    "dataType": "String",
                    "precision": 3
                },
                {
                    "kind": "Range",
                    "dataType": "Number",
                    "precision": -1
                }
            ]
        }
    ],
}
```

### <a name="index-data-types-kinds-and-precisions"></a>Dizin veri türleri, tür ve Precision bilgisayarlar
Yolu için dizin oluşturma ilkesini yapılandırırken birçok seçeneğiniz vardır. Her yol için bir veya daha fazla dizin tanımları belirtebilirsiniz:

* **Veri türü**: dize, sayı, nokta, çokgen veya LineString (yol başına veri türü başına yalnızca bir girdi içerebilir).
* **Dizin türü**: karma (eşitlik sorguları), aralığı (eşitlik, aralığı veya ORDER BY sorguları) veya uzamsal (uzamsal sorgular).
* **Duyarlık**: için bir karma dizine, 8 hem dize hem de sayı 1'den değişir bu. Varsayılan değer 3'tür. Aralık dizin için bu değer -1 (en yüksek duyarlık) olabilir. 1 ile 100 (en yüksek duyarlık) dize veya sayı değerlerini arasındaki farklılık gösterebilir.

#### <a name="index-kind"></a>Dizin türü
Azure Cosmos DB, dize veya sayı veri türleri için yapılandırılmış her bir yol veya her ikisi için karma dizine ve aralık dizini türlerini destekler.

* **Karma** verimli eşitlik ve JOIN sorgularını destekler. Kullanım örnekleri için varsayılan değer 3 bayt daha yüksek bir duyarlık karma dizinler gerekmez. Veri türü dize veya sayı olabilir.
* **Aralık** verimli eşitlik sorguları, aralık sorguları destekler (kullanarak >, <>, =, < =,! =) ve ORDER BY sorgular. ORDER By sorguları varsayılan olarak, ayrıca maksimum dizin duyarlık (-1) gerektirir. Veri türü dize veya sayı olabilir.

Azure Cosmos DB, nokta, çokgen veya LineString veri türleri için belirtilebilir her yol için uzamsal dizin türü de destekler. Değer belirtilen yolda gibi geçerli bir GeoJSON parçası olmalıdır `{"type": "Point", "coordinates": [0.0, 10.0]}`.

* **Uzamsal** destekler verimli uzamsal (içinde ve uzaklık) sorgular. Veri türü, nokta, çokgen veya LineString olabilir.

> [!NOTE]
> Azure Cosmos DB, otomatik dizin oluşturma noktası Çokgen ve LineString veri türlerini destekler.
> 
> 

Desteklenen dizin türleri ve örnekleri olabilirler sorgular şunlardır sunmak için kullanılır:

| Dizin türü | Açıklama/kullanım örneği                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Karma       | / Prop / karma? (veya /) aşağıdaki sorguları verimli bir şekilde sunmak için kullanılabilir:<br><br>SELECT FROM koleksiyon c WHERE c.prop = "değer"<br><br>Karma üzerinden/özellikler / [] /? (veya / veya/Özellikler /) aşağıdaki sorguları verimli bir şekilde sunmak için kullanılabilir:<br><br>WHERE etiketi seçin etiketi koleksiyon c birleşim etiketi IN c.props = 5                                                                                                                       |
| Aralık      | / Prop / aralığı? (veya /) aşağıdaki sorguları verimli bir şekilde sunmak için kullanılabilir:<br><br>SELECT FROM koleksiyon c WHERE c.prop = "değer"<br><br>SELECT FROM koleksiyon c WHERE c.prop > 5<br><br>Koleksiyon c ORDER BY c.prop seçin                                                                                                                                                                                                              |
| Uzamsal     | / Prop / aralığı? (veya /) aşağıdaki sorguları verimli bir şekilde sunmak için kullanılabilir:<br><br>SELECT FROM c koleksiyonu<br><br>WHERE ST_DISTANCE (c.prop, {"type": "Nokta", "koordinatları": [0.0, 10.0]}) < 40<br><br>Etkin noktalarında dizin ile SELECT FROM koleksiyon c burada ST_WITHIN(c.prop, {"type": "Polygon",...})--<br><br>Etkin çokgenler üzerinde dizin ile SELECT FROM koleksiyon c burada ST_WITHIN({"type": "Point",...}, c.prop)--              |

Sorgu işleçleri gibi aralığı için varsayılan olarak, hata döndürülür > = tarama sorgu sunmak gerekli olabilir sinyal yok aralık dizini (herhangi bir hassasiyet) olduğunda. Aralık sorguları gerçekleştirilebilir olmadan bir aralık dizini kullanarak **x-ms-documentdb-enable-tarama** üst bilgisinde REST API veya **EnableScanInQuery** seçeneği .NET SDK kullanarak istek. Azure Cosmos DB dizine göre filtrelemek için kullanabileceğiniz sorgusunda herhangi bir filtre varsa, hata döndürülür.

Aynı kurallara uzamsal sorgular için geçerlidir. Varsayılan olarak, uzamsal dizin yok ve dizinden sunulabilen diğer filtre varsa, uzamsal sorgular için bir hata döndürülür. Bir tarama bunlar gerçekleştirilebilir kullanarak **x-ms-documentdb-enable-tarama** veya **EnableScanInQuery**.

#### <a name="index-precision"></a>Dizin duyarlık
Gereksinimlerimi karşılamama dizin depolama yükü ve sorgu performansını sağlamak için dizin duyarlık kullanabilirsiniz. Sayılar için varsayılan duyarlık yapılandırma-1 (maksimum) kullanmanızı öneririz. Sayı 8 baytlık JSON olduğundan, bu 8 baytlık bir yapılandırmaya eşdeğerdir. Bazı aralıklar dahilinde değerleri aynı eşleyin anlamına gelir, duyarlığı, 1 ile 7 arasında gibi daha düşük bir değer seçme giriş dizini. Bu nedenle, dizin depolama alanını azaltmak, ancak sorgu yürütme daha fazla belgeleri işlemek zorunda kalabilirsiniz. Sonuç olarak, daha fazla üretilen iş istek birimi kullanıyor.

Dizini duyarlılık yapılandırmasını dize aralıklarıyla daha pratik uygulama vardır. Dizeleri herhangi bir rastgele uzunluktaki olabileceğinden, dizin duyarlık seçimi dize aralığı sorguların performansını etkileyebilir. Ayrıca, gerekli olan dizin depolama alanı miktarı da etkileyebilir. Dizinleri dizesi aralık 1 ile 100 veya -1 (maksimum) ile yapılandırılabilir. Dize özellikleri ORDER BY sorguları gerçekleştirmek istiyorsanız, bir duyarlık karşılık gelen yollarla için-1 belirtmeniz gerekir.

Uzaysal dizinler, her zaman varsayılan dizini duyarlık tüm türleri için (Point, LineString ve Çokgen) kullanın. Uzaysal dizinler için varsayılan dizin duyarlık geçersiz kılınamaz. 

Aşağıdaki örnek, .NET SDK'sını kullanarak bir koleksiyon aralığı dizinlerde Duyarlığı artırmak gösterilmektedir. 

**Özel dizin kesinliği ile bir koleksiyon oluşturun**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };

    // Override the default policy for strings to Range indexing and "max" (-1) precision
    rangeDefault.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), rangeDefault);   


> [!NOTE]
> Bir sorgu ORDER BY kullanır ancak karşı en yüksek duyarlık sorgulanan yoluyla bir aralık dizini yok, azure Cosmos DB, bir hata döndürür. 
> 
> 

Benzer şekilde, tamamen yolları dizine elmadan hariç tutabilirsiniz. Sonraki örnek, belgelerin tamamını bir bölümü hariç tutmak gösterilmektedir (bir *alt ağacı*) kullanarak dizin gelen \* joker karakter işleci.

    var excluded = new DocumentCollection { Id = "excludedPathCollection" };
    excluded.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    excluded.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*" });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);



## <a name="opt-in-and-opt-out-of-indexing"></a>Kabul et ve dışında dizin oluşturma işlemi
Tüm belgelerin otomatik olarak dizinini koleksiyonu isteyip istemediğinizi seçebilirsiniz. Varsayılan olarak, tüm belgelerin otomatik olarak dizine alınır, ancak otomatik dizin oluşturma devre dışı kapatabilirsiniz. Dizin oluşturmayı devre dışı bırakıldığında, belgelerin yalnızca aracılığıyla erişilebilen kendi kendine bağlantılar veya belge kullanarak sorgular tarafından kimliği

Otomatik kapalı dizin oluşturma ile yalnızca belirli belgeler dizine seçmeli olarak yine de ekleyebilirsiniz. Buna karşılık, dizin oluşturmayı otomatik bırakın ve belirli belgelere dışlanacak seçerek. Açık/kapalı yapılandırmaları dizin yararlı belgelerin sorgulanmasını gereken yalnızca bir alt sahip olduğunuzda.

Aşağıdaki örnek bir belge kullanarak açıkça içerecek şekilde gösterilmektedir [SQL API .NET SDK'sı](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet) ve [RequestOptions.IndexingDirective](http://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx) özelliği.

    // If you want to override the default collection behavior to either
    // exclude (or include) a document in indexing,
    // use the RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## <a name="modify-the-indexing-policy-of-a-collection"></a>Bir koleksiyonun dizin oluşturma ilkesini değiştirme
Azure Cosmos DB'de hareket halindeyken koleksiyonunun dizin oluşturma ilkesi değişiklik yapabilirsiniz. Dizin oluşturma ilkesi bir Azure Cosmos DB koleksiyonu üzerinde bir değişikliğin dizini şeklinde bir değişiklik neden olabilir. Değişiklik sıralanabilir yolları, kendi duyarlık ve dizin tutarlılık modelini etkiler. Dizin oluşturma ilkesi etkili bir şekilde bir değişiklik bir dönüşümünü eski dizinin yeni bir dizin gerektirir. 

**Çevrimiçi dizin dönüşümleri**

![Dizin oluşturmanın nasıl çalıştığını – Azure Cosmos DB çevrimiçi dizin dönüşümleri](./media/indexing-policies/index-transformations.png)

Dizin dönüşümleri çevrimiçi hale getirilir. Yani her yeni ilke eski ilkeyi dizine belgeleri verimli bir şekilde dönüştürülür *yazma kullanılabilirliği veya sağlanan aktarım hızı etkilemeden* koleksiyon. Tutarlılığını okuma ve yazma işlemleri, SDK, REST API kullanarak yapılan veya içinden saklı yordamları ve Tetikleyicileri etkilenmez dizin dönüştürme sırasında. Performans düşüşü veya yoktur uygulamalarınıza kapalı kalma süresi bir dizin oluşturma ilkesini değişiklik yaptığınızda.

Ancak, sorgular dizini dönüşümdür ilerleme süre boyunca, dizin oluşturma modu yapılandırma bağımsız olarak (Consistent veya Lazy) son tutarlılık sağlar. Bu da sorgular için tüm arabirimlerden geçerlidir: REST API, SDK ve içinde saklı yordamlar ve tetikleyiciler. Gibi dizin oluşturma, Lazy ile dizin dönüştürme zaman uyumsuz olarak arka planda çoğaltmalarındaki belirli bir yineleme için kullanılabilir yedek kaynakları kullanarak gerçekleştirilir. 

Dizin dönüşümleri yerinde da yapılır. Azure Cosmos DB, dizin ve eski dizinin kullanıma takas iki kopyasını yeni bir tane ile korumak değil. Bu, hiçbir ek disk alanı gerekli veya dizin dönüşümleri meydana gelirken, koleksiyonlarında tüketilen anlamına gelir.

Dizin oluşturma ilkesini değiştirdiğinizde, değişiklikler yeni birincil dizin oluşturma modu yapılandırmalarına göre eski dizinden taşımak için uygulanır. Dizin oluşturma modu yapılandırmaları, diğer değerler dahil edilen/Dışlanan yollar ve dizin tür Precision bilgisayarlar gibi daha büyük bir rol oynar. 

Azure Cosmos DB, eski ve yeni ilkelerinizi hem tutarlı dizin kullanırsanız, bir çevrimiçi dizin dönüşümü gerçekleştirir. Dönüştürme işlemi devam ederken, tutarlı bir dizin oluşturma moduna sahip başka bir dizin oluşturma ilkesi değişikliği uygulanamıyor. Ancak, Lazy veya yok ediyor dizin oluşturma modu bir dönüştürme sırasında taşıyabilirsiniz: 

* Lazy için taşıdığınızda, dizin ilke değişikliği hemen etkili olur. Azure Cosmos DB, dizin oluşturma zaman uyumsuz olarak yeniden başlatılır. 
* Hiçbiri taşıdığınızda, dizin hemen bırakıldı. Hiçbiri taşıma, devam eden dönüştürme iptal edin ve farklı bir dizin oluşturma ilkesi ile yeni başlangıç istediğinizde yararlıdır. 

Aşağıdaki kod parçacığı, bir koleksiyonun yavaş dizin oluşturma modu için tutarlı bir dizin oluşturma modundan dizin oluşturma ilkesini değiştirme işlemi gösterilmektedir. .NET SDK'sı kullanıyorsanız, yeni kullanarak bir dizin oluşturma ilkesi değişiklik başlatabilir **ReplaceDocumentCollectionAsync** yöntemi.

**Lazy Consistent gelen dizin oluşturma ilkesini değiştirme**

    // Switch to Lazy indexing mode.
    Console.WriteLine("Changing from Default to Lazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);

**Dizin dönüşümünün ilerlemesini İzle**

Kullanarak, tutarlı bir dizin için dizin dönüşümü yüzdesi ilerlemesini izleyebilirsiniz **IndexTransformationProgress** yanıt özelliğinden bir **ReadDocumentCollectionAsync** çağırın. Diğer SDK'lar ve REST API, dizin oluşturma ilkesi değişiklikler yapmak için eşdeğer özellikleri ve yöntemleri destekler. Tutarlı bir dizine bir dizin dönüşümü ilerlemesini çağırarak denetleyebilirsiniz **ReadDocumentCollectionAsync**: 

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
> * **IndexTransformationProgress** tutarlı bir dizine dönüştürürken özelliği uygulanabilir. Kullanım **ResourceResponse.LazyIndexingProgress** yavaş bir dizine dönüştürmeleri izleme özelliği.
> * **IndexTransformationProgress** ve **LazyIndexingProgress** özellikleri yalnızca koleksiyon bölümlenmemiş, diğer bir deyişle, bir bölüm anahtarı oluşturulan bir koleksiyon için doldurulur.
>

Bir koleksiyon için dizin yok modu dizin taşıyarak bırakabilirsiniz. Devam eden dönüştürme iptal edin ve yeni bir tane hemen başlatmak istiyorsanız, bu yararlı bir işletimsel aracı olabilir.

**Bir koleksiyonun dizini bırakın**

    // Switch to Lazy indexing mode.
    Console.WriteLine("Dropping index by changing to to the None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

Dizin oluşturma ilkesi değişiklikleri, Azure Cosmos DB koleksiyonlarına yaptığınızda? En yaygın kullanım örnekleri şunlardır:

* Normal işlem sırasında tutarlı sonuçlar hizmet, ancak dizin oluşturma moduna geç toplu veri içeri aktarımlarını sırasında geri döner.
* Geçerli Azure Cosmos DB koleksiyonunda yeni bir dizin oluşturma özellikleri kullanmaya başlayın. Örneğin, Jeo-uzamsal sorgulama, uzamsal dizin türü gerektiren kullanma / aralığı dizin türü dize gerektiren aralık sorguları dize ORDER BY.
* Sıralanacak özelliklerini el-seçin ve bunları zamanla değişir.
* Sorgu performansını artırmak için veya depolama miktarı azaltmak için dizin oluşturma duyarlık ayarlayın.

> [!NOTE]
> Dizin oluşturma ilkesini kullanarak değiştirmek için **ReplaceDocumentCollectionAsync**, 1.3.0 veya .NET SDK'sı daha sonraki bir sürümü kullanmanız gerekir.
> 
> Dizin dönüştürme başarıyla tamamlamak yeterli boş depolama alanı kullanılabilir olduğunu koleksiyonunda emin olun. Toplama, depolama kotasını ulaşırsa, dizin dönüşümü duraklatıldı. Örneğin, depolama alanı kullanılabilir olduğunda bazı belgeler silerseniz dizin dönüştürme otomatik olarak sürdürür.
> 
> 

## <a name="performance-tuning"></a>Performans ayarı
SQL API'leri, kullanılan dizin depolaması ve her işlem için aktarım hızı maliyeti (istek birimleri) gibi performans ölçümlerini hakkında bilgi sağlar. Çeşitli dizin oluşturma ilkeleri karşılaştırmak için bu bilgileri kullanın ve performans ayarlama.

Çalıştırma depolama kotası ve bir koleksiyon kullanımını denetlemek için bir **baş** veya **alma** koleksiyonu kaynağı isteği. Daha sonra incelemek **x-ms-isteği-quota** ve **x-ms-istek kullanım** üstbilgileri. .NET SDK'sındaki [DocumentSizeQuota](http://msdn.microsoft.com/library/dn850325.aspx) ve [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) özelliklerinde [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) bu karşılık gelen değerleri içerir.

     // Measure the document size usage (which includes the index size) against   
     // different policies.
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


Her yazma işlemi dizin oluşturma ek yükü ölçmek için (oluşturma, güncelleştirme veya silme) İnceleme **x-ms-istek-ücretsiz olarak** üst bilgisi (veya eşdeğer [RequestCharge](http://msdn.microsoft.com/library/dn799099.aspx) özelliğinde [ ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) .NET SDK'sındaki) bu işlemleri tarafından kullanılan istek birimleri sayısını ölçmek için.

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

## <a name="changes-to-the-indexing-policy-specification"></a>Dizin oluşturma ilkesi belirtimi değişiklikler
Dizin oluşturma ilkesi için bir şema değişikliği 7 Temmuz 2015 sürümü 2015-06-03 REST API ile kullanıma sunulmuştur. SDK sürümlerinde karşılık gelen sınıflar şemasıyla eşleşmesi için yeni uygulamalara sahip. 

JSON belirtiminde uygulanan aşağıdaki değişiklikler:

* Dizin oluşturma ilkesi aralığı dizinler için dizeleri destekler.
* Her yol, birden çok dizin tanımı olabilir. Her veri türü için bir tane olabilir.
* Duyarlık dizin oluşturma, 1 ile 8 sayılar için 1 ile 100 dizeleri ve -1 (en yüksek duyarlık) destekler.
* Yol kesimleri, her bir yol atlamak için bir çift tırnak gerekmez. Örneğin, bir yolunu ekleyebilirsiniz   **/başlık /?** yerine **/ "title" /?**.
* "Tüm yolları" temsil eden kök yol olarak gösterilen **/ \*** (ek olarak **/**).

SDK sürümü 1.2.0 olarak güncelleştirilir, taşımak için .NET SDK sürüm 1.1.0 veya önceki bir sürümü ile yazılmış özel bir dizin oluşturma ilkesi ile bu hükümleri koleksiyonları kodunuz varsa, bu değişiklikleri işlemek için uygulama kodunuzu değiştirmeniz gerekir. SDK'ın önceki bir sürümünü kullanmaya devam etmeyi planlıyorsanız, değişiklik gerekmez veya kodunuz yoksa, dizin oluşturma ilkesini yapılandırır.

Pratik bir karşılaştırması için REST API sürümü 2015-06-03, önceki REST API sürümü 2015-04-08 kullanılarak yazılan aynı dizin oluşturma ilkesini ardından kullanılarak yazılan özel bir dizin oluşturma ilkesini örneği aşağıda verilmiştir.

**Geçerli dizin oluşturma ilkesi JSON (REST API sürümü 2015-06-03)**

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


**Daha önce dizin oluşturma ilkesi JSON (REST API sürümü 2015-04-08)**

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
Dizin ilke yönetimi örnekleri ve Azure Cosmos DB sorgu dili hakkında daha fazla bilgi edinmek için aşağıdaki bağlantılara bakın:

* [SQL API .NET dizin yönetimi kod örnekleri](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
* [SQL API REST toplama işlemleri](https://msdn.microsoft.com/library/azure/dn782195.aspx)
* [SQL ile sorgulama](sql-api-sql-query.md)

