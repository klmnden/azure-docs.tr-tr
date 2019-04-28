---
title: Azure Cosmos DB’de tablo verilerini sorgulama
description: Azure Cosmos DB’de tablo verilerini sorgulamayı öğrenin
author: wmengmsft
ms.author: wmeng
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: tutorial
ms.date: 11/15/2017
ms.reviewer: sngun
ms.openlocfilehash: 28e9b420f3abc972e047af72e1b5bc755e7f9d2b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62115811"
---
# <a name="tutorial-query-azure-cosmos-db-by-using-the-table-api"></a>Öğretici: Tablo API'sini kullanarak Azure Cosmos DB'yi sorgulama

Azure Cosmos DB [Table API’si](table-introduction.md), anahtar/değer (tablo) verilerine karşı OData ve [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) sorgularını destekler.  

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Tablo API’si ile verileri sorgulama

Bu makaledeki sorgularda aşağıdaki `People` tablosu kullanılmaktadır:

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0101 |
| Smith | Ben | Ben@contoso.com| 425-555-0102 |
| Smith | Jeff | Jeff@contoso.com| 425-555-0104 | 

Tablo API’sini kullanarak nasıl sorgulama yapılacağı ile ilgili ayrıntılar için bkz. [Tabloları ve Varlıkları Sorgulama](https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities). 

Azure Cosmos DB’nin sunduğu üstün yetenekler hakkında daha fazla bilgi için bkz. [Azure Cosmos DB Tablo API’si](table-introduction.md) ve [.NET’te Tablo API’si ile geliştirme](tutorial-develop-table-dotnet.md). 

## <a name="prerequisites"></a>Önkoşullar

Bu sorguların çalışması için bir Azure Cosmos DB hesabınız ve kapsayıcıda varlık verileriniz olmalıdır. Bunlardan biri yok mu? Bir hesap oluşturmak ve veritabanınızı doldurmak için [beş dakikalık hızlı başlangıç](create-table-dotnet.md) veya [geliştirici öğreticisini](tutorial-develop-table-dotnet.md) tamamlayın.

## <a name="query-on-partitionkey-and-rowkey"></a>PartitionKey ve RowKey üzerinde sorgulama
PartitionKey ve RowKey özellikleri bir varlığın birincil anahtarını oluşturduğundan, varlığı belirlemek için aşağıdaki özel sözdizimini kullanabilirsiniz: 

**Sorgu**

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
**Sonuçlar**

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0104 |

Alternatif olarak, aşağıdaki bölümde gösterildiği gibi, `$filter` seçeneğinin parçası olarak bu özellikleri belirtebilirsiniz. Anahtar özelliği adlarının ve sabit değerlerin büyük/küçük harfe duyarlı olduğunu unutmayın. PartitionKey ve RowKey özellikleri, Dize türündedir. 

## <a name="query-by-using-an-odata-filter"></a>OData filtresi kullanarak sorgulama
Bir filtre dizesi oluştururken şu kuralları göz önünde bulundurun: 

* Bir özelliği bir değerle karşılaştırmak için OData Protokol Belirtimi tarafından tanımlanan mantıksal işleçleri kullanın. Bir özelliği dinamik değerle karşılaştıramayacağınızı unutmayın. İfadenin bir tarafı sabit olmalıdır. 
* Özellik adı, işleç ve sabit değeri, URL kodlamalı boşluklarla ayrılmalıdır. Boşluk, `%20` olarak URL kodlamalı olur. 
* Filtre dizesinin tüm kısımları büyük/küçük harfe duyarlıdır. 
* Filtrenin geçerli sonuçlar döndürmesi için sabit değer, özellikle aynı veri türünde olmalıdır. Desteklenen özellik türleri hakkında daha fazla bilgi için bkz. [Tablo Hizmeti Veri Modelini anlama](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). 

Aşağıda, OData `$filter` kullanılarak PartitionKey ve Email özelliklerine göre nasıl filtreleme yapılacağını gösteren örnek bir sorgu verilmiştir.

**Sorgu**

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

Çeşitli veri türleri için filtre ifadeleri oluşturma hakkında daha fazla bilgi için bkz. [Tabloları ve Varlıkları Sorgulama](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).

**Sonuçlar**

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Smith |Ben | Ben@contoso.com| 425-555-0102 |

## <a name="query-by-using-linq"></a>LINQ kullanarak sorgulama 
İlgili OData sorgu ifadelerine çeviri yapan LINQ kullanarak da sorgulama yapabilirsiniz. Aşağıda, .NET SDK kullanılarak sorgu derlemeye ilişkin bir örnek verilmiştir:

```csharp
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("People");

TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
    .Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("Email", QueryComparisons.Equal,"Ben@contoso.com")
    ));

await table.ExecuteQuerySegmentedAsync<CustomerEntity>(query, null);
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Tablo API’sini kullanarak nasıl sorgulama yapıldığını öğrendiniz

Artık verilerinizi genel olarak nasıl dağıtacağınızı öğrenmek için sonraki öğreticiye ilerleyebilirsiniz.

> [!div class="nextstepaction"]
> [Verilerinizi genel olarak dağıtma](tutorial-global-distribution-table.md)
