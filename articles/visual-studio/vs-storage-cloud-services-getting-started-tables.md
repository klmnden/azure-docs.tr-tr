---
title: Tablo depolama ve Visual Studio ile çalışmaya başlama bağlı Hizmetleri (bulut Hizmetleri) | Microsoft Docs
description: Visual Studio kullanarak bir depolama hesabına bağlanma Hizmetleri bağlandıktan sonra bir bulut hizmeti projesini Visual Studio'da Azure tablo depolaması ile çalışmaya başlamak nasıl
services: storage
author: ghogen
manager: douge
ms.assetid: a3a11ed8-ba7f-4193-912b-e555f5b72184
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: d88e8e85613faa24213b6e12b5ba4f30e3d84f74
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Azure tablo depolaması ve Visual Studio ile çalışmaya başlama (Projeler bulut Hizmetleri) Hizmetleri bağlı
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Genel Bakış
Bu makale, oluşturduğunuz veya Visual Studio kullanarak bir Azure depolama hesabı bulut Hizmetleri projesinde başvurulan sonra Visual Studio'da Azure tablo depolaması ile çalışmaya başlamak açıklamaktadır **bağlı Hizmetleri Ekle** iletişim. **Bağlı Hizmetleri Ekle** işlemi Azure depolama projenize erişmek için uygun NuGet paketlerini yükler ve proje yapılandırma dosyalarınızı depolama hesabı için bağlantı dizesi ekler.

Azure Table depolama birimi hizmeti, büyük miktarlarda yapılandırılmış verileri depolamak sağlar. Kimliği doğrulanmış çağrılarından içinden ve dışından Azure bulut kabul eden bir NoSQL veri deposu hizmetidir. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir.

Başlamak için önce bir tablo depolama hesabınızdaki oluşturmanız gerekir. Kod içinde bir Azure tablosu oluşturma ve temel tablo ve ekleme, değiştirme, okuma ve tablo varlıkları okuma gibi varlık işlemlerini gerçekleştirmek nasıl göstereceğiz. Örnekler C yazılır\# kod ve kullanma [.NET için Microsoft Azure Storage istemci Kitaplığı](https://msdn.microsoft.com/library/azure/dn261237.aspx).

**Not:** bazı Azure depolamaya giden çağrıları gerçekleştirdiğinizde API'leri zaman uyumsuzdur. Bkz: [uyumsuz ve bekleme ile zaman uyumsuz programlama](http://msdn.microsoft.com/library/hh191443.aspx) daha fazla bilgi için. Aşağıdaki kodu, zaman uyumsuz programlama yöntemleri kullanıldığı varsayılmaktadır.

* Bkz: [.NET kullanarak Azure Table storage'ı kullanmaya başlama](../storage/storage-dotnet-how-to-use-tables.md) program aracılığıyla tablolarını düzenleme hakkında daha fazla bilgi.
* Bkz: [Storage belgeleri](https://azure.microsoft.com/documentation/services/storage/) Azure Storage hakkında genel bilgiler.
* Bkz: [bulut Hizmetleri belgelerinde](https://azure.microsoft.com/documentation/services/cloud-services/) Azure bulut hizmetleri hakkında genel bilgi için.
* Bkz: [ASP.NET](http://www.asp.net) ASP.NET uygulamalarını programlama hakkında daha fazla bilgi.

## <a name="access-tables-in-code"></a>Kod erişim tablolarında
Bulut hizmeti projeleri tablolarda erişmek için Azure tablo depolaması erişen tüm C# kaynak dosyaları için aşağıdaki öğeleri eklemeniz gerekir.

1. Ad alanı bildirimlerini dosyanın üst kısmındaki C# bu eklediğinizden emin olun **kullanarak** deyimleri.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Alma bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
   > [!NOTE]
   > Tüm kod önünde Yukarıdaki kod, aşağıdaki örneklerde kullanın.
   > 
   > 
3. Alma bir **CloudTableClient** depolama hesabınızdaki tablo nesneleri başvurmak için.
   
         // Create the table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. Alma bir **CloudTable** belirli bir tablo ve varlıkları başvurmak için başvuru nesnesi.
   
        // Get a reference to a table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Kod içinde bir tablo oluşturma
Azure tablo oluşturmak için yalnızca bir çağrı ekleyin **CreateIfNotExistsAsync** için size sonra bir **CloudTable** nesne "kod tablolarda erişim" bölümünde açıklandığı gibi.

    // Create the CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme
Tabloya bir varlık eklemek için varlığınızın özelliklerini tanımlayan bir sınıf oluşturun. Aşağıdaki kod olarak adlandırılan bir varlık sınıfı tanımlar **CustomerEntity** , müşterinin adını satır anahtarını ve Soyadı bölüm anahtarı olarak kullanır.

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

Varlıkları içeren tablo işlemleri yapılır kullanarak **CloudTable** "Access tabloları kodda." bölümünde daha önce oluşturduğunuz nesnesi **TableOperation** nesnesi yapılması işlemi temsil eder. Aşağıdaki kod örneğinde nasıl oluşturulacağını gösterir bir **CloudTable** nesne ve **CustomerEntity** nesnesi. İşlemi hazırlamak için bir **TableOperation** müşteri varlığını tabloya yerleştirmek için oluşturulur. Son olarak, işlem çağrılarak çalıştırılır **CloudTable.ExecuteAsync**.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a>Toplu işlem varlık yerleştirme
Birden çok varlık, bir tek bir yazma işleminde bir tabloya ekleyebilirsiniz. Aşağıdaki kod örneği iki varlık nesnesi ("Jeff Smith" ve "Ben Smith") oluşturur, eklenmektedir bir **TableBatchOperation** Ekle yöntemi kullanılarak nesne ve ardından çağırarak işlemi başlatır **CloudTable.ExecuteBatchAsync**.

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

## <a name="get-all-of-the-entities-in-a-partition"></a>Tüm varlıkları bir bölüme alma
Tüm varlıkları bir bölüme için bir tabloyu sorgulamak için kullanın bir **TableQuery** nesnesi. Aşağıdaki kod örneği, ‘Smith’in bölüm anahtarı olduğu varlıklar için bir filtre belirtir. Bu örnek sorgu sonuçlarındaki her varlığın alanlarını konsola yazdırır.

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
Tek, belirli bir varlığı almak üzere bir sorgu yazabilirsiniz. Aşağıdaki kod bir **TableOperation** 'Ben Smith' adlı bir müşteri belirtmek için nesne. Bu yöntem yalnızca tek bir varlık yerine bir koleksiyonu ile döndürülen değeri döndürür **TableResult.Result** olan bir **CustomerEntity** nesnesi. Tek bir varlık almanın en hızlı yolu olan bir sorguda hem Bölüm hem de satır anahtarını belirterek **tablo** hizmet.

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
Bunu bulduktan sonra bir varlık silebilirsiniz. "Ben Smith" adlı bir müşteri varlığı için aşağıdaki kodu arar ve bulursa, onu siler.

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

