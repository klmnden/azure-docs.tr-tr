---
title: "Tablo verisi Azure Cosmos veritabanı nasıl? | Microsoft Belgeleri"
description: "Sorgu tablosu veri Azure Cosmos veritabanı hakkında bilgi edinin"
services: cosmos-db
documentationcenter: 
author: kanshiG
manager: jhubbard
editor: 
tags: 
ms.assetid: 14bcb94e-583c-46f7-9ea8-db010eb2ab43
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: govindk
ms.openlocfilehash: e59cfa85c6bf584e44bdc6e88cc19d67df390041
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-how-to-query-table-data-by-using-the-table-api-preview"></a>Azure Cosmos DB: Tablo API (Önizleme) kullanarak tablo verileri sorgulamak nasıl?

Azure Cosmos DB [tablo API](table-introduction.md) (Önizleme) destekleyen OData ve [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) anahtar/değer (tablo) verileri sorgular.  

Bu makalede aşağıdaki görevleri içerir: 

> [!div class="checklist"]
> * Tablo API ile veri sorgulama

Bu makalede sorgularda aşağıdaki örneği kullanın `People` tablosu:

| PartitionKey | RowKey | E-posta | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0101 |
| Smith | Ben | Ben@contoso.com| 425-555-0102 |
| Smith | Jeff | Jeff@contoso.com| 425-555-0104 | 

Azure Cosmos DB Azure Table depolama API'leri ile uyumlu olduğundan [sorgulama tabloları ve varlıkları] bakın (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) tablosunu kullanarak sorguya hakkında ayrıntılar için API. 

Azure Cosmos DB sunar premium özellikleri hakkında daha fazla bilgi için bkz: [Azure Cosmos DB: Tablo API](table-introduction.md) ve [.NET içinde tablo API ile geliştirme](tutorial-develop-table-dotnet.md). 

## <a name="prerequisites"></a>Ön koşullar

Bu sorguları çalışmak bir Azure Cosmos DB hesabınız varsa ve kapsayıcısında varlık verilere sahip. Bu yok? Tamamlamak [beş dakikalık quickstart](https://aka.ms/acdbtnetqs) veya [Geliştirici öğretici](https://aka.ms/acdbtabletut) bir hesap oluşturun ve veritabanınızı doldurmak için.

## <a name="query-on-partitionkey-and-rowkey"></a>Sorgu PartitionKey ve RowKey
Bir varlığın birincil anahtarının PartitionKey ve RowKey özellikler form için varlık tanımlamak için aşağıdaki özel söz dizimini kullanabilirsiniz: 

**Sorgu**

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
**Sonuçları**

| PartitionKey | RowKey | E-posta | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0104 |

Alternatif olarak, bir parçası olarak bu özellikleri belirtebilirsiniz `$filter` , aşağıdaki bölümde gösterildiği gibi seçeneği. Anahtar özellik adlarının ve sabit değerleri büyük küçük harfe duyarlı olduğunu unutmayın. PartitionKey ve RowKey dize türünde özelliklerdir. 

## <a name="query-by-using-an-odata-filter"></a>OData filtresini kullanarak sorgulama
Bu kurallar, bir filtre dizesi oluşturulurken göz önünde bulundurun: 

* Bir özellik için bir değer karşılaştırmak için OData Protokolü belirtim tarafından tanımlanan mantıksal işleçler kullanın. Özelliği dinamik bir değere karşılaştırılamıyor unutmayın. İfade bir tarafındaki bir sabit olmalıdır. 
* Özellik adı, işleç ve sabit değer URL kodlanmış boşluklarla ayrılmış olması gerekir. URL kodlu olarak bir boşluk `%20`. 
* Filtre dizesi tüm parçalarını büyük/küçük harfe duyarlıdır. 
* Geçerli sonuçları döndürmek için aynı veri türünde filtre sırasını özelliği olarak sabit değeri olmalıdır. Desteklenen özellik türleri hakkında daha fazla bilgi için bkz: [tablo hizmeti veri modelini anlama](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). 

Bir OData kullanarak PartitionKey ve e-posta özellikleri tarafından filtre gösteren örnek bir sorgu işte `$filter`.

**Sorgu**

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

Çeşitli veri türleri için filtre ifadeleri oluşturma hakkında daha fazla bilgi için bkz: [sorgulama tabloları ve varlıkları](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).

**Sonuçları**

| PartitionKey | RowKey | E-posta | PhoneNumber |
| --- | --- | --- | --- |
| Ben |Smith | Ben@contoso.com| 425-555-0102 |

## <a name="query-by-using-linq"></a>LINQ kullanarak sorgulama 
Karşılık gelen OData sorgu ifadeleri için çevirir LINQ kullanarak da sorgulayabilirsiniz. .NET SDK kullanarak sorgu oluşturma konusunda bir örnek aşağıda verilmiştir:

```csharp
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("people");

TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
    .Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition(PartitionKey, QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition(Email, QueryComparisons.Equal,"Ben@contoso.com")
    ));

await table.ExecuteQuerySegmentedAsync<CustomerEntity>(query, null);
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakileri yaptığınızdan:

> [!div class="checklist"]
> * Tablo API (Önizleme) kullanarak sorgulama öğrendiniz 

Verilerinizi Genel dağıtma konusunda bilgi almak için sonraki öğretici şimdi devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Verilerinizi genel Dağıt](tutorial-global-distribution-documentdb.md)
