---
title: Tablo depolama ve Visual Studio ile çalışmaya nasıl başlayacağınız bağlı Hizmetleri (ASP.NET Core) | Microsoft Docs
description: Visual Studio'da ASP.NET Core projesinde Azure Table storage ile Visual Studio kullanarak bir depolama hesabı bağlandıktan sonra nasıl başlayacağınızı Hizmetleri bağlı
services: storage
documentationcenter: ''
author: ghogen
manager: douge
editor: ''
ms.assetid: c3c451d1-71ff-4222-a348-c41c98a02b85
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: ghogen
ms.openlocfilehash: c20176b33962760560bbdb1cfe0d41377227d720
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-get-started-with-azure-table-storage-and-visual-studio-connected-services"></a>Azure tablo depolaması ve Visual Studio ile çalışmaya nasıl başlayacağınız Hizmetleri bağlı

[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

Bu makale, oluşturduğunuz veya Visual Studio kullanarak bir ASP.NET Core projesini bir Azure depolama hesabında başvurulan sonra Visual Studio'da Azure tablo depolaması ile çalışmaya başlamak açıklamaktadır **bağlantılı Hizmetler** özelliği. **Bağlantılı Hizmetler** işlemi Azure depolama projenize erişmek için uygun NuGet paketlerini yükler ve proje yapılandırma dosyalarınızı depolama hesabı için bağlantı dizesi ekler. (Bkz [Storage belgeleri](https://azure.microsoft.com/documentation/services/storage/) Azure Storage hakkında genel bilgi için.)

Azure Table depolama birimi hizmeti, büyük miktarlarda yapılandırılmış verileri depolamak sağlar. Hizmet, kimliği doğrulanmış çağrılarından içinden ve dışından Azure bulut kabul eden bir NoSQL veri deposudur. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir. Azure Table storage kullanma hakkında daha fazla genel bilgi için bkz: [.NET kullanarak Azure Table storage'ı kullanmaya başlama](../storage/storage-dotnet-how-to-use-tables.md).

Başlamak için önce depolama hesabınızdaki bir tablo oluşturun. Bu makalede daha sonra bir tablo C# ' ta oluşturma ve ekleme, değiştirme, okuma ve tablo girdilerini kaldırma gibi temel tablo işlemleri gösterir.  Kod, .NET için Azure Storage istemci kitaplığı kullanır. ASP.NET hakkında daha fazla bilgi için bkz: [ASP.NET](http://www.asp.net).

Bazı Azure depolama API'leri zaman uyumsuz ve zaman uyumsuz yöntemleri kullanıldığı bu makaledeki kod varsayar. Bkz: [zaman uyumsuz programlama](https://docs.microsoft.com/dotnet/csharp/async) daha fazla bilgi için.

## <a name="access-tables-in-code"></a>Kod erişim tablolarında

ASP.NET Core projeleri tablolarda erişmek için Azure tablo depolaması erişen tüm C# kaynak dosyaları için aşağıdaki öğeleri eklemeniz gerekir.

1. Gerekli eklemek `using` deyimleri:

    ```cs
    using Microsoft.Framework.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Table;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Framework.Logging.LogLevel;
    ```

1. Alma bir `CloudStorageAccount` depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın:

    ```cs
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir `CloudTableClient` depolama hesabınızdaki tablo nesneleri başvurmak için:

    ```cs
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Alma bir `CloudTable` belirli bir tablo ve varlıkları başvurmak için başvuru nesnesi:

    ```cs
    // Get a reference to a table named "peopleTable"
    CloudTable peopleTable = tableClient.GetTableReference("peopleTable");
    ```

## <a name="create-a-table-in-code"></a>Kod içinde bir tablo oluşturma

Azure tablo oluşturmak için arama '' CreateIfNotExistsAsync()':

```cs
// Create the CloudTable if it does not exist
await peopleTable.CreateIfNotExistsAsync();
```

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme

Bir tabloya bir varlık eklemek için varlığınızın özelliklerini tanımlayan bir sınıf oluşturun. Aşağıdaki kod olarak adlandırılan bir varlık sınıfı tanımlar `CustomerEntity` , müşterinin adını satır anahtarını ve Soyadı bölüm anahtarı olarak kullanır.

```cs
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

Tablo varlıkları kullanım ilgili işlemler `CloudTable` daha önce oluşturduğunuz nesne [erişim kodu tablolarda](#access-tables-in-code). `TableOperation` Nesnesi yapılması işlemi temsil eder. Aşağıdaki kod örneğinde nasıl oluşturulacağını gösterir bir `CloudTable` nesne ve `CustomerEntity` nesne. İşlemi hazırlamak için bir `TableOperation` müşteri varlığını tabloya yerleştirmek için oluşturulur. Son olarak, işlem çağrılarak çalıştırılır `CloudTable.ExecuteAsync`.

```cs
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
await peopleTable.ExecuteAsync(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a>Toplu işlem varlık yerleştirme

Birden çok varlık, bir tek bir yazma işleminde bir tabloya ekleyebilirsiniz. Aşağıdaki kod örneği iki varlık nesnesi ("Jeff Smith" ve "Ben Smith") oluşturur, eklenmektedir bir `TableBatchOperation` kullanarak nesne `Insert` yöntemi, ve ardından çağırarak işlemi başlatır `CloudTable.ExecuteBatchAsync`.

```cs
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
await peopleTable.ExecuteBatchAsync(batchOperation);
```

## <a name="get-all-of-the-entities-in-a-partition"></a>Tüm varlıkları bir bölüme alma

Tüm varlıkları bir bölüme için bir tabloyu sorgulamak için kullanın bir `TableQuery` nesnesi. Aşağıdaki kod örneği, ‘Smith’in bölüm anahtarı olduğu varlıklar için bir filtre belirtir. Bu örnek sorgu sonuçlarındaki her varlığın alanlarını konsola yazdırır.

```cs
// Construct the query operation for all customer entities where PartitionKey="Smith".
TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

// Print the fields for each customer.
TableContinuationToken token = null;
do
{
    TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
    token = resultSegment.ContinuationToken;

    foreach (CustomerEntity entity in resultSegment.Results)
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
    }
} while (token != null);
```

## <a name="get-a-single-entity"></a>Tek bir varlık alma

Tek, belirli bir varlığı almak üzere bir sorgu yazabilirsiniz. Aşağıdaki kod bir `TableOperation` 'Ben Smith' adlı bir müşteri belirtmek için nesne. Yöntemi yalnızca bir varlık yerine bir koleksiyon ve döndürülen değeri döndürür `TableResult.Result` olan bir `CustomerEntity` nesnesi. Tek bir varlık almanın en hızlı yolu olan bir sorguda hem Bölüm hem de satır anahtarını belirterek `Table` hizmet.

```cs
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

// Print the phone number of the result.
if (retrievedResult.Result != null)
   Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
else
   Console.WriteLine("The phone number could not be retrieved.");
```

## <a name="delete-an-entity"></a>Bir varlığı silme

Bunu bulduktan sonra bir varlık silebilirsiniz. Aşağıdaki kod arar ve "Ben Smith" adlı bir müşteri varlığı siler:

```cs
// Create a retrieve operation that expects a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

// Assign the result to a CustomerEntity object.
CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

// Create the Delete TableOperation and then execute it.
if (deleteEntity != null)
{
   TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

   // Execute the operation.
   await peopleTable.ExecuteAsync(deleteOperation);

   Console.WriteLine("Entity deleted.");
}

else
   Console.WriteLine("Couldn't delete the entity.");
```

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]
