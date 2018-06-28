---
title: .NET kullanarak Azure Tablo depolamayı ve Azure Cosmos DB Tablo API’sini kullanmaya başlama | Microsoft Docs
description: Azure Tablo Depolama veya Azure Cosmos DB Tablo API’sini kullanarak yapılandırılmış verileri bulutta depolayın.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.devlang: dotnet
ms.topic: sample
ms.date: 03/14/2018
ms.author: sngun
ms.openlocfilehash: d0c587b3d43f7511775a4a114bead96348372bc5
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36959976"
---
# <a name="get-started-with-azure-table-storage-and-the-azure-cosmos-db-table-api-using-net"></a>.NET kullanarak Azure Tablo depolamayı ve Azure Cosmos DB Tablo API’sini kullanmaya başlama
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-applies-to-storagetable-and-cosmos](../../includes/storage-table-applies-to-storagetable-and-cosmos.md)]

Şemasız bir tasarım ile anahtar/öznitelik deposu sağlayarak yapılandırılmış NoSQL verilerini bulutta depolamak için Azure Tablo depolama veya Azure Cosmos DB Tablo API’sini kullanabilirsiniz. Tablo depolama ve Azure Cosmos DB Tablo API’si şemasız olduğu için uygulamanızın ihtiyaçları geliştikçe verilerinizi kolayca uyarlayabilirsiniz. Tablo depolama verilerine ve Azure Cosmos DB Tablo API’sine erişim, çoğu uygulama türü için hızlı ve ekonomik olmanın yanı sıra benzer veri hacimleri için geleneksel SQL’e kıyasla genellikle daha düşük maliyetlidir.

Web uygulamaları için kullanıcı verileri, adres defterleri, cihaz bilgileri veya hizmetiniz için gerekli olan tüm diğer meta veri türleri gibi esnek veri kümelerini depolamak için Tablo depolama veya Azure Cosmos DB Tablo API’sini kullanabilirsiniz. Bir tabloda istediğiniz kadar varlık depolayabilirsiniz ve bir depolama hesabı veya Tablo API’si depolama hesabının ya da Tablo API’si hesabının kapasite limitini dolduracak kadar tablo içerebilir.

### <a name="about-this-sample"></a>Bu örnek hakkında
Bu örnekte ortak Azure Tablo depolama ve Tablo API’si senaryoları için [.NET için Microsoft Azure CosmosDB Tablo Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table)’nın kullanılması gösterilmektedir. Paket adı Azure Cosmos DB ile kullanılabileceğini gösteriyor ancak paket hem Azure Cosmos DB Tablo API’si hem de Azure Tabloları depolamasıyla birlikte kullanılabilir ve her bir hizmetin kendine özgü bir uç noktası vardır. Bu senaryolarda aşağıdakileri yapmayı gösteren C# örnekleri kullanılır:
* Tablo oluşturma ve silme
* Satır ekleme, güncelleştirme ve silme
* Sorgu tabloları

## <a name="prerequisites"></a>Ön koşullar

Bu örneği başarıyla tamamlamak için aşağıdakiler gerekir:

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [.NET için Azure Depolama Ortak Kitaplığı (Önizleme)](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/). Bu, üretim ortamlarında desteklenen gerekli bir önizleme paketidir. 
* [.NET için Microsoft Azure CosmosDB Tablo Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table)
* [.NET için Azure Yapılandırma Yöneticisi](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [Azure depolama hesabı](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Daha fazla örnek
Tablo depolama kullanan diğer örnekler için [.NET’te Azure Table Storage Kullanmaya Başlama](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/). Örnek uygulamayı indirip çalıştırabilir veya GitHub’daki örneğe göz atabilirsiniz.

## <a name="create-an-azure-service-account"></a>Azure hizmet hesabı oluşturma
[!INCLUDE [cosmos-db-create-azure-service-account](../../includes/cosmos-db-create-azure-service-account.md)]

### <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma
İlk Azure depolama hesabınızı oluşturmanın en kolay yolu [Azure Portalı](https://portal.azure.com)’nı kullanmaktır. Daha fazla bilgi için bkz. [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account).

[Azure PowerShell](../storage/common/storage-powershell-guide-full.md), [Azure CLI](../storage/common/storage-azure-cli.md) veya [.NET için Depolama Kaynak Sağlayıcısı İstemci Kitaplığı](/dotnet/api/microsoft.azure.management.storage)’nı da kullanarak Azure Storage esabı oluşturabilirsiniz.

Şu anda bir depolama hesabı oluşturmayı tercih ediyorsanız, yerel bir ortamda kodunuzu çalıştırıp sınamak için Azure Storage öykünücüsü de kullanabilirsiniz. Daha fazla bilgi için bkz. [Geliştirme ve Sınama için Azure Storage Öykünücüsünü Kullanma](../storage/common/storage-use-emulator.md).

### <a name="create-an-azure-cosmos-db-table-api-account"></a>Azure Cosmos DB Tablo API’si hesabı oluşturma
[!INCLUDE [cosmos-db-create-tableapi-account](../../includes/cosmos-db-create-tableapi-account.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma
Ardından, geliştirme ortamınızı Visual Studio’da ayarlayın; böylece bu kılavuzdaki kod örneklerini denemeye hazır olursunuz.

### <a name="create-a-windows-console-application-project"></a>Windows konsol uygulaması projesi oluşturma
Visual Studio'da yeni bir Windows konsol uygulaması oluşturun. Aşağıdaki adımlar Visual Studio 2017’de bir konsol uygulaması oluşturmayı gösterir. Adımlar Visual Studio’nun diğer sürümlerinde de benzerdir.

1. **Dosya** > **Yeni** > **Proje**’yi seçin.
2. **Yüklü** > **Visual C#** > **Windows Klasik Masaüstü** öğesini seçin.
3. **Konsol Uygulaması (.NET Framework)** öğesini seçin.
4. **Ad** alanına uygulamanız için bir ad girin.
5. **Tamam**’ı seçin.

Bu örnekteki tüm kod örnekleri konsol uygulamanızın `Program.cs` dosyasındaki `Main()` yöntemine eklenebilir.

Azure bulut hizmeti veya web uygulaması ile masaüstü ve mobil uygulamaları dahil olmak üzere herhangi bir .NET uygulaması türünde Azure CosmosDB Tablo Kitaplığını kullanabilirsiniz. Bu kılavuzda, sadeleştirmek için konsol uygulaması kullanmaktayız.

### <a name="use-nuget-to-install-the-required-packages"></a>Gereken paketleri yüklemek için NuGet kullanma
Bu örneği tamamlamak için projenizde başvurmanız gereken üç önerilen paket vardır:

* [.NET için Azure Depolama Ortak Kitaplığı (önizleme)](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common). 
* [.NET için Microsoft Azure Cosmos DB Tablo Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table). Bu paket Azure Tablo depolama hesabınızdaki veya Azure Cosmos DB Tablo API’si hesabınızdaki veri kaynaklarına programlı erişim sağlar.
* [.NET için Microsoft Azure Configuration Manager Kitaplığı](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): Bu paket, uygulamanızın nerede çalıştığına bakmaksızın yapılandırma dosyasından bağlantı dizesini ayrıştırmak için bir sınıf sağlar.

Her iki paketi de almak için NuGet kullanabilirsiniz. Şu adımları uygulayın:

1. **Çözüm Gezgini**'nde projenize sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. "Microsoft.Azure.Storage.Common" için çevrimiçi arama yapın ve .NET için Azure Depolama Ortak Kitaplığı (Önizleme) ve bağımlılıklarını yüklemek için **Yükle**’yi seçin. Bu bir ön sürüm paketi olduğundan, **Ön sürümü dahil et** kutusunun işaretli olduğundan emin olun.
3. Microsoft Azure CosmosDB Tablo Kitaplığı’nı yüklemek için çevrimiçi "Microsoft.Azure.CosmosDB.Table" öğesini arayın ve **Yükle**’yi seçin.
4. Çevrimiçi olarak "WindowsAzure.ConfigurationManager" ifadesini arayın ve Microsoft Azure Yapılandırma Yöneticisi Kitaplığı’nı yüklemek için **Yükle**’yi seçin.

> [!NOTE]
> .NET için Depolama Ortak Kitaplığı’ndaki ODataLib bağımlılıkları, WCF Veri Hizmetleri’nden değil, NuGet üzerindeki ODataLib paketleriyle çözümlenir. ODataLib kitaplıkları NuGet aracılığıyla doğrudan indirilebilir veya kod projenizle başvurulabilir. Depolama İstemcisi Kitaplığı tarafından kullanılan belirli ODataLib paketleri [OData](http://nuget.org/packages/Microsoft.Data.OData/), [Edm](http://nuget.org/packages/Microsoft.Data.Edm/) ve [Spatial](http://nuget.org/packages/System.Spatial/) paketleridir. Bu kitaplıklar, Azure Tablo depolama sınıfları tarafından kullanılırken Depolama Ortak Kitaplığıyla programlama için gerekli bağımlılıklardır.
> 
> 

> [!TIP]
> Azure Tablo depolamayı zaten tanıyan geliştiriciler geçmişte [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) paketini kullanmış olabilir. Tüm yeni tablo uygulamalarının [Azure Depolama Ortak Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common) ve [Azure Cosmos DB Tablo Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table)’nı kullanması önerilir ancak WindowsAzure.Storage paketi hala desteklenmektedir. WindowsAzure.Storage kitaplığını kullanırsanız, kullanım deyimlerinize Microsoft.WindowsAzure.Storage.Table deyimini ekleyin.
>
>

### <a name="determine-your-target-environment"></a>Hedef ortamınızı saptama
Bu kılavuzdaki örnekleri çalıştırmak için üç ortam seçeneğiniz vardır:

* Kodunuzu buluttaki bir Azure Storage hesabına karşı çalıştırabilirsiniz. 
* Kodunuzu buluttaki bir Azure Cosmos DB hesabına karşı çalıştırabilirsiniz.
* Kodunuzu Azure Storage öykünücüsüne karşı çalıştırabilirsiniz. Depolama öykünücüsü, buluttaki Azure Storage hesabına öykünen bir yerel ortamdır. Öykünücü, uygulamanız geliştirildiği sırada kodunuzu test etmek ve hatalarını ayıklamak için bağımsız bir seçenektir. Öykünücü iyi bilinen hesabı ve anahtarı kullanır. Daha fazla bilgi için bkz. [Geliştirme ve test için Azure depolama öykünücüsünü kullanma](../storage/common/storage-use-emulator.md).

Buluttaki bir depolama hesabını hedefliyorsanız, depolama hesabınız için birincil erişim anahtarını Azure portalından kopyalayın. Daha fazla bilgi için bkz. [Depolama erişim anahtarlarını görüntüleme ve kopyalama](../storage/common/storage-create-storage-account.md#view-and-copy-storage-access-keys).

> [!NOTE]
> Azure Storage ile ilişkili maliyetlerin oluşmasını önlemek için depolama öykünücüsünü hedefleyebilirsiniz. Ancak, buluttaki bir Azure Storage hesabını hedeflemeyi seçerseniz, bu örneği gerçekleştirme maliyetleri göz ardı edilecektir.
> 
> 

Bir Azure Cosmos DB hesabını hedefliyorsanız, Tablo API’si hesabınız için birincil erişim anahtarını Azure portalından kopyalayın. Daha fazla bilgi için bkz. [Bağlantı dizenizi güncelleştirme](create-table-dotnet.md#update-your-connection-string).

### <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma
.NET için Azure Depolama Ortak Kitaplığı,depolama hizmetlerine erişilmesi amacıyla uç noktaları ve kimlik bilgilerini yapılandıracak depolama bağlantı dizesinin kullanılmasını destekler. Depolama bağlantı dizenizi korumanın en iyi yolu bir yapılandırma dosyasında tutmaktır. 

Bağlantı dizeleri hakkında daha fazla bilgi için bkz. [Azure Depolama’da bir bağlantı dizesi yapılandırma](../storage/common/storage-configure-connection-string.md).

> [!NOTE]
> Hesap anahtarınız depolama hesabınızın kök parolasına benzer. Depolama hesabı anahtarınızı korumak için her zaman özen gösterin. Diğer kullanıcılara dağıtmaktan, sabit kodlamaktan ve başkalarının erişebileceği düz metin dosyasına kaydetmekten kaçının. Anahtarınızın tehlikede olduğunu düşünüyorsanız, Azure portalını kullanarak hesap anahtarınızı yeniden oluşturun.
> 
> 

Bağlantı dizenizi yapılandırmak için, `app.config` dosyasını Visual Studio’daki Çözüm Gezgini'nden açın. `<appSettings>` öğesinin içeriğini aşağıda gösterildiği gibi ekleyin. `account-name` değerini hesabınızın adıyla ve `account-key` değerini hesabınızın erişim anahtarıyla değiştirin:

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.6.1" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
    </appSettings>
</configuration>
```

Örneğin, bir Azure Depolama hesabı kullanıyorsanız, yapılandırma ayarlarınız şuna benzer şekilde görünür:

```xml
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>" />
```

Bir Azure Cosmos DB hesabı kullanıyorsanız, yapılandırma ayarlarınız şuna benzer şekilde görünür:

```xml
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=tableapiacct;AccountKey=<account-key>;TableEndpoint=https://tableapiacct.table.cosmosdb.azure.com:443/;" />
```

Depolama öykünücüsünü hedeflemek için iyi bilinen hesap adıyla ve anahtarıyla eşleşen bir kısayolu kullanabilirsiniz. Bu durumda, bağlantı dizesi ayarı şöyle olur:

```xml
<add key="StorageConnectionString" value="UseDevelopmentStorage=true;" />
```

### <a name="add-using-directives"></a>Using yönergeleri ekleme
Aşağıdaki **using** yönergelerini `Program.cs` dosyasının üst tarafına ekleyin:

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.Azure.Storage; // Namespace for StorageAccounts
using Microsoft.Azure.CosmosDB.Table; // Namespace for Table storage types
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

* [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
* [.NET’te Azure Table Storage Kullanmaya Başlama](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/) bölümünde daha fazla Tablo depolama örneği bulabilirsiniz
* Kullanılabilir API’ler ile ilgili eksiksiz bilgiler için Tablo hizmeti başvuru belgelerini görüntüleyin:
* [.NET başvurusu için Depolama İstemci Kitaplığı](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
* [REST API başvurusu](http://msdn.microsoft.com/library/azure/dd179355)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) kullanarak Azure Storage ile birlikte çalışmak üzere yazdığınız kodları nasıl sadeleştireceğinizi öğrenin.
* Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.
* Yapılandırılmamış verileri depolamak için [.NET kullanarak Azure Blob Storage’ı kullanmaya başlayın](../storage/blobs/storage-dotnet-how-to-use-blobs.md).
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
