---
title: Azure Cosmos DB'de SQL sorgusu yürütme
description: Azure Cosmos DB'de SQL sorgusu yürütme hakkında bilgi edinin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: tisande
ms.openlocfilehash: e4e26b658bd29e4589be40e4d29935059836c909
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342519"
---
# <a name="azure-cosmos-db-sql-query-execution"></a>Azure Cosmos DB SQL sorgusu yürütme

HTTP/HTTPS istekleri yapabilen herhangi bir dilde, Cosmos DB REST API'si çağırabilirsiniz. Cosmos DB ayrıca .NET, Node.js, JavaScript ve Python programlama dilde programlama kitaplıkları sunar. SQL ile sorgulama tüm REST API ve kitaplıkları destekleyen ve .NET SDK'yı da destekler [LINQ sorgusu](sql-query-linq-to-sql.md).

Aşağıdaki örnekler bir sorgu oluşturun ve Cosmos DB veritabanı hesabını karşı gönderme işlemini göstermektedir.

## <a id="REST-API"></a>REST API

Cosmos DB, HTTP üzerinden açık bir RESTful programlama modeli sunar. Kaynak modeli, bir veritabanı hesabı altında kaynak kümesinden oluşur, bir Azure aboneliği sağlar. Veritabanı hesabı kümesinden oluşur *veritabanları*, her biri birden çok içerebilir *kapsayıcıları*, hangi sırayla içeren *öğeleri*, UDF'ler ve diğer kaynak türleri. Her Cosmos DB mantıksal ve kararlı bir URI kullanılarak adreslenebilir bir kaynaktır. Bir kaynak kümesi olarak adlandırılan bir *akış*. 

HTTP fiilleri temel etkileşim modelidir bu kaynaklarla `GET`, `PUT`, `POST`, ve `DELETE`, kendi standart ınterpretations ile. Kullanım `POST` yeni bir kaynak oluşturmak için bir saklı yordamı yürütme veya bir Cosmos DB sorgusu yayınlanacak. Sorgu her zaman salt okunur yan etkileri olan işlemlerdir.

Aşağıdaki örneklerde gösterildiği bir `POST` örnek öğeleri bir API SQL sorgusu için. Sorgu üzerinde JSON basit bir filtre sahip `name` özelliği. `x-ms-documentdb-isquery` Ve Content-Type: `application/query+json` üstbilgileri belirtmek olduğu sorgu işlemi. Değiştirin `mysqlapicosmosdb.documents.azure.com:443` ile Cosmos DB hesabınız için URI.

```json
    POST https://mysqlapicosmosdb.documents.azure.com:443/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",
        "parameters": [
            {"name": "@familyId", "value": "AndersenFamily"}
        ]
    }
```

Sonuçlar şu şekildedir:

```json
    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"Seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }
```

İleri, daha karmaşık bir sorgu birleştirme sonucu birden çok sonuç döndürür:

```json
    POST https://https://mysqlapicosmosdb.documents.azure.com:443/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT
                     f.id AS familyName,
                     c.givenName AS childGivenName,
                     c.firstName AS childFirstName,
                     p.givenName AS petName
                  FROM Families f
                  JOIN c IN f.children
                  JOIN p in c.pets",
        "parameters": []
    }
```

Sonuçlar şu şekildedir: 

```json
    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }
```

Bir sorgunun sonuçlarını tek bir sayfasına sığdıramazsanız REST API aracılığıyla bir devamlılık belirteci döndürür. `x-ms-continuation-token` yanıtı üstbilgisi. İstemcileri, sonraki sonuçları üst bilgisi dahil olmak üzere sonuçlarını sayfalandırma. İle ilgili sayfa başına sonuç sayısı de denetleyebilirsiniz `x-ms-max-item-count` sayı başlığı.

Bir sorgu sayısı gibi bir toplama işlevi varsa, sorgu sayfası yalnızca bir sayfalık sonuç kısmen toplanan bir değer döndürebilir. İstemciler, son sonuçlar için bu sonuçlar üzerinde ikinci düzey toplama gerçekleştirmeniz gerekir. Örneğin, her bir sayfayı toplam sayısını döndürmek için döndürülen sayıları üzerinden toplayın.

Sorgular için veri tutarlılık ilkesi yönetmek için kullandığınız `x-ms-consistency-level` tüm REST API istekleri olduğu gibi üst bilgisi. Oturum tutarlılığı ayrıca gerektirir en son Yankı `x-ms-session-token` sorgu istekteki tanımlama bilgisi üstbilgisi. Sorgulanan kapsayıcının dizin oluşturma ilkesini tutarlılığını sorgu sonuçlarını da etkileyebilir. İle dizin oluşturma ilkesi ayarları kapsayıcılar için varsayılan dizin her zaman geçerli öğe içeriğiyle ve sorgu sonuçları için veri seçilen tutarlılık eşleşen. Daha fazla bilgi için bkz. [Azure Cosmos DB tutarlılık düzeyleri] [tutarlılık düzeyleri].

Belirtilen sorgu kapsayıcı üzerindeki yapılandırılmış dizin oluşturma ilkesini destekleyemiyorsa, Azure Cosmos DB sunucusu 400 "Bad Request" döndürür. Bu hata iletisini sorgular için açıkça dizine elmadan hariç yollarla döndürür. Belirtebileceğiniz `x-ms-documentdb-query-enable-scan` dizin kullanılamadığında bir tarama gerçekleştirmek sorgu izni başlığı.

Ayarlayarak, sorgu yürütme ayrıntılı ölçümleri alabilirsiniz `x-ms-documentdb-populatequerymetrics` başlığına `true`. Daha fazla bilgi için [Azure Cosmos DB için SQL sorgu ölçümleri](sql-api-query-metrics.md).

## <a name="c-net-sdk"></a>C# (.NET SDK)

LINQ hem SQL .NET SDK'yı destekleyen sorgulama. Aşağıdaki örnek, .NET ile önceki filtre sorgusu gerçekleştirmeye gösterilmektedir:

```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(containerLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(containerLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(containerLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

Aşağıdaki örnek, her öğenin eşitlik için iki özellik karşılaştırır ve anonim projeksiyonlar kullanır.

```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family
        FROM Families f
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(containerLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(containerLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

Sonraki örnek, birleşimler, LINQ ifade gösterir `SelectMany`.

```csharp
    foreach (var pet in client.CreateDocumentQuery(containerLink,
          @"SELECT p
            FROM Families f
                 JOIN c IN f.children
                 JOIN p in c.pets
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions:
    foreach (var pet in
        client.CreateDocumentQuery<Family>(containerLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }
```

.NET istemci otomatik olarak sorgu sonuçlarında tüm sayfaları aracılığıyla yinelenir `foreach` , yukarıdaki örnekte gösterildiği gibi engeller. Sorgu seçenekleri de kullanıma sunulan [REST API](#REST-API) bölüm de mevcuttur .NET SDK kullanarak `FeedOptions` ve `FeedResponse` sınıfları `CreateDocumentQuery` yöntemi. Sayfa sayısı kullanarak denetleyebilirsiniz `MaxItemCount` ayarı.

Disk belleği oluşturarak de açıkça denetleyebilirsiniz `IDocumentQueryable` kullanarak `IQueryable` okuyarak sonra nesne, `ResponseContinuationToken` değerleri ve bunları geçirmeden geri olarak `RequestContinuationToken` içinde `FeedOptions`. Ayarlayabileceğiniz `EnableScanInQuery` sorgu tarafından yapılandırılan dizin oluşturma ilkesini sunulmaması halinde taramaları etkinleştirmek için. Bölümlenmiş kapsayıcılar için kullanabileceğiniz `PartitionKey` Azure Cosmos DB otomatik olarak bu sorgu metni ayıklayabilir olsa da tek bir bölüm karşı sorgu çalıştırmak için. Kullanabileceğiniz `EnableCrossPartitionQuery` birden çok bölüm karşı sorguları çalıştırmak için.

Sorgular sayesinde daha fazla .NET örnekleri için bkz. [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet) github'da.

## <a id="JavaScript-server-side-API"></a>JavaScript sunucu tarafı API'si

Azure Cosmos DB için bir programlama modeli sağlar [JavaScript yürütme, uygulama tabanlı](stored-procedures-triggers-udfs.md) doğrudan kullanarak kapsayıcıları mantığını saklı yordamlar ve tetikleyiciler. Kapsayıcı düzeyinde kayıtlı JavaScript mantığının ardından öğelerinde belirtilen kapsayıcının içinde çevresel ACID işlemlerini sarmalanmış veritabanı işlemleri verebilir.

Aşağıdaki örnek nasıl kullanılacağını gösterir `queryDocuments` JavaScript Server sorgularından yapmak için API iç saklı yordamları ve Tetikleyicileri:

```javascript
    function findName(givenName, familyName) {
        var context = getContext();
        var containerManager = context.getCollection();
        var containerLink = containerManager.getSelfLink()

        // create a new item.
        containerManager.createDocument(containerLink,
            { givenName: givenName, familyName: familyName },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter items by familyName
                var filterQuery = "SELECT * from root r WHERE r.familyName = 'Wakefield'";
                containerManager.queryDocuments(containerLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace the familyName for all items that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].familyName = "Robin Wakefield";
                            // we don't need to execute a callback because they are in parallel
                            containerManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'ye giriş](introduction.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Azure Cosmos DB tutarlılık düzeyleri](consistency-levels.md)
