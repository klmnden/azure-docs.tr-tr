---
title: Azure Cosmos DB tablosu .NET standart SDK'sını kullanarak API'yi kullanmaya başlama
description: Azure Cosmos DB tablo API'si kullanarak bulutta yapılandırılmış veri Store.
author: wmengmsft
ms.author: wmeng
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: sample
ms.date: 03/11/2019
ms.openlocfilehash: 0a329722b65e407f011016a1f55e86ef17b47d70
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65192396"
---
# <a name="get-started-with-azure-cosmos-db-table-api-and-azure-table-storage-using-the-net-sdk"></a>.NET SDK kullanarak Azure Cosmos DB tablo API'si ve Azure tablo depolama ile çalışmaya başlama

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

[!INCLUDE [storage-table-applies-to-storagetable-and-cosmos](../../includes/storage-table-applies-to-storagetable-and-cosmos.md)]

Azure Cosmos DB tablo API'si veya Azure tablo depolama tasarım daha az bir şema ile bulutta bir anahtar/öznitelik sağlayan veri depolama, yapılandırılmış NoSQL depolamak için kullanabilirsiniz. Azure Cosmos DB tablo API'si ve tablo depolama şema daha az olduğundan, ihtiyaçları, uygulama geliştikçe verilerinizi uyarlamak da kolaylaşır. Kullanıcı verileri için web uygulamaları, adres defterleri, cihaz bilgileri veya hizmetiniz için gerekli meta verileri diğer türleri gibi esnek veri kümelerini depolamak için Azure Cosmos DB tablo API'si veya tablo Depolama'yı kullanabilirsiniz. 

Bu öğreticide, nasıl kullanılacağını gösteren bir örnek açıklanmaktadır [.NET için Microsoft Azure Cosmos DB tablo Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) Azure Cosmo DB tablo API'si ve Azure tablo depolama senaryolarında. Azure hizmete özgü bağlantı kullanmanız gerekir. Bu senaryoları kullanarak incelenmektedir C# tabloları oluşturma işlemini göstermektedir örnekler ekleme / güncelleştirme veri, veri sorgulama ve tabloları silin.

## <a name="prerequisites"></a>Önkoşullar

Bu örneği başarıyla tamamlamak için aşağıdakiler gerekir:

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

* [.NET için Microsoft Azure CosmosDB tablo Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) -bu kitaplığı için .NET Standard ve .NET framework şu anda kullanılabilir. 

* [Azure Cosmos DB tablo API'si hesabı](create-table-dotnet.md#create-a-database-account).

## <a name="create-an-azure-cosmos-db-table-api-account"></a>Azure Cosmos DB Tablo API’si hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="create-a-net-console-project"></a>Bir .NET konsol projesi oluşturun

Visual Studio'da yeni bir .NET konsol uygulaması oluşturun. Aşağıdaki adımlar Visual Studio 2017’de bir konsol uygulaması oluşturmayı gösterir. Adımlar Visual Studio’nun diğer sürümlerinde de benzerdir. Herhangi bir türde bir Azure bulut hizmeti veya web uygulaması da dahil olmak üzere, .NET uygulaması ve Masaüstü ve mobil uygulamaları, Azure Cosmos DB tablo kitaplığı kullanabilirsiniz. Bu kılavuzda, sadeleştirmek için konsol uygulaması kullanmaktayız.

1. **Dosya** > **Yeni** > **Proje**’yi seçin.

1. Seçin **yüklü** > **Visual C#**   >  **konsol uygulaması (.NET Core)**.

1. İçinde **adı** alan, uygulamanız için bir ad girin, örneğin, **CosmosTableSamples** (, farklı bir ad gerektiğinde sağlayabilirsiniz).

1. **Tamam**’ı seçin.

Bu örnekte tüm kod örnekleri konsol uygulamanızın Main() yöntemine eklenebilir **Program.cs** dosya.

## <a name="install-the-required-nuget-package"></a>Gerekli NuGet paketini yükle

NuGet paketini edinmek için şu adımları izleyin:

1. **Çözüm Gezgini**'nde projenize sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.

1. Çevrimiçi olarak arayın `Microsoft.Azure.Cosmos.Table`, `Microsoft.Extensions.Configuration`, `Microsoft.Extensions.Configuration.Json`, `Microsoft.Extensions.Configuration.Binder` seçip **yükleme** Microsoft Azure Cosmos DB tablo kitaplığı yüklemek için.

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma

1. Gelen [Azure portalında](https://portal.azure.com/), tıklayın **bağlantı dizesi**. Pencerenin sağ tarafındaki kopyala düğmesini kullanarak **PRIMARY CONNECTION STRING**'i kopyalayın.

   ![Bağlantı Dizesi bölmesindeki PRIMARY CONNECTION STRING’i görüntüleyin ve kopyalayın](./media/create-table-dotnet/connection-string.png)
   
1. Bağlantı dizenizi yapılandırmak için visual Studio'dan sağ projenizde tıklayın **CosmosTableSamples**.

1. Seçin **ekleme** ardından **yeni öğe**. Yeni bir dosya oluşturun **Settings.json** dosya türü ile **TypeScript JSON yapılandırma** dosya. 

1. Settings.json dosyasındaki kodu aşağıdaki kodla değiştirin ve birincil bağlantı dizenizi atayabilirsiniz:

   ```csharp
   {
   "StorageConnectionString": <Primary connection string of your Azure Cosmos DB account>
   }
   ```

1. Projenize sağ tıklayın **CosmosTableSamples**. Seçin **Ekle**, **yeni öğe** ve adlı bir sınıf ekleyin **AppSettings.cs**.

1. AppSettings.cs dosyaya aşağıdaki kodu ekleyin. Bu dosya Settings.json dosyasından bağlantı dizesini okur ve yapılandırma parametresine atar:

   ```csharp
   namespace CosmosTableSamples
   {
    using Microsoft.Extensions.Configuration;
    public class AppSettings
    {
        public string StorageConnectionString { get; set; }
        public static AppSettings LoadAppSettings()
        {
            IConfigurationRoot configRoot = new ConfigurationBuilder()
                .AddJsonFile("Settings.json")
                .Build();
            AppSettings appSettings = configRoot.Get<AppSettings>();
            return appSettings;
        }
    }
   }
   ```

## <a name="parse-and-validate-the-connection-details"></a>Ayrıştırma ve bağlantı ayrıntılarını doğrulayın 

1. Projenize sağ tıklayın **CosmosTableSamples**. Seçin **Ekle**, **yeni öğe** ve adlı bir sınıf ekleyin **Common.cs**. Bağlantı ayrıntılarını doğrulayın ve bu sınıf içinde bir tablo oluşturmak için kod yazacaksınız.

1. Bir yöntemi tanımlamak `CreateStorageAccountFromConnectionString` aşağıda gösterildiği gibi. Bu yöntem, bağlantı dizesi ayrıntılarını ayrıştırma ve hesap adını ve hesap anahtarı Ayrıntıları "Settings.json" dosyasında sağlanan geçerli olduğunu doğrulayın. 

   ```csharp
   public static CloudStorageAccount CreateStorageAccountFromConnectionString(string storageConnectionString)
    {
            CloudStorageAccount storageAccount;
            try
            {
                storageAccount = CloudStorageAccount.Parse(storageConnectionString);
            }
            catch (FormatException)
            {
                Console.WriteLine("Invalid storage account information provided. Please confirm the AccountName and AccountKey are valid in the app.config file - then restart the application.");
                throw;
            }
            catch (ArgumentException)
            {
                Console.WriteLine("Invalid storage account information provided. Please confirm the AccountName and AccountKey are valid in the app.config file - then restart the sample.");
                Console.ReadLine();
                throw;
            }

            return storageAccount;
        }
   ```


## <a name="create-a-table"></a>Tablo oluşturma 

[CloudTableClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.table.cloudtableclient) sınıfı, Table Storage’da depolanan tabloları ve varlıkları almanızı sağlar. Cosmos DB tablo API'si hesabı size herhangi bir tablo olmadığından, ekleyelim `CreateTableAsync` yönteme **Common.cs** sınıfı bir tablo oluşturun:

```csharp
public static async Task<CloudTable> CreateTableAsync(string tableName)
  {
    string storageConnectionString = AppSettings.LoadAppSettings().StorageConnectionString;

    // Retrieve storage account information from connection string.
    CloudStorageAccount storageAccount = CreateStorageAccountFromConnectionString(storageConnectionString);

    // Create a table client for interacting with the table service
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient(new TableClientConfiguration());

    Console.WriteLine("Create a Table for the demo");

    // Create a table client for interacting with the table service 
    CloudTable table = tableClient.GetTableReference(tableName);
    if (await table.CreateIfNotExistsAsync())
    {
      Console.WriteLine("Created Table named: {0}", tableName);
    }
    else
    {
      Console.WriteLine("Table {0} already exists", tableName);
    }

    Console.WriteLine();
    return table;
}
```

## <a name="define-the-entity"></a>Varlık tanımlayın 

Varlık eşleme C# özel bir sınıf kullanarak nesneleri türetilen [TableEntity](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.table.tableentity). Tabloya bir varlık eklemek için varlığınızın özelliklerini tanımlayan bir sınıf oluşturun.

Projenize sağ tıklayın **CosmosTableSamples**. Seçin **Ekle**, **yeni klasör** olarak adlandırın **Model**. Adlı bir sınıf Model klasörü içinde eklemek **CustomerEntity.cs** ve aşağıdaki kodu ekleyin.

```csharp
namespace CosmosTableSamples.Model
{
    using Microsoft.Azure.Cosmos.Table;
    public class CustomerEntity : TableEntity
    {
        public CustomerEntity()
        {
        }

        public CustomerEntity(string lastName, string firstName)
        {
            PartitionKey = lastName;
            RowKey = firstName;
        }

        public string Email { get; set; }
        public string PhoneNumber { get; set; }
    }
}
```

Bu kod, bölüm anahtarı olarak soyadını ve satır anahtarı müşterinin ad kullanan bir varlık sınıfı tanımlar. Birlikte, bir varlığın bölüm ve sıra anahtarı varlığı tabloda benzersiz şekilde tanımlar. Aynı bölüm anahtarına sahip varlıklar farklı bölüm anahtarlı varlıklara göre daha hızlı sorgulanabilir ancak farklı bölüm anahtarlarının kullanılması paralel işlemler için büyük ölçeklendirme sağlar. Tablolarda depolanacak varlıklar, türetilen, desteklenen bir türde olmalıdır [TableEntity](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.table.tableentity) sınıfı. Bir tabloda depolamak istediğiniz varlık özellikleri, türün genel özellikleri olmalı ve değerleri hem almayı hem de ayarlamayı desteklemelidir. Ayrıca, varlık türü bir parametresiz oluşturucu kullanıma açmalıdır.

## <a name="insert-or-merge-an-entity"></a>Ekleme veya bir varlık Birleştir

Aşağıdaki kod örneği bir varlık nesnesi oluşturur ve tabloya ekler. InsertOrMerge yöntemi içinde [TableOperation](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.table.tableoperation) sınıfı eklemek veya bir varlık birleştirmek için kullanılır. [CloudTable.ExecuteAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.table.cloudtable.executeasync?view=azure-dotnet) yöntemi, işlemi yürütmek için çağrılır. 

Projenize sağ tıklayın **CosmosTableSamples**. Seçin **Ekle**, **yeni öğe** ve adlı bir sınıf ekleyin **SamplesUtils.cs**. Bu sınıf, varlıkları CRUD işlemleri gerçekleştirmek için gereken tüm kod depolar. 

```csharp
public static async Task<CustomerEntity> InsertOrMergeEntityAsync(CloudTable table, CustomerEntity entity)
    {
      if (entity == null)
    {
       throw new ArgumentNullException("entity");
    }
    try
    {
       // Create the InsertOrReplace table operation
       TableOperation insertOrMergeOperation = TableOperation.InsertOrMerge(entity);

       // Execute the operation.
       TableResult result = await table.ExecuteAsync(insertOrMergeOperation);
       CustomerEntity insertedCustomer = result.Result as CustomerEntity;
        
        // Get the request units consumed by the current operation. RequestCharge of a TableResult is only applied to Azure CosmoS DB 
        if (result.RequestCharge.HasValue)
          {
            Console.WriteLine("Request Charge of InsertOrMerge Operation: " + result.RequestCharge);
          }

        return insertedCustomer;
        }
        catch (StorageException e)
        {
          Console.WriteLine(e.Message);
          Console.ReadLine();
          throw;
        }
    }
```

### <a name="get-an-entity-from-a-partition"></a>Bir bölümün dışında bir varlık alma

Bir bölümün dışında varlık altında alma yöntemini kullanarak alabilirsiniz [TableOperation](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.table.tableoperation) sınıfı. Aşağıdaki kod örneği bölüm anahtarı satır anahtarı, bir müşteri varlığı e-posta ve telefon numarasını alır. Bu örnek, varlık için sorgu için kullanılan istek birimleri çıkış yazdırır. Bir varlık için sorgulamak için aşağıdaki kodu ekleyin. **SamplesUtils.cs** dosyası: 

```csharp
public static async Task<CustomerEntity> RetrieveEntityUsingPointQueryAsync(CloudTable table, string partitionKey, string rowKey)
    {
      try
      {
        TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>(partitionKey, rowKey);
        TableResult result = await table.ExecuteAsync(retrieveOperation);
        CustomerEntity customer = result.Result as CustomerEntity;
        if (customer != null)
        {
          Console.WriteLine("\t{0}\t{1}\t{2}\t{3}", customer.PartitionKey, customer.RowKey, customer.Email, customer.PhoneNumber);
        }

        // Get the request units consumed by the current operation. RequestCharge of a TableResult is only applied to Azure CosmoS DB 
        if (result.RequestCharge.HasValue)
        {
           Console.WriteLine("Request Charge of Retrieve Operation: " + result.RequestCharge);
        }

        return customer;
        }
        catch (StorageException e)
        {
           Console.WriteLine(e.Message);
           Console.ReadLine();
           throw;
        }
    }
```

## <a name="delete-an-entity"></a>Bir varlığı silme

Bir varlığı güncelleştirmek için gösterilen aynı yöntemi kullanarak, bir varlığı aldıktan sonra kolayca silebilirsiniz. Aşağıdaki kod bir müşteri girişini alır ve siler. Bir varlığı silmek için aşağıdaki kodu ekleyin. **SamplesUtils.cs** dosyası: 

```csharp
public static async Task DeleteEntityAsync(CloudTable table, CustomerEntity deleteEntity)
   {
     try
     {
        if (deleteEntity == null)
     {
        throw new ArgumentNullException("deleteEntity");
     }

    TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
    TableResult result = await table.ExecuteAsync(deleteOperation);

    // Get the request units consumed by the current operation. RequestCharge of a TableResult is only applied to Azure CosmoS DB 
    if (result.RequestCharge.HasValue)
    {
       Console.WriteLine("Request Charge of Delete Operation: " + result.RequestCharge);
    }

    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="execute-the-crud-operations-on-sample-data"></a>Örnek verileri CRUD işlemleri yürütün

Tablo, INSERT veya birleştirme varlıkları oluşturma yöntemleri tanımladıktan sonra bu yöntemler, örnek veriler üzerinde çalıştırın. Bunu yapmak için sağ, projeye tıklayın **CosmosTableSamples**. Seçin **Ekle**, **yeni öğe** ve adlı bir sınıf ekleyin **basicsamples.cs dosyasını** ve aşağıdaki kodu ekleyin. Bu kod, bir tablo oluşturur, varlıkları eklenir. Varlık ve proje sonundaki tabloda silmek istiyorsanız açıklamalarını kaldırın `table.DeleteIfExistsAsync()` ve `SamplesUtils.DeleteEntityAsync(table, customer)` aşağıdaki kodu yöntemleri:

```csharp
using System;
namespace CosmosTableSamples
{
    using System.Threading.Tasks;
    using Microsoft.Azure.Cosmos.Table;
    using Model;

    class BasicSamples
    {
        public async Task RunSamples()
        {
            Console.WriteLine("Azure Cosmos DB Table - Basic Samples\n");
            Console.WriteLine();

            string tableName = "demo" + Guid.NewGuid().ToString().Substring(0, 5);

            // Create or reference an existing table
            CloudTable table = await Common.CreateTableAsync(tableName);

            try
            {
                // Demonstrate basic CRUD functionality 
                await BasicDataOperationsAsync(table);
            }
            finally
            {
                // Delete the table
                // await table.DeleteIfExistsAsync();
            }
        }

        private static async Task BasicDataOperationsAsync(CloudTable table)
        {
            // Create an instance of a customer entity. See the Model\CustomerEntity.cs for a description of the entity.
            CustomerEntity customer = new CustomerEntity("Harp", "Walter")
            {
                Email = "Walter@contoso.com",
                PhoneNumber = "425-555-0101"
            };

            // Demonstrate how to insert the entity
            Console.WriteLine("Insert an Entity.");
            customer = await SamplesUtils.InsertOrMergeEntityAsync(table, customer);

            // Demonstrate how to Update the entity by changing the phone number
            Console.WriteLine("Update an existing Entity using the InsertOrMerge Upsert Operation.");
            customer.PhoneNumber = "425-555-0105";
            await SamplesUtils.InsertOrMergeEntityAsync(table, customer);
            Console.WriteLine();

            // Demonstrate how to Read the updated entity using a point query 
            Console.WriteLine("Reading the updated Entity.");
            customer = await SamplesUtils.RetrieveEntityUsingPointQueryAsync(table, "Harp", "Walter");
            Console.WriteLine();

            // Demonstrate how to Delete an entity
            //Console.WriteLine("Delete the entity. ");
            //await SamplesUtils.DeleteEntityAsync(table, customer);
            //Console.WriteLine();
        }
    }
}
```

Önceki kodun "tanıtım" ile başlayan bir tablo oluşturur ve oluşturulan GUID tablo adına eklenir. Bu, ardından "Walter Harp" olarak adı ve Soyadı ile müşteri varlığı ekler ve daha sonra bu kullanıcının telefon numarasını güncelleştirir. 

Bu öğreticide, tablo API'si hesapta depolanan veriler üzerinde temel CRUD işlemleri gerçekleştirmek için kod yerleşik. Gibi gelişmiş işlemleri gerçekleştirebilirsiniz: bir bölüm içindeki tüm verilerin toplu ekleme verileri, sorgu bölüm içindeki verileri, listeleri tablo adları belirtilen bir önek ile başlayan hesabında bir dizi sorgu. Tam örnek form indirebileceğiniz [azure-cosmos-table-dotnet-core-getting-started](https://github.com/Azure-Samples/azure-cosmos-table-dotnet-core-getting-started) GitHub deposu. [AdvancedSamples.cs](https://github.com/Azure-Samples/azure-cosmos-table-dotnet-core-getting-started/blob/master/CosmosTableSamples/AdvancedSamples.cs) sınıfı, verilerinizde gerçekleştirebileceğiniz daha fazla işlem vardır.  

## <a name="run-the-project"></a>Projeyi çalıştırma

Şimdi çözüm oluşturun ve projeyi çalıştırmak için F5 tuşuna basın. Projeyi çalıştırdığınızda, komut isteminde aşağıdaki çıktıyı görürsünüz:

![Komut istemi çıkışı](./media/tutorial-develop-table-standard/output-from-sample.png)

Projeyi çalıştırırken Settings.json dosyasına bulunamıyor diyen bir hata alırsanız, proje ayarları için aşağıdaki XML giriş ekleyerek çözülebilir. CosmosTableSamples üzerinde sağ tıklayın, Düzen CosmosTableSamples.csproj seçin ve aşağıdaki ItemGroup ekleyin: 

```csharp
  <ItemGroup>
    <None Update="Settings.json">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </None>
  </ItemGroup>
```
Artık Azure portalında oturum açıp veri tablosunda var olduğundan emin olun. 

![Sonuçlarını portalında](./media/tutorial-develop-table-standard/results-in-portal.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık sonraki öğreticiye devam edin ve Azure Cosmos DB tablo API'si hesabı için veri geçirmeyi öğrenin. 

> [!div class="nextstepaction"]
>[Verilerini sorgulama](../cosmos-db/table-import.md)
