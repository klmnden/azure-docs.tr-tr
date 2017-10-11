---
title: "Tablo depolama ve Visual Studio ile çalışmaya nasıl başlayacağınız bağlı Hizmetleri (ASP.NET Core) | Microsoft Docs"
description: "Visual Studio'da ASP.NET Core projesinde Azure Table storage ile Visual Studio kullanarak bir depolama hesabı bağlandıktan sonra nasıl başlayacağınızı Hizmetleri bağlı"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: c3c451d1-71ff-4222-a348-c41c98a02b85
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 8d05fe3ed9a5c66f186a930d4107162c1f322c05
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-get-started-with-azure-table-storage-and-visual-studio-connected-services"></a>Azure tablo depolaması ve Visual Studio ile çalışmaya nasıl başlayacağınız Hizmetleri bağlı
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede nasıl oluşturduğunuz veya Visual Studio kullanarak bir ASP.NET Core projesini bir Azure depolama hesabında başvurulan sonra Visual Studio'da Azure tablo depolaması ile çalışmaya başlamak **bağlı Hizmetleri Ekle** iletişim.

Azure Table depolama birimi hizmeti, büyük miktarlarda yapılandırılmış verileri depolamak sağlar. Hizmet, kimliği doğrulanmış çağrılarından içinden ve dışından Azure bulut kabul eden bir NoSQL veri deposudur. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir.

**Bağlı Hizmetleri Ekle** işlemi Azure depolama projenize erişmek için uygun NuGet paketlerini yükler ve proje yapılandırma dosyalarınızı depolama hesabı için bağlantı dizesi ekler.

Azure Table storage kullanma hakkında daha fazla genel bilgi için bkz: [.NET kullanarak Azure Table storage'ı kullanmaya başlama](../storage/storage-dotnet-how-to-use-tables.md).

Başlamak için önce bir tablo depolama hesabınızdaki oluşturmanız gerekir. Kodda bir Azure tablo oluşturulacağını göstereceğiz. Ayrıca, temel tablo ve ekleme, değiştirme, okuma ve tablo varlıkları okuma gibi varlık işlemlerini gerçekleştirmek nasıl göstereceğiz. Örnekler C yazılır\# kod ve .NET için Azure Storage istemci kitaplığı kullanın.

**Not** -ASP.NET Core Azure depolamaya giden çağrıları gerçekleştirmek API'leri bazıları zaman uyumsuzdur. Bkz: [uyumsuz ve bekleme ile zaman uyumsuz programlama](http://msdn.microsoft.com/library/hh191443.aspx) daha fazla bilgi için. Aşağıdaki kodu, zaman uyumsuz programlama yöntemleri kullanıldığı varsayılmaktadır.

## <a name="access-tables-in-code"></a>Kod erişim tablolarında
ASP.NET Core projeleri tablolarda erişmek için Azure tablo depolaması erişen tüm C# kaynak dosyaları için aşağıdaki öğeleri eklemeniz gerekir.

1. Ad alanı bildirimlerini dosyanın üst kısmındaki C# bu eklediğinizden emin olun **kullanarak** deyimleri.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Alma bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Almak için aşağıdaki kodu kullanın depolama bağlantı dizesini ve Azure hizmet yapılandırma depolama hesabı bilgileri.
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
   
    **Not** -tüm kod önünde Yukarıdaki kod aşağıdaki örneklerde kullanın.
3. Alma bir **CloudTableClient** depolama hesabınızdaki tablo nesneleri başvurmak için.  
   
        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. Alma bir **CloudTable** belirli bir tablo ve varlıkları başvurmak için başvuru nesnesi.
   
        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Kod içinde bir tablo oluşturma
Azure tablo oluşturmak için yalnızca bir çağrı ekleyin **CreateIfNotExistsAsync()**.

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme
Bir tabloya bir varlık eklemek için varlığınızın özelliklerini tanımlayan bir sınıf oluşturun. Aşağıdaki kod olarak adlandırılan bir varlık sınıfı tanımlar **CustomerEntity** , müşterinin adını satır anahtarını ve Soyadı bölüm anahtarı olarak kullanır.

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

Varlıkları içeren tablo işlemleri yapılır kullanarak **CloudTable** "Access tabloları kodda." içinde daha önce oluşturduğunuz nesnesi **TableOperation** nesnesi yapılması işlemi temsil eder. Aşağıdaki kod örneğinde nasıl oluşturulacağını gösterir bir **CloudTable** nesne ve **CustomerEntity** nesnesi. İşlemi hazırlamak için bir **TableOperation** müşteri varlığını tabloya yerleştirmek için oluşturulur. Son olarak, işlem CloudTable.ExecuteAsync çağrılarak çalıştırılır.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Toplu işlem varlık yerleştirme
Birden çok varlık, bir tek bir yazma işleminde bir tabloya ekleyebilirsiniz. Aşağıdaki kod örneği iki varlık nesnesi ("Jeff Smith" ve "Ben Smith") oluşturur, eklenmektedir bir **TableBatchOperation** kullanarak nesne **Ekle** yöntemi, ve ardından CloudTable.ExecuteBatchAsync çağırarak işlemi başlatır.

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

