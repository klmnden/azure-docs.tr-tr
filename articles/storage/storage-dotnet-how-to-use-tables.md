---
title: ".NET kullanarak Azure Tablo Depolama’yı kullanmaya başlama | Microsoft Belgeleri"
description: "Bir NoSQL veri deposu olan Azure Table Storage kullanarak bulutta yapılandırılmış veri depolayın."
services: storage
documentationcenter: .net
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: fe46d883-7bed-49dd-980e-5c71df36adb3
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 04/10/2017
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 757d6f778774e4439f2c290ef78cbffd2c5cf35e
ms.openlocfilehash: 0764d4cbcd618be54c8b6e71a632d24c5c3bfe67
ms.lasthandoff: 04/10/2017


---
# <a name="get-started-with-azure-table-storage-using-net"></a>.NET kullanarak Azure Table Storage’ı kullanmaya başlayın
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

Azure Tablo Depolama, yapılandırılmış NoSQL verilerini bulutta depolayan ve şemasız bir tasarım ile anahtar/öznitelik deposu sağlayan bir hizmettir. Table Storage şemasız olduğu için uygulamanızın ihtiyaçları geliştikçe verilerinizi kolayca uyarlayabilirsiniz. Tablo Depolama verilerine erişim, çoğu uygulama türü için hızlı ve ekonomik olmanın yanı sıra benzer veri hacimleri için geleneksel SQL’e kıyasla genellikle daha düşük maliyetlidir.

Web uygulamaları için kullanıcı verileri, adres defterleri, cihaz bilgileri veya hizmetiniz için gerekli olan tüm diğer meta veri türleri gibi esnek veri kümelerini Tablo Depolama ile depolayabilirsiniz. Bir tabloda istediğiniz kadar varlık depolayabilirsiniz ve bir depolama hesabı kapasite limitini dolduracak kadar tablo içerebilir.

### <a name="about-this-tutorial"></a>Bu öğretici hakkında
Bu öğreticide, sık kullanılan bazı Azure Tablo Depolama senaryolarında [.NET için Azure Depolama İstemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/)’nın nasıl kullanılacağı gösterilmektedir. Bu senaryolar, tablo oluşturup silme ve tablo verileri ekleme, güncelleştirme, silme ve sorgulama işlemleri için C# örnekleri kullanılarak sunulmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi başarıyla tamamlamak için aşağıdakiler gerekir:

* [Microsoft Visual Studio](https://www.visualstudio.com/visual-studio-homepage-vs.aspx)
* [.NET için Azure Depolama İstemcisi](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [.NET için Azure Yapılandırma Yöneticisi](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [Azure depolama hesabı](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Daha fazla örnek
Tablo depolama kullanan diğer örnekler için [.NET’te Azure Table Storage Kullanmaya Başlama](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/). Örnek uygulamayı indirip çalıştırabilir veya GitHub’daki örneğe göz atabilirsiniz.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a>Using yönergeleri ekleme
Aşağıdaki **using** yönergelerini `Program.cs` dosyasının üst tarafına ekleyin:

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types
```

### <a name="parse-the-connection-string"></a>Bağlantı dizesini ayrıştırma
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>Tablo hizmeti istemcisi oluşturma
[CloudTableClient][dotnet_CloudTableClient] sınıfı, Tablo Depolamada depolanan tabloları ve varlıkları almanızı sağlar. Tablo hizmeti istemcisini oluşturma yöntemlerinden biri aşağıda verilmiştir:

```csharp
// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```

Artık Table Storage’dan veri okuyan ve bu depolamaya veri yazan kodu yazmaya hazırsınız.

## <a name="create-a-table"></a>Bir tablo oluşturma
Bu örnek, zaten yoksa, nasıl bir tablo oluşturulacağını gösterir:

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Retrieve a reference to the table.
CloudTable table = tableClient.GetTableReference("people");

// Create the table if it doesn't exist.
table.CreateIfNotExists();
```

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme
Varlıklar, [TableEntity][dotnet_TableEntity]’den oluşturulan özel bir sınıf kullanarak C# nesneleriyle eşlenir. Tabloya bir varlık eklemek için varlığınızın özelliklerini tanımlayan bir sınıf oluşturun. Aşağıdaki kod, sıra anahtarı olarak müşterinin adını, bölüm anahtarı olarak soyadını kullanan bir varlık sınıfı tanımlar. Birlikte, bir varlığın bölüm ve sıra anahtarı varlığı tabloda benzersiz şekilde tanımlar. Aynı bölüm anahtarına sahip varlıklar farklı bölüm anahtarlı varlıklara göre daha hızlı sorgulanabilir ancak farklı bölüm anahtarlarının kullanılması paralel işlemler için daha büyük ölçeklendirme sağlar. Tablolarda depolanacak varlıklar, [TableEntity][dotnet_TableEntity] sınıfından türetilen türler gibi desteklenen bir türde olmalıdır. Bir tabloda depolamak istediğiniz varlık özellikleri, türün genel özellikleri olmalı ve değerleri hem almayı hem de ayarlamayı desteklemelidir. Bununla birlikte varlık türü parametresiz bir oluşturucu *olmalıdır*.

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

Varlıklarla ilgili tablo işlemleri daha önce “Tablo oluşturma” bölümünde oluşturduğunuz [CloudTable][dotnet_CloudTable] nesnesi ile gerçekleştirilecektir. Gerçekleştirilecek işlem [TableOperation][dotnet_TableOperation] nesnesi ile belirtilir. Aşağıdaki kod örneği [CloudTable][dotnet_CloudTable] nesnesi ve ardından **CustomerEntity** nesnesinin oluşturulmasını gösterir. İşlemi hazırlamak için, müşteri varlığını tabloya yerleştirmek üzere bir [TableOperation][dotnet_TableOperation] nesnesi oluşturulur. Son olarak işlem [CloudTable][dotnet_CloudTable].[Execute][dotnet_CloudTable_Execute] çağrılarak çalıştırılır.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a>Toplu işlem varlık yerleştirme
Bir tabloya tek bir yazma işlemiyle çok sayıda varlık yerleştirebilirsiniz. Toplu işlemler ile ilgili diğer notlar:

* Aynı tek toplu işlemde güncelleştirme, silme ve yerleştirme yapabilirsiniz.
* Tek bir toplu işlem en fazla 100 varlık içerebilir.
* Tek bir toplu işlemdeki tüm varlıkların bölüm anahtarları aynı olmalıdır.
* Toplu işlem olarak sorgu gerçekleştirmek mümkün olsa da, toplu işlemdeki tek işlem olmalıdır.

Aşağıdaki kod örneği iki varlık nesnesi oluşturur ve [Insert][dotnet_TableBatchOperation_Insert] yöntemi kullanarak her birini [TableBatchOperation][dotnet_TableBatchOperation]’a ekler. Ardından, işlemi yürütmek için [CloudTable][dotnet_CloudTable].[Execute][dotnet_CloudTable_ExecuteBatch] çağrılır.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create the batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it to the table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it to the table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities to the batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute the batch operation.
table.ExecuteBatch(batchOperation);
```

## <a name="retrieve-all-entities-in-a-partition"></a>Tüm varlıkları bir bölüme alma
Bir bölümdeki tüm varlıklar için bir tabloyu sorgulamak üzere bir [TableQuery][dotnet_TableQuery] nesnesi kullanın. Aşağıdaki kod örneği, ‘Smith’in bölüm anahtarı olduğu varlıklar için bir filtre belirtir. Bu örnek sorgu sonuçlarındaki her varlığın alanlarını konsola yazdırır.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Construct the query operation for all customer entities where PartitionKey="Smith".
TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

// Print the fields for each customer.
foreach (CustomerEntity entity in table.ExecuteQuery(query))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Bir bölüme bir grup varlık alma
Bir bölümdeki tüm varlıkları sorgulamak istemiyorsanız bölüm anahtarı filtresi ile bir satır anahtarı filtresini birleştirerek bir aralık belirleyebilirsiniz. Aşağıdaki kod örneği, 'Smith' bölümünde, satır anahtarı (ad) alfabede 'E' harfinden önce gelen bir harfle başlayan tüm varlıkları almak için iki filtre kullanır, ardından sorgu sonuçlarını yazdırır.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create the table query.
TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

// Loop through the results, displaying information about the entity.
foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-single-entity"></a>Tek bir varlık alma
Tek, belirli bir varlığı almak üzere bir sorgu yazabilirsiniz. Aşağıdaki kod 'Ben Smith' müşterisini belirlemek üzere [TableOperation][dotnet_TableOperation] kullanır. Bu yöntem bir koleksiyon yerine yalnızca bir varlık döndürür ve [TableResult][dotnet_TableResult].[Result][dotnet_TableResult_Result]’ta dönen değer **CustomerEntity** nesnesidir. Bir sorguda hem bölüm hem de satır anahtarını belirtmek Tablo hizmetinden tek bir varlık almanın en hızlı yoludur.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Print the phone number of the result.
if (retrievedResult.Result != null)
{
    Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
}
else
{
    Console.WriteLine("The phone number could not be retrieved.");
}
```

## <a name="replace-an-entity"></a>Bir varlığı değiştirme
Bir varlığı güncelleştirmek için Tablo hizmetinden alın, varlık nesnesini değiştirin ve değişiklikleri Tablo hizmetine geri kaydedin. Aşağıdaki kod mevcut bir müşterinin telefon numarasını değiştirir. [Insert][dotnet_TableOperation_Insert] yöntemini çağırmak yerine bu kod [Replace][dotnet_TableOperation_Replace] kullanır. [Replace][dotnet_TableOperation_Replace], sunucu üzerindeki varlık alındığından beri değiştirilmemişse varlığın sunucu üzerinde tamamen değiştirilmesini sağlar, aksi takdirde işlem başarısız olur. Bu işlem, uygulamanızın başka bir bileşeninin alım ve güncelleştirme arasında gerçekleştirilen bir değişikliğin yanlışlıkla üzerine yazılmasını engellemek üzere başarısız olur. Bu başarısız işlem, varlığın yeniden alınması, (hala geçerli ise) değişikliklerin yapılması ve yeni bir [Replace][dotnet_TableOperation_Replace] işleminin gerçekleştirilmesiyle uygun şekilde ele alınır. Sonraki bölüm bu davranışı nasıl geçersiz kılacağınızı gösterecektir.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign the result to a CustomerEntity object.
CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

if (updateEntity != null)
{
    // Change the phone number.
    updateEntity.PhoneNumber = "425-555-0105";

    // Create the Replace TableOperation.
    TableOperation updateOperation = TableOperation.Replace(updateEntity);

    // Execute the operation.
    table.Execute(updateOperation);

    Console.WriteLine("Entity updated.");
}
else
{
    Console.WriteLine("Entity could not be retrieved.");
}
```

## <a name="insert-or-replace-an-entity"></a>Bir varlığı yerleştirme veya değiştirme
Varlık sunucudan alındığından beri değiştirilmişse, [Replace][dotnet_TableOperation_Replace] işlemleri başarısız olacaktır. Dahası, [Replace][dotnet_TableOperation_Replace] işleminin başarılı olması için ilk olarak varlığın sunucudan alınması gerekir. Buna karşın bazı durumlarda varlığın sunucuda olup olmadığını ve içinde saklı geçerli değerlerin ilgisiz olup olmadığını bilemeyebilirsiniz. Güncelleştirmeniz tümünün üzerine yazmalıdır. Bunu gerçekleştirmek için [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] işlemi kullanmanız gerekir. Bu işlem, varlık mevcut değilse varlığı yerleştirir, eğer varlık mevcutsa yapılan son güncelleştirmeden bağımsız olarak değiştirir.

Aşağıdaki kod örneğinde, 'Fred Jones' için bir müşteri varlığı oluşturulup 'people' tablosuna eklenir. Ardından, [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] işlemi ile varlık aynı bölüm anahtarı (Jones) ve satır anahtarı (Fred) ile sunucuya kaydedilir, ancak bu kez PhoneNumber özelliği için farklı bir değer kullanılır. [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] özelliğini kullandığımız için tüm özellik değerleri değiştirilir. Ancak, bir 'Fred Jones' varlığı tabloda mevcut olmasaydı, eklenecekti.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a customer entity.
CustomerEntity customer3 = new CustomerEntity("Jones", "Fred");
customer3.Email = "Fred@contoso.com";
customer3.PhoneNumber = "425-555-0106";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer3);

// Execute the operation.
table.Execute(insertOperation);

// Create another customer entity with the same partition key and row key.
// We've already created a 'Fred Jones' entity and saved it to the
// 'people' table, but here we're specifying a different value for the
// PhoneNumber property.
CustomerEntity customer4 = new CustomerEntity("Jones", "Fred");
customer4.Email = "Fred@contoso.com";
customer4.PhoneNumber = "425-555-0107";

// Create the InsertOrReplace TableOperation.
TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(customer4);

// Execute the operation. Because a 'Fred Jones' entity already exists in the
// 'people' table, its property values will be overwritten by those in this
// CustomerEntity. If 'Fred Jones' didn't already exist, the entity would be
// added to the table.
table.Execute(insertOrReplaceOperation);
```

## <a name="query-a-subset-of-entity-properties"></a>Giriş özellikleri alt kümesi sorgulama
Bir tablo sorgusu, varlığın tüm özellikleri yerine bir varlıktaki birkaç özelliği alabilir. Projeksiyon olarak adlandırılan bu yöntem bant genişliğini azaltır ve özellikle büyük varlıklar için sorgu performansını iyileştirebilir. Aşağıdaki kodda yer alan sorgu yalnızca tablodaki varlıkların e-posta adreslerini döndürür. Bu, [DynamicTableEntity][dotnet_DynamicTableEntity] ve ayrıca [EntityResolver][dotnet_EntityResolver] sorgusu kullanılarak gerçekleştirilir. [Upsert ve Sorgu Projeksiyon Tanıtımı blog yazısı][blog_post_upsert] ile projeksiyon hakkında daha fazla bilgi edinebilirsiniz. Projeksiyon depolama öykünücüsünde desteklenmez, bu nedenle bu kod yalnızca Tablo hizmetinde bir hesap kullanırken çalıştırılır.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Define the query, and select only the Email property.
TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

// Define an entity resolver to work with the entity after retrieval.
EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
{
    Console.WriteLine(projectedEmail);
}
```

## <a name="delete-an-entity"></a>Bir varlığı silme
Bir varlığı güncelleştirmek için gösterilen aynı yöntemi kullanarak, bir varlığı aldıktan sonra kolayca silebilirsiniz. Aşağıdaki kod bir müşteri girişini alır ve siler.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that expects a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign the result to a CustomerEntity.
CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

// Create the Delete TableOperation.
if (deleteEntity != null)
{
    TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

    // Execute the operation.
    table.Execute(deleteOperation);

    Console.WriteLine("Entity deleted.");
}
else
{
    Console.WriteLine("Could not retrieve the entity.");
}
```

## <a name="delete-a-table"></a>Bir tablo silme
Son olarak aşağıdaki kod örneği bir depolama hesabından bir tablo siler. Silinen bir tablo, silme işleminin ardından yeniden oluşturma için belirli bir süre kullanılamayacaktır.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Delete the table it if exists.
table.DeleteIfExists();
```

## <a name="retrieve-entities-in-pages-asynchronously"></a>Sayfalarda zaman uyumsuz olarak varlıkları alma
Çok sayıda varlık okuyorsanız ve tamamının dönmesini beklemek yerine alındıkları gibi varlıkları işlemek/görüntülemek istiyorsanız, bölümlendirilmiş bir sorgu kullanarak varlıkları alabilirsiniz. Bu örnek, geniş bir sonuç kümesinin dönmesini beklerken çalıştırmanın engellenmemesi için Zaman Uyumsuz - Bekleme yöntemi kullanarak sayfalardaki sonuçların nasıl döndürüleceğini gösterir. .NET’te Zaman Uyumsuz-Bekleme yönteminin kullanılması ile ilgili daha fazla ayrıntı için bkz. [Zaman Uyumsuz ve Bekleme ile zaman uyumsuz programlama (C# ve Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

```csharp
// Initialize a default TableQuery to retrieve all the entities in the table.
TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

// Initialize the continuation token to null to start from the beginning of the table.
TableContinuationToken continuationToken = null;

do
{
    // Retrieve a segment (up to 1,000 entities).
    TableQuerySegment<CustomerEntity> tableQueryResult =
        await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

    // Assign the new continuation token to tell the service where to
    // continue on the next iteration (or null if it has reached the end).
    continuationToken = tableQueryResult.ContinuationToken;

    // Print the number of rows retrieved.
    Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

// Loop until a null continuation token is received, indicating the end of the table.
} while(continuationToken != null);
```

## <a name="next-steps"></a>Sonraki adımlar
Table Storage’ın temellerini öğrendiğinize göre, daha karmaşık depolama görevleri hakkında daha fazla bilgi edinmek için bu bağlantıları takip edin:

* [.NET’te Azure Table Storage Kullanmaya Başlama](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/) bölümünde daha fazla Tablo depolama örneği bulabilirsiniz
* Kullanılabilir API’ler ile ilgili eksiksiz bilgiler için Tablo hizmeti başvuru belgelerini görüntüleyin:
  * [.NET başvurusu için Depolama İstemci Kitaplığı](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
  * [REST API başvurusu](http://msdn.microsoft.com/library/azure/dd179355)
* [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md) kullanarak Azure Storage ile birlikte çalışmak üzere yazdığınız kodları nasıl sadeleştireceğinizi öğrenin.
* Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.
  * Yapılandırılmamış verileri depolamak için [.NET kullanarak Azure Blob Storage’ı kullanmaya başlayın](storage-dotnet-how-to-use-blobs.md).
  * İlişkisel verileri depolamak için [.NET (C#) kullanarak SQL Veritabanı'na bağlanın](../sql-database/sql-database-develop-dotnet-simple.md).

[Download and install the Azure SDK for .NET]: /develop/net/
[Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

[blog_post_upsert]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx

[dotnet_api_ref]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[dotnet_CloudTableClient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtableclient.aspx
[dotnet_CloudTable]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.aspx
[dotnet_CloudTable_Execute]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.execute.aspx
[dotnet_CloudTable_ExecuteBatch]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.executebatch.aspx
[dotnet_DynamicTableEntity]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.dynamictableentity.aspx
[dotnet_EntityResolver]: https://msdn.microsoft.com/library/jj733144.aspx
[dotnet_TableBatchOperation]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx
[dotnet_TableBatchOperation_Insert]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.insert.aspx
[dotnet_TableEntity]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableentity.aspx
[dotnet_TableOperation]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.aspx
[dotnet_TableOperation_Insert]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.insert.aspx
[dotnet_TableOperation_InsertOrReplace]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.insertorreplace.aspx
[dotnet_TableOperation_Replace]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.replace.aspx
[dotnet_TableQuery]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablequery.aspx
[dotnet_TableResult]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableresult.aspx
[dotnet_TableResult_Result]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableresult.result.aspx

