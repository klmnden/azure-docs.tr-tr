---
title: Tablo depolama ve Visual Studio ile çalışmaya başlama nasıl bağlı hizmetler (ASP.NET Core) | Microsoft Docs
description: Nasıl bir ASP.NET Core projesi Visual Studio'da Azure tablo depolama ile Visual Studio kullanarak bir depolama hesabına bağlandıktan sonra başlamak bağlı hizmetler
services: storage
author: ghogen
manager: douge
ms.assetid: c3c451d1-71ff-4222-a348-c41c98a02b85
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ghogen
ms.openlocfilehash: 1f90ce71084ba3acbf5a0aec5c7b8e9683323766
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60362127"
---
# <a name="how-to-get-started-with-azure-table-storage-and-visual-studio-connected-services"></a>Azure tablo depolama ve Visual Studio ile çalışmaya başlama nasıl bağlı hizmetler

[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

Bu makalede oluşturduğunuz veya Visual Studio kullanarak bir ASP.NET Core projesi bir Azure depolama hesabında başvurulan sonra Visual Studio'da Azure tablo depolama kullanmaya başlama işlemini açıklamaktadır **bağlı hizmetler** özelliği. **Bağlı hizmetler** işlemi projenizde Azure depolamaya erişmek için uygun NuGet paketlerini yükler ve proje yapılandırma dosyalarınızı depolama hesabı için bağlantı dizesi ekler. (Bkz [depolama belgeleri](https://azure.microsoft.com/documentation/services/storage/) Azure depolama hakkında genel bilgiler.)

Azure Table storage hizmeti büyük miktarlarda yapısal veriyi depolamanızı sağlar. Kimliği doğrulanmış çağrılarından içindeki ve Azure Bulutu dışındaki kabul eden bir NoSQL veri deposu hizmetidir. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir. Azure tablo depolama kullanma hakkında daha fazla genel bilgi için bkz: [.NET kullanarak Azure tablo depolama ile çalışmaya başlama](../storage/storage-dotnet-how-to-use-tables.md).

Başlamak için önce depolama hesabınızdaki bir tablo oluşturun. Bu makalede daha sonra C# dilinde bir tablo oluşturma ve ekleme, değiştirme, okuma ve tablo girdilerini kaldırma gibi temel tablo işlemlerini nasıl gerçekleştireceğinizi gösterir.  Kod, .NET için Azure depolama istemci kitaplığı kullanır. ASP.NET hakkında daha fazla bilgi için bkz: [ASP.NET](https://www.asp.net).

Bazı Azure depolama API'leri uyumsuzdur ve bu makalede kod zaman uyumsuz yöntemler kullanıldığını varsayar. Bkz: [zaman uyumsuz programlama](https://docs.microsoft.com/dotnet/csharp/async) daha fazla bilgi için.

## <a name="access-tables-in-code"></a>Kod erişim tablolarında

ASP.NET Core projeleri tablolarında erişmek için Azure tablo depolaması erişen tüm C# kaynak dosyaları için aşağıdaki öğeleri ekleyin gerekir.

1. Gerekli Ekle `using` ifadeleri:

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Table;
    using System.Threading.Tasks;
    ```

1. Alma bir `CloudStorageAccount` depolama hesap bilgilerini temsil eden nesne. Depolama hesabınızı ve appSettings.json depolama bağlantı dizesinde bulabilirsiniz hesap anahtarı adını kullanarak, aşağıdaki kodu kullanın:

    ```csharp
        CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
                "<name>", "<account-key>"), true);
    ```

1. Alma bir `CloudTableClient` depolama hesabınızda tablosu nesnelerini başvurmak için:

    ```csharp
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Alma bir `CloudTable` belirli bir tablo ve varlıklar başvurmak için başvuru nesnesi:

    ```csharp
    // Get a reference to a table named "peopleTable"
    CloudTable peopleTable = tableClient.GetTableReference("peopleTable");
    ```

## <a name="create-a-table-in-code"></a>Kod içinde bir tablo oluşturma

Azure tablo oluşturmak için zaman uyumsuz bir yöntem oluşturmak ve içine çağırmak `CreateIfNotExistsAsync()`:

```csharp
async void CreatePeopleTableAsync()
{
    // Create the CloudTable if it does not exist
    await peopleTable.CreateIfNotExistsAsync();
}
```
    
## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme

Tabloya bir varlık eklemek için varlığınızın özelliklerini tanımlayan bir sınıf oluşturun. Aşağıdaki kod adlı bir varlık sınıfı tanımlar `CustomerEntity` , müşterinin ad son adı ve satır anahtarı bölüm anahtarı olarak kullanır.

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

Tablo varlıkları kullanın ilgili işlemler `CloudTable` daha önce oluşturduğunuz nesne [erişim kodu tablolarında](#access-tables-in-code). `TableOperation` Nesneyi temsil ediyor, işlemin gerçekleştirilmesi için. Aşağıdaki kod örneğinde nasıl oluşturulacağını gösterir. bir `CloudTable` nesnesi ve bir `CustomerEntity` nesne. İşlemi hazırlamak için bir `TableOperation` müşteri varlığını tabloya yerleştirmek üzere oluşturulur. Son olarak, işlem çağrılarak çalıştırılır `CloudTable.ExecuteAsync`.

```csharp
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

Birden çok varlık, bir tek bir yazma işleminde bir tabloya ekleyebilirsiniz. Aşağıdaki kod örneği iki varlık nesnesi ("Jeff Smith" ve "Ben Smith") oluşturur, eklenmeye bir `TableBatchOperation` kullanarak nesne `Insert` yöntemi, ve sonra işlemi çağrılarak başlatılır `CloudTable.ExecuteBatchAsync`.

```csharp
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

## <a name="get-all-of-the-entities-in-a-partition"></a>Tüm varlıklar bir bölümde Al

Bir bölümdeki varlıkların tümü için bir tabloyu sorgulamak üzere kullanmak bir `TableQuery` nesne. Aşağıdaki kod örneği, ‘Smith’in bölüm anahtarı olduğu varlıklar için bir filtre belirtir. Bu örnek sorgu sonuçlarındaki her varlığın alanlarını konsola yazdırır.

```csharp
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

Tek, belirli bir varlığı almak için sorgu yazabilirsiniz. Aşağıdaki kod bir `TableOperation` adlı 'Ben Smith' müşterisini belirlemek nesne. Yöntemi yalnızca bir varlık yerine bir koleksiyon, döndürülen değer döndürür ve `TableResult.Result` olduğu bir `CustomerEntity` nesne. Bir sorguda hem Bölüm hem de satır anahtarını belirterek olan en hızlı yolu, tek bir varlık kümesinden varlık alma `Table` hizmeti.

```csharp
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

Bunu bulduktan sonra bir varlığı silebilirsiniz. Aşağıdaki kod arar ve "Ben Smith" adlı bir müşteri varlığı siler:

```csharp
// Create a retrieve operation that expects a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

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
