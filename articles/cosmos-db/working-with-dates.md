---
title: "Azure Cosmos DB tarihleri ile çalışma | Microsoft Docs"
description: "Azure Cosmos veritabanı tarihleri ile birlikte çalışma hakkında bilgi edinin."
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: e587772f-ce9f-498c-a017-a51e7265bb23
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: arramac
ms.openlocfilehash: b6a77e33eea24000037ffb31d7aae3cb1d345ce9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="working-with-dates-in-azure-cosmos-db"></a>Azure Cosmos DB tarihleri ile çalışma
Azure Cosmos DB sunar şema esnekliği ve zengin bir yerel dizin oluşturma [JSON](http://www.json.org) veri modeli. Veritabanları, koleksiyonlar, belgeler ve saklı yordamları da dahil olmak üzere tüm Azure Cosmos DB kaynakları modellenir ve JSON belgeleri olarak depolanır. Olma taşınabilir bir zorunluluk, JSON (ve Azure Cosmos DB) temel türleri, yalnızca küçük bir kümesini destekler: dize, sayı, Boole değeri, dizi, nesne ve Null. Ancak, JSON esnektir ve geliştiriciler ve çerçeveleri nesneleri veya dizi oluşturma ve bu temelleri kullanarak daha karmaşık türleri temsil eden kullanmasına olanak tanır. 

Temel türlerine ek olarak birçok uygulama gereksinim [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) tarihleri ve tarih damgası temsil eden tür. Bu makalede nasıl geliştiriciler depolamak, alabilir ve .NET SDK kullanarak Azure Cosmos DB tarihleri sorgu açıklanmaktadır.

## <a name="storing-datetimes"></a>Tarih/saat depolanması
Varsayılan olarak, [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) DateTime değerleri olarak serileştiren [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) dizeleri. Uygulamaların çoğu, aşağıdaki nedenlerle DateTime için varsayılan dize gösterimi kullanabilirsiniz:

* Dizeleri karşılaştırma ve dizeye dönüştürülen DateTime değerlerini göreli sıralama korunur. 
* Bu yaklaşım, JSON dönüştürme için herhangi bir özel kod veya öznitelikleri gerektirmez.
* JSON içinde depolanan tarihleri İnsan okunabilir.
* Bu yaklaşım hızlı sorgu performansı için Azure Cosmos veritabanı dizin yararlanabilir.

Örneğin, aşağıdaki kod parçacığında depolayan bir `Order` içeren iki DateTime özellikleri - nesne `ShipDate` ve `OrderDate` .NET SDK kullanarak bir belge olarak:

    public class Order
    {
        [JsonProperty(PropertyName="id")]
        public string Id { get; set; }
        public DateTime OrderDate { get; set; }
        public DateTime ShipDate { get; set; }
        public double Total { get; set; }
    }

    await client.CreateDocumentAsync("/dbs/orderdb/colls/orders", 
        new Order 
        { 
            Id = "09152014101",
            OrderDate = DateTime.UtcNow.AddDays(-30),
            ShipDate = DateTime.UtcNow.AddDays(-14), 
            Total = 113.39
        });

Bu belgede Azure Cosmos DB'de şekilde depolanır:

    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
    

Alternatif olarak, tarih/saat olarak UNIX zaman damgaları, diğer bir deyişle, 1 Ocak 1970'ten beri geçen saniyelerin sayısını gösteren bir sayı olarak saklayabilirsiniz. Azure Cosmos DB'ın iç zaman damgası (`_ts`) özelliği, bu yaklaşım izler. Kullanabileceğiniz [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) sayı olarak tarih/saat serileştirmek için sınıfı. 

## <a name="indexing-datetimes-for-range-queries"></a>Tarih/saat aralığı sorgular için dizin oluşturma
Aralık sorguları içeren DateTime değerleri yaygındır. Örneğin, dünden bu yana oluşturulan tüm siparişleri bulmak veya son beş dakika içinde gönderilen tüm siparişleri bulmak gerekiyorsa, aralık sorguları gerçekleştirmek için gerekir. Bu sorguları verimli bir şekilde çalıştırmak için koleksiyonunuzu aralığı dizeleri dizin oluşturma için yapılandırmanız gerekir.

    DocumentCollection collection = new DocumentCollection { Id = "orders" };
    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    await client.CreateDocumentCollectionAsync("/dbs/orderdb", collection);

Dizin oluşturma ilkeleri yapılandırma hakkında daha fazla bilgi edinebilirsiniz [Azure Cosmos DB dizin oluşturma ilkeleri](indexing-policies.md).

## <a name="querying-datetimes-in-linq"></a>Tarih/saat LINQ sorgulama
DocumentDB .NET SDK'yı otomatik olarak LINQ aracılığıyla Azure Cosmos veritabanında depolanan verileri Sorgulama destekler. Örneğin, aşağıdaki kod parçacığını bir LINQ Sorgu son üç günde sevk edilen bu filtreler siparişleri gösterir.

    IQueryable<Order> orders = client.CreateDocumentQuery<Order>("/dbs/orderdb/colls/orders")
        .Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
          
    // Translated to the following SQL statement and executed on Azure Cosmos DB
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")

Azure Cosmos veritabanı SQL sorgu dili ve LINQ sağlayıcısındaki hakkında daha fazla bilgiyi [sorgulama Cosmos DB](documentdb-sql-query.md).

Bu makalede, nasıl depolamak, dizin ve tarih/saat Azure Cosmos veritabanı sorgu inceledik.

## <a name="next-steps"></a>Sonraki Adımlar
* İndirme ve çalıştırma [github'daki kod örnekleri](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)
* Daha fazla bilgi edinmek [DocumentDB API sorgusu](documentdb-sql-query.md)
* Daha fazla bilgi edinmek [Azure Cosmos DB dizin oluşturma ilkeleri](indexing-policies.md)
