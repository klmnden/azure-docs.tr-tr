---
title: Azure Cosmos veritabanı Jeo-uzamsal verilerle çalışma | Microsoft Docs
description: Oluşturma, dizin ve Azure Cosmos DB ve SQL API'yi uzamsal nesneleriyle sorgu anlayın.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/20/2017
ms.author: sngun
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 893b985514f4c812da673a90fc40148e8ac9ce81
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34611376"
---
# <a name="working-with-geospatial-and-geojson-location-data-in-azure-cosmos-db"></a>Jeo-uzamsal ve Azure Cosmos veritabanı GeoJSON konum verileri ile çalışma
Bu makalede Jeo-uzamsal işlevine bir giriş olduğunu [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Bu okuduktan sonra aşağıdaki soruları yanıtlayın mümkün olacaktır:

* Azure Cosmos DB'de nasıl uzamsal veri depoluyor?
* Azure Cosmos DB'de SQL ve LINQ Jeo-uzamsal verileri nasıl sorgulama yapabilirsiniz?
* Nasıl etkinleştirmek veya Azure Cosmos DB'de uzamsal dizin oluşturmayı devre dışı?

Bu makalede, SQL API'yi uzamsal verilerle çalışmak gösterilmiştir. Bu bkz [GitHub proje](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) kod örnekleri için.

## <a name="introduction-to-spatial-data"></a>Uzamsal veri giriş
Uzamsal veri konumu ve Şekil alanı nesnelerin açıklar. Çoğu uygulamada bu nesneler dünya ve Jeo-uzamsal verileri üzerinde karşılık gelir. Uzamsal veriler, bir kişi, ilgilendiğiniz bir yerde veya bir şehir veya bir lake sınırını konumunu temsil etmek için kullanılabilir. Ortak kullanım durumları genellikle yakınlık sorguları, örneğin içeren, "tüm kafeterya my geçerli konumu bulun." 

### <a name="geojson"></a>GeoJSON
Dizin oluşturma ve kullanma temsil Jeo-uzamsal noktası verileri Sorgulama Azure Cosmos DB destekler [GeoJSON belirtimi](https://tools.ietf.org/html/rfc7946). Böylece depolanabilir ve herhangi bir özel araçlar veya kitaplıkları Azure Cosmos DB kullanarak sorgulanan GeoJSON veri yapıları her zaman geçerli JSON, nesneleridir. Azure Cosmos DB SDK'ları yardımcı sınıfları ve kolaylaştıran uzamsal verilerle çalışmak yöntemler sağlar. 

### <a name="points-linestrings-and-polygons"></a>Noktaları, MultiPoint ve çokgenler
A **noktası** alanı tek bir konumda gösterir. Jeo-uzamsal verileri bir noktayı Market, bir bilgi noktası, bir otomobil ya da bir şehir sokak adresi olabilir tam konumu temsil eder.  Bir noktayı kendi koordinat kullanarak GeoJSON (ve Azure Cosmos DB) çifti veya boylam ve enlem temsil edilir. Bir noktası için JSON örnek aşağıda verilmiştir.

**Azure Cosmos DB noktaları**

```json
{
    "type":"Point",
    "coordinates":[ 31.9, -4.8 ]
}
```

> [!NOTE]
> GeoJSON belirtimi boylam belirtir ilk ve enlem ikinci. Gibi diğer eşleme uygulamalarda boylam ve enlem açıları ve derece cinsinden temsil. Boylam değerleri Meridyeninden ölçülür ve -180 derece ve 180.0 derece arasında ve enlem değerleri ekvatora ölçülür ve-90.0 derece ve 90.0 derece arasında. 
> 
> Azure Cosmos DB 84 WGS başvuru sistemi belirtildiği şekilde koordinatları yorumlama. Koordinat başvuru sistemleri hakkında daha fazla bilgi için aşağıya bakın.
> 
> 

Bu bir Azure Cosmos DB belgesinde konum verileri içeren bir kullanıcı profili bu örnekte gösterildiği gibi katıştırılabilen:

**Azure Cosmos DB içinde depolanan konumla profilini kullan**

```json
{
    "id":"cosmosdb-profile",
    "screen_name":"@CosmosDB",
    "city":"Redmond",
    "topics":[ "global", "distributed" ],
    "location":{
        "type":"Point",
        "coordinates":[ 31.9, -4.8 ]
    }
}
```

Noktalarına ek olarak, GeoJSON MultiPoint ve çokgenler destekler. **MultiPoint** iki veya daha çok puan alan ve bunları bağlamak çizgi dilimleri bir dizi temsil eder. Jeo-uzamsal verileri MultiPoint Otoyollar veya rivers göstermek için yaygın olarak kullanılır. A **Çokgen** kapalı LineString forms bir sınırı bağlı noktalarının. Çokgenler yaygın olarak Göller gibi doğal durum oluşumlarıyla veya siyasi daireleri şehirler ve durumlar gibi göstermek için kullanılır. Burada, Azure Cosmos veritabanı çokgenin bir örnek verilmiştir. 

**GeoJSON'daki çokgenler**

```json
{
    "type":"Polygon",
    "coordinates":[ [
        [ 31.8, -5 ],
        [ 31.8, -4.7 ],
        [ 32, -4.7 ],
        [ 32, -5 ],
        [ 31.8, -5 ]
    ] ]
}
```

> [!NOTE]
> Geçerli çokgenler için sağlanan son koordinat çifti kapalı bir şekil oluşturmak için birinci ile aynı olması gerektiğini GeoJSON belirtilmesini gerektiriyor.
> 
> Çokgen içindeki noktaları yönünün sırayla belirtilmelidir. Belirtilen saat yönünde sırayla Çokgen bölge içindeki tersini temsil eder.
> 
> 

GeoJSON Point, LineString ve Çokgen ek olarak, ayrıca isteğe bağlı özellikler coğrafi konum ilişkilendirmek nasıl yanı sıra birden çok Jeo-uzamsal konumları grubuna nasıl için gösterimi belirtir. bir **özelliği**. Bu nesneler geçerli JSON olduğundan, bunlar tüm depolanabilir ve Azure Cosmos DB'de işlenebilir. Ancak Azure Cosmos DB yalnızca noktalarının otomatik dizin oluşturma işlemi destekler.

### <a name="coordinate-reference-systems"></a>Koordinat başvuru sistemleri
Dünya şeklini düzensiz olduğundan Jeo-uzamsal veri koordinatlarını sistemlerindeki birçok koordinat başvurusu (CR), her biri kendi çerçeveler başvuru ve ölçü temsil edilir. Örneğin, "ulusal kılavuz, Britanya" başvuru sistemi doğru ise İngiltere, ancak değil dışında. 

En popüler CRS kullanımda bugün dünya Geodetic sistemidir [WGS 84](http://earth-info.nga.mil/GandG/wgs84/). GPS aygıtları ve Google Haritalar ve Bing haritaları API'si dahil olmak üzere birçok eşleme Hizmetleri WGS 84 kullanın. Azure Cosmos DB dizin oluşturma ve Jeo-uzamsal verileri yalnızca WGS 84 CRS kullanarak sorgulama destekler. 

## <a name="creating-documents-with-spatial-data"></a>Uzamsal verilerle belgeleri oluşturma
GeoJSON değerleri içeren belgeleri oluşturduğunuzda, uzamsal dizin ile dizin oluşturma ilkesini koleksiyonunun uygun otomatik olarak dizini oluşturulur. Bir Azure Cosmos DB SDK'sı ile Python veya Node.js gibi dinamik olarak yazılan bir dilde çalışıyorsanız, geçerli GeoJSON oluşturmanız gerekir.

**Node.js içinde Jeo-uzamsal verilerle belge oluşturma**

```json
var userProfileDocument = {
    "name":"cosmosdb",
    "location":{
        "type":"Point",
        "coordinates":[ -122.12, 47.66 ]
    }
};

client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
    // additional code within the callback
});
```

SQL API'leri ile çalışıyorsanız, kullanabileceğiniz `Point` ve `Polygon` içinde sınıfları `Microsoft.Azure.Documents.Spatial` uygulama nesnelerinizi içindeki konum bilgileri katıştırmak için ad alanı. Bu sınıfların seri hale getirme ve seri durumdan çıkarma uzamsal veri GeoJSON içine kolaylaştırmaya yardımcı.

**.NET Jeo-uzamsal verilerle belge oluşturma**

```json
using Microsoft.Azure.Documents.Spatial;

public class UserProfile
{
    [JsonProperty("name")]
    public string Name { get; set; }

    [JsonProperty("location")]
    public Point Location { get; set; }

    // More properties
}

await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
    new UserProfile 
    { 
        Name = "cosmosdb", 
        Location = new Point (-122.12, 47.66) 
    });
```

Enlem ve boylam bilgilere sahip değilseniz, ancak fiziksel adreslerini veya şehir veya ülke gibi konum adı, Bing Haritalar REST Hizmetleri gibi bir coğrafi kodlama hizmet kullanarak gerçek koordinatları bakabilirsiniz. Bing Haritalar coğrafi kodlama hakkında daha fazla bilgi [burada](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Uzamsal türler sorgulama
Biz Jeo-uzamsal veri eklemek nasıl göz ayırdıktan, SQL ve LINQ kullanarak Azure Cosmos DB kullanarak bu verileri sorgulamak nasıl bir göz atalım.

### <a name="spatial-sql-built-in-functions"></a>Uzamsal SQL yerleşik işlevler
Azure Cosmos DB Jeo-uzamsal sorgulamak için aşağıdaki açık Jeo-uzamsal Konsorsiyumu (OGC) yerleşik işlevleri destekler. Tamamını SQL dilinde yerleşik işlevler hakkında daha fazla bilgi için bkz: [sorgu Azure Cosmos DB](sql-api-sql-query.md).

<table>
<tr>
  <td><strong>Kullanım</strong></td>
  <td><strong>Açıklama</strong></td>
</tr>
<tr>
  <td>St_dıstance (spatial_expr, spatial_expr)</td>
  <td>İki GeoJSON noktası, çokgen veya LineString ifadeleri uzaklığı döndürür.</td>
</tr>
<tr>
  <td>ST_WITHIN (spatial_expr, spatial_expr)</td>
  <td>İlk GeoJSON nesne (noktası, çokgen veya LineString) ikinci GeoJSON nesne içinde (noktası, çokgen veya LineString) olup olmadığını gösteren bir Boole ifadesi döndürür.</td>
</tr>
<tr>
  <td>ST_INTERSECTS (spatial_expr, spatial_expr)</td>
  <td>İki belirtilen GeoJSON nesne (noktası, çokgen veya LineString) kesiştiği olup olmadığını gösteren bir Boole ifadesi döndürür.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Belirtilen GeoJSON noktası, çokgen veya LineString ifade geçerli olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Belirtilen GeoJSON noktası, çokgen veya LineString ifade geçerli olup olmadığını ve geçersiz bir Boole değeri içeren bir JSON değeri değeri döndürür, ayrıca bir dize değeri olarak nedeni.</td>
</tr>
</table>

Uzamsal işlevleri uzamsal veriler yakınlaştırmalı sorguları gerçekleştirmek için kullanılabilir. Örneğin, içinde 30 km st_dıstance yerleşik işlevini kullanarak belirtilen konumun olan tüm ailesi belgeleri döndüren bir sorgu aşağıdadır. 

**Sorgu**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Sonuçlar**

    [{
      "id": "WakefieldFamily"
    }]

Dizin oluşturma ilkenizi uzamsal dizin oluşturma eklerseniz, "uzaklığı sorguları" verimli bir şekilde dizin sunulacak. Uzamsal dizin oluşturma hakkında daha fazla ayrıntı için aşağıdaki bölümüne bakın. Belirtilen yol için bir uzamsal dizin yoksa, hala uzamsal sorguları belirterek gerçekleştirebileceğiniz `x-ms-documentdb-query-enable-scan` "true" değerini kümesiyle istek üstbilgisi. .NET içinde bu isteğe bağlı geçirerek yapılabilir **FeedOptions** sorgularıyla bağımsız değişkeni [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) true olarak ayarlanmış. 

ST_WITHIN bir noktası içinde Çokgen arasındadır varsa denetlemek için kullanılabilir. Yaygın olarak çokgenler sınırları posta kodları, durumu sınırları veya doğal durum oluşumlarıyla gibi göstermek için kullanılır. Dizin oluşturma ilkenizi uzamsal dizin oluşturma dahil ederseniz, tekrar sonra "içindeki" sorguları verimli bir şekilde dizin sunulacak. 

Yalnızca tek bir halka ST_WITHIN Çokgen değişkenlerinde içerebilir, diğer bir deyişle, çokgenler bunlara boşluklar içermemelidir. 

**Sorgu**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Sonuçlar**

    [{
      "id": "WakefieldFamily",
    }]

> [!NOTE]
> Hatalı biçimlendirilmiş veya geçersiz bağımsız değişken sonra için değerlendirir konum değeri belirtilen varsa Azure Cosmos DB sorgusunda nasıl eşleşmeyen türler iş benzer **tanımsız** ve değerlendirilen belgenin sorgu sonuçlarından atlanır. Sorgunuz hiçbir sonuç döndürürse, hata ayıklama ST_ISVALIDDETAILED için uzamsal türü neden geçersiz çalıştırın.     
> 
> 

Ters sorgular gerçekleştirme Azure Cosmos DB de destekler, diğer bir deyişle, çokgenler veya Azure Cosmos DB satırlarında dizin sonra için belirtilen bir nokta içeren alanlar sorgu. Bu desen sık Lojistik içinde bir kamyonu girdiğinde veya belirlenen alan bırakır, örneğin, tanımlamak için kullanılır. 

**Sorgu**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Sonuçlar**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID ve ST_ISVALIDDETAILED uzamsal nesne geçerli olup olmadığını denetlemek için kullanılabilir. Örneğin, aşağıdaki sorguyu bir noktası geçerlilik aralığı enlem değeri (-132.8) dışı ile denetler. ST_ISVALID yalnızca bir Boole değeri döndürür ve ST_ISVALIDDETAILED Boole ve neden geçersiz değerlendirilir neden içeren bir dize döndürür.

** Sorgu **

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Sonuçlar**

    [{
      "$1": false
    }]

Bu işlevler çokgenler doğrulamak için de kullanılabilir. Örneğin, burada ST_ISVALIDDETAILED kapalı bir Çokgen doğrulamak için kullanırız. 

**Sorgu**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Sonuçlar**

    [{
       "$1": { 
            "valid": false, 
            "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a Polygon must have the same start and end points." 
          }
    }]

### <a name="linq-querying-in-the-net-sdk"></a>.NET SDK'ın sorgulama LINQ
SQL .NET SDK'yı da sağlayıcıları yöntemleri saplama `Distance()` ve `Within()` LINQ ifadeleri içinde kullanmak için. Bu yöntem SQL LINQ sağlayıcısı çevirir eşdeğer SQL yerleşik işlev çağrıları çağrı (st_dıstance ve ST_WITHIN sırasıyla). 

"Konum" değeri 30 km belirtilen bir RADIUS içinde noktasındaki LINQ kullanarak Azure Cosmos DB koleksiyonunda tüm belgeleri bulur bir LINQ Sorgu bir örneği burada verilmiştir.

**LINQ sorgusu için uzaklık**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Benzer şekilde, belirtilen kutusunu/Çokgen içinde "Konum" olan tüm belgeleri bulmak için bir sorgu İşte. 

**İçinde LINQ sorgulamak için**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


LINQ ve SQL kullanarak belgeleri nasıl göz yapılan, uzamsal dizin oluşturma işlemi için Azure Cosmos DB yapılandırmak nasıl bir göz atalım.

## <a name="indexing"></a>Dizinleme
Bölümünde açıklanan [belirsiz şema dizin Azure Cosmos DB ile](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) kağıt, biz tasarlanmış olmasını Azure Cosmos veritabanı veritabanı altyapısı gerçekten şema belirsiz ve JSON için birinci sınıf destek sağlar. Azure Cosmos DB yazma en iyi duruma getirilmiş veritabanı motoruna yerel GeoJSON standart gösterilen uzamsal veriler (noktaları, çokgenler ve satırları) bilir.

Buna koysalar geometri 2B düzlemi üzerine geodetic koordinatları gelen öngörülen sonra aşamalı olarak kullanarak hücrelere bölünmüş bir **quadtree**. Bu hücreler 1 hücrede konumuna bağlı olarak D eşlendiği bir **Hilbert alanı doldurma eğri**, yerleşim yeri noktalarının korur. Konum verileri sıralandığında ayrıca olarak bilinen bir işlemle gidiyor **Mozaik döşeme**, diğer bir deyişle, bir konum kesiştiği tüm hücreleri tanımlanır ve Azure Cosmos DB dizin anahtar olarak depolanır. Sorgu zamanında noktaları ve çokgenler gibi bağımsız değişkenler de ilgili hücre kimliği aralıkları ayıklamak için grubun Mozaik, sonra verileri dizinden almak için kullanılır.

Uzamsal dizini içeren bir dizin oluşturma ilkesini belirtirseniz / * (tüm yolları), koleksiyon içinde bulunan tüm noktalarını verimli uzamsal sorguları için (ST_WITHIN ve st_dıstance) dizine sonra. Uzamsal dizinler değil duyarlılık değeri vardır ve her zaman varsayılan duyarlılık değeri kullanın.

> [!NOTE]
> Azure Cosmos DB noktaları, çokgenler ve MultiPoint otomatik dizin oluşturma destekler
> 
> 

Etkinleştirilirse, yani uzamsal dizin ile bir dizin oluşturma ilkesini aşağıdaki JSON parçacığı gösterir, uzamsal sorgulamak için belgeler içinde bulunan herhangi bir GeoJSON noktası dizini. Azure portalını kullanarak dizin oluşturma ilkesini değiştiriyorsanız, koleksiyonunuzu üzerinde dizin uzamsal etkinleştirmek için dizin oluşturma ilkesi için aşağıdaki JSON belirtebilirsiniz.

**Toplama dizin oluşturma ilkesi JSON noktaları ve çokgenler için etkin Spatial ile**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Bir kod parçacığı aşağıda verilmiştir noktaları içeren tüm yollar için açık uzamsal dizin ile bir koleksiyon oluşturmayı gösteren .NET içinde. 

**Uzamsal dizin oluşturma ile bir koleksiyon oluşturma**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

Burada da var olan bir koleksiyon içindeki belgelerde depolanır noktaları üzerinden uzamsal dizin oluşturma avantajından yararlanmak için nasıl değiştirebilirsiniz.

**Var olan bir koleksiyon uzamsal dizin oluşturma ile değiştirme**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [!NOTE]
> GeoJSON değeri belge içindeki konum hatalı biçimlendirilmiş veya geçersiz ise, ardından uzamsal sorgulamak için dizine değil. Konum değerleri ST_ISVALID ve ST_ISVALIDDETAILED kullanarak doğrulayabilirsiniz.
> 
> Koleksiyon tanımınızı bölüm anahtarı içeriyorsa, dönüşüm ilerleme dizin bildirilmedi. 
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Sonraki Jeo-uzamsal desteği Azure Cosmos veritabanı ile çalışmaya başlamak öğrendiniz, şunları yapabilirsiniz:

* İle kod yazmaya başlayın [Jeo-uzamsal .NET github'daki kod örnekleri](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)
* Konumundaki Jeo-uzamsal sorgulama ile ele almak [Azure Cosmos DB Query Playground](http://www.documentdb.com/sql/demo#geospatial)
* Daha fazla bilgi edinmek [Azure Cosmos DB sorgusu](sql-api-sql-query.md)
* Daha fazla bilgi edinmek [Azure Cosmos DB dizin oluşturma ilkeleri](indexing-policies.md)

