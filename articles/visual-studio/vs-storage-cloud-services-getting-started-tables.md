---
title: Tablo depolama ve Visual Studio ile çalışmaya başlama (bulut Hizmetleri) Hizmetleri bağlı | Microsoft Docs
description: Visual Studio kullanarak bir depolama hesabına bağlandıktan sonra bir bulut hizmeti projesini Visual Studio'da Azure tablo depolama ile çalışmaya başlamak nasıl bağlı hizmetler
services: storage
author: ghogen
manager: douge
ms.assetid: a3a11ed8-ba7f-4193-912b-e555f5b72184
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: cec8ab9d678ff559176580fa8eccc261f449f4c5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60362008"
---
# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Azure tablo depolama ve Visual Studio ile çalışmaya başlama (bulut) bağlı Hizmetleri projeleri
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede oluşturduğunuz veya Visual Studio kullanarak bir Azure depolama hesabı bulut Hizmetleri projesinde başvurulan sonra Visual Studio'da Azure tablo depolama kullanmaya başlama işlemini açıklamaktadır **bağlı hizmet Ekle** iletişim. **Bağlı hizmet Ekle** işlemi projenizde Azure depolamaya erişmek için uygun NuGet paketlerini yükler ve proje yapılandırma dosyalarınızı depolama hesabı için bağlantı dizesi ekler.

Azure Table storage hizmeti büyük miktarlarda yapısal veriyi depolamanızı sağlar. Kimliği doğrulanmış çağrılarından içindeki ve Azure Bulutu dışındaki kabul eden bir NoSQL veri deposu hizmetidir. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir.

Başlamak için önce depolama hesabınızdaki bir tablo oluşturmanız gerekir. Kodda bir Azure tablosu oluşturma ve temel tablo ve ekleme, değiştirme, okuma ve tablo varlıkları okuma gibi varlık işlemlerini gerçekleştirme göstereceğiz. Örnekler C dilinde yazılan\# kullanın ve kod [.NET için Microsoft Azure depolama istemci Kitaplığı](https://msdn.microsoft.com/library/azure/dn261237.aspx).

**NOT:** Azure depolama alanına çağrı kullanıma gerçekleştirmek API'lerden bazılarını uyumsuzdur. Bkz: [Async ve Await ile zaman uyumsuz programlama](https://msdn.microsoft.com/library/hh191443.aspx) daha fazla bilgi için. Aşağıdaki kod, zaman uyumsuz programlama yöntemler kullanıldığını varsayar.

* Bkz: [.NET kullanarak Azure tablo depolama ile çalışmaya başlama](../storage/storage-dotnet-how-to-use-tables.md) program aracılığıyla tablolar düzenleme hakkında daha fazla bilgi.
* Bkz: [depolama belgeleri](https://azure.microsoft.com/documentation/services/storage/) Azure depolama hakkında genel bilgiler.
* Bkz: [bulut Hizmetleri belgeleri](https://azure.microsoft.com/documentation/services/cloud-services/) Azure bulut hizmetleri hakkında genel bilgiler.
* Bkz: [ASP.NET](https://www.asp.net) ASP.NET uygulamalarını programlama hakkında daha fazla bilgi.

## <a name="access-tables-in-code"></a>Kod erişim tablolarında
Bulut hizmeti projeleri tablolarında erişmek için Azure tablo depolaması erişen tüm C# kaynak dosyaları için aşağıdaki öğeleri ekleyin gerekir.

1. Bu ad alanı bildirimi C# dosyası üst kısmındaki eklediğinizden emin olun **kullanarak** deyimleri.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Alma bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından bir depolama bağlantı dizesi ve depolama hesabı bilgileri almak için aşağıdaki kodu kullanın.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
   > [!NOTE]
   > Tüm kod önünde Yukarıdaki kod, aşağıdaki örnekleri kullanın.
   > 
   > 
3. Alma bir **CloudTableClient** tablo depolama hesabınızda başvuru nesnelerine nesne.
   
         // Create the table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. Alma bir **CloudTable** belirli bir tablo ve varlıklar başvurmak için başvuru nesnesi.
   
        // Get a reference to a table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Kod içinde bir tablo oluşturma
Azure tablo oluşturmak için yalnızca bir çağrı ekleyin **Createıfnotexistsasync** için size sonra bir **CloudTable** nesne "kod tablolara erişen" bölümünde açıklandığı gibi.

    // Create the CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme
Tabloya bir varlık eklemek için varlığınızın özelliklerini tanımlayan bir sınıf oluşturun. Aşağıdaki kod adlı bir varlık sınıfı tanımlar **CustomerEntity** , müşterinin ad satır anahtarını ve Soyadı bölüm anahtarı olarak kullanır.

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

Varlıkları içeren tablo işlemleri gerçekleştirilir kullanarak **CloudTable** "Access tabloları kodda." içinde daha önce oluşturduğunuz nesnesi **TableOperation** nesne yapılacak işlemi temsil eder. Aşağıdaki kod örneğinde nasıl oluşturulacağını gösterir. bir **CloudTable** nesnesi ve bir **CustomerEntity** nesne. İşlemi hazırlamak için bir **TableOperation** müşteri varlığını tabloya yerleştirmek üzere oluşturulur. Son olarak, işlem çağrılarak çalıştırılır **CloudTable.ExecuteAsync**.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a>Toplu işlem varlık yerleştirme
Birden çok varlık, bir tek bir yazma işleminde bir tabloya ekleyebilirsiniz. Aşağıdaki kod örneği iki varlık nesnesi ("Jeff Smith" ve "Ben Smith") oluşturur, eklenmeye bir **TableBatchOperation** Ekle yöntemi kullanılarak nesne ve çağırarak işlemi başlatır  **CloudTable.ExecuteBatchAsync**.

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

## <a name="get-all-of-the-entities-in-a-partition"></a>Tüm varlıklar bir bölümde Al
Bir bölümdeki varlıkların tümü için bir tabloyu sorgulamak üzere kullanmak bir **TableQuery** nesne. Aşağıdaki kod örneği, ‘Smith’in bölüm anahtarı olduğu varlıklar için bir filtre belirtir. Bu örnek sorgu sonuçlarındaki her varlığın alanlarını konsola yazdırır.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

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

    return View();


## <a name="get-a-single-entity"></a>Tek bir varlık alma
Tek, belirli bir varlığı almak için sorgu yazabilirsiniz. Aşağıdaki kod bir **TableOperation** adlı 'Ben Smith' müşterisini belirlemek nesne. Bu yöntem yalnızca bir varlık yerine bir koleksiyon ve döndürülen değer döndürür **TableResult.Result** olduğu bir **CustomerEntity** nesne. Olan en hızlı yolu, tek bir varlık kümesinden varlık alma sorguda hem Bölüm hem de satır anahtarını belirterek **tablo** hizmeti.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Bir varlığı silme
Bunu bulduktan sonra bir varlığı silebilirsiniz. Aşağıdaki kod bir müşteri varlığı için "Ben Smith" adlı görünür ve bulursa, onu siler.

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

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

