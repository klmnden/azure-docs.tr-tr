---
title: 'Azure Cosmos DB: .NET’te Tablo API’si ile geliştirme'
description: .NET kullanarak Azure Cosmos DB Tablo API'si ile geliştirmeyi öğrenin
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 12/18/2017
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 02c4ead0f41463a70cc7123427193f835d9cca94
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52877744"
---
# <a name="azure-cosmos-db-develop-with-the-table-api-in-net"></a>Azure Cosmos DB: .NET’te Tablo API’si ile geliştirme

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz.

Bu öğretici aşağıdaki görevleri kapsar: 

> [!div class="checklist"] 
> * Azure Cosmos DB hesabı oluşturma 
> * App.config dosyasında işlevi etkinleştirme 
> * [Tablo API’si](table-introduction.md) kullanarak tablo oluşturma
> * Tabloya bir varlık ekleme 
> * Toplu işlem varlık yerleştirme 
> * Tek bir varlık alma 
> * Otomatik ikincil dizinleri kullanarak varlıkları sorgulama 
> * Bir varlığı değiştirme 
> * Bir varlığı silme 
> * Bir tablo silme
 
## <a name="tables-in-azure-cosmos-db"></a>Azure Cosmos DB’de tablolar 

Azure Cosmos DB, şemasız tasarımla bir anahtar-değer deposuna gereksinim duyan uygulamalar için [Tablo API](table-introduction.md)’sini sağlar. Hem Azure Cosmos DB Tablo API’si hem de [Azure Tablo depolama](../storage/common/storage-introduction.md) artık aynı SDK’ları ve REST API’lerini desteklemektedir. Azure Cosmos DB’yi kullanarak yüksek aktarım hızı gereksinimleri olan tablolar oluşturabilirsiniz.

Bu öğretici, Azure Tablo depolama SDK’sını bilen ve Azure Cosmos DB ile sunulan premium özellikleri kullanmak isteyen geliştiricilere yöneliktir. [.NET kullanarak Azure Tablo depolama ile çalışmaya başlama](table-storage-how-to-use-dotnet.md) makalesini temel alır ve ikincil dizinler, hazırlanmış aktarım hızı ve birden çok giriş gibi ek özelliklerden nasıl yararlanılacağını gösterir. Bu öğreticide Azure portalını kullanarak bir Azure Cosmos DB hesabı oluşturma ve sonra bir Tablo API uygulaması derleyip dağıtma işlemi açıklanmaktadır. Ayrıca tablo oluşturup silme ve tablo verileri ekleme, güncelleştirme, silme ve sorgulamaya yönelik .NET örneklerini göstereceğiz. 

Şu anda Azure Tablo depolama hizmetini kullanıyorsanız, Azure Cosmos DB Tablo API’si ile aşağıdaki avantajları elde edersiniz:

- Birden çok giriş ve [otomatik ve el ile yük devretme](high-availability.md) içeren anahtar teslim [genel dağıtım](distribute-data-globally.md)
- Tüm özelliklere yönelik otomatik şemadan bağımsız dizinleme ("ikincil dizinler") desteği ve hızlı sorgular 
- Dilediğiniz sayıda bölgede [depolama ve aktarım hızını bağımsız ölçeklendirme](partition-data.md) desteği
- Saniyede yüzlerce ile milyonlarca istekten ölçeklenebilen [tablo başına ayrılmış aktarım hızı](request-units.md) desteği
- Uygulama gereksinimlerinize bağlı olarak kullanılabilirlik, gecikme süresi ve tutarlılık arasında denge sağlamak için [beş ayarlanabilir tutarlılık düzeyi](consistency-levels.md) desteği
- Tek bölge içinde %99,99 kullanılabilirlik ve daha yüksek kullanılabilirlik için daha fazla bölge ekleme olanağı ve genel kullanılabilirlikte [sektör lideri kapsamlı SLA’lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- Mevcut Azure depolama .NET SDK'sı ile çalışma ve uygulamanız için kod değişiklikleri yapılmaması

Bu öğretici, .NET SDK’sı kullanan Azure Cosmos DB Tablo API'sini ele almaktadır. [Azure Cosmos DB Tablo API .NET SDK'sını](https://aka.ms/tableapinuget) NuGet’ten indirebilirsiniz.

Karmaşık Azure Tablo depolama görevleri hakkında bilgi almak için bkz.

* [Azure Cosmos DB Tablo API’sine Giriş](table-introduction.md)
* Kullanılabilir API’ler ile ilgili eksiksiz bilgiler için Tablo hizmeti başvuru belgeleri [Azure Cosmos DB Tablo API .NET SDK’sı](https://docs.microsoft.com/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)

### <a name="about-this-tutorial"></a>Bu öğretici hakkında
Bu öğretici, Azure Tablo depolama SDK’sını bilen ve Azure Cosmos DB ile sunulan premium özellikleri kullanmak isteyen geliştiricilere yöneliktir. [.NET kullanarak Azure Tablo depolama ile çalışmaya başlama](table-storage-how-to-use-dotnet.md) makalesini temel alır ve ikincil dizinler, hazırlanmış aktarım hızı ve birden çok giriş gibi ek özelliklerden nasıl yararlanılacağını gösterir. Azure portalını kullanarak bir Azure Cosmos DB hesabı oluşturma ve sonra bir Tablo uygulaması derleyip dağıtma işlemini ele alıyoruz. Ayrıca tablo oluşturup silme ve tablo verileri ekleme, güncelleştirme, silme ve sorgulamaya yönelik .NET örneklerini göstereceğiz. 

Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

İlk olarak Azure portalında bir Azure Cosmos DB hesabı oluşturalım.  
 
> [!IMPORTANT]  
> Genel olarak kullanılabilir Tablo API’si SDK’ları ile çalışmak için yeni bir Tablo API’si hesabı oluşturmanız gerekir. Önizleme sırasında oluşturulan Tablo API’si hesapları genel olarak kullanılabilir SDK’lar tarafından desteklenmez. 
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Tablo uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle program aracılığıyla çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Git Bash gibi bir Git terminal penceresi açın ve örnek uygulamayı yüklemek üzere bir klasör olarak değiştirmek için `cd` komutunu kullanın. 

    ```bash
    cd "C:\git-samples"
    ```

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur. 

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-dotnet-getting-started.git
    ```

3. Ardından çözüm dosyasını Visual Studio'da açın. 

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bu, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır. 

1. [Azure portalında](http://portal.azure.com/) **Bağlantı Dizesi**’ne tıklayın. 

    Ekranın sağ tarafındaki kopyala düğmesini kullanarak PRIMARY CONNECTION STRING’i kopyalayın.

    ![Bağlantı Dizesi bölmesindeki CONNECTION STRING’i kopyalama ve görüntüleme](./media/create-table-dotnet/connection-string.png)

2. Visual Studio'da app.config dosyasını açın. 

3. Bu öğretici Depolama Öykünücüsü kullanmadığından 8. satırdaki StorageConnectionString öğesini açıklama durumundan çıkarın ve 7. satırdaki StorageConnectionString öğesini açıklama yapın. 7. ve 8. satır artık şöyle görünmelidir:

    ```
    <!--key="StorageConnectionString" value="UseDevelopmentStorage=true;" />-->
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]" />
    ```

4. Portaldan PRIMARY CONNECTION STRING öğesini 8. satırdaki StorageConnectionString değerine yapıştırın. Dizeyi tırnak işareti içinde yapıştırın.
   
    > [!IMPORTANT]
    > Uç Noktanız documents.azure.com kullanıyorsa, bir önizleme hesabınız var demektir ve genel olarak kullanılabilir Tablo API’si SDK’ları ile çalışmak için [yeni bir Tablo API’si hesabı](#create-a-database-account) oluşturmanız gerekir. 
    >

    8. Satır şuna benzer şekilde görünmelidir:

    ```
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=txZACN9f...==;TableEndpoint=https://<account name>.table.cosmosdb.azure.com;" />
    ```

5. App.config dosyasını kaydedin.

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

## <a name="azure-cosmos-db-capabilities"></a>Azure Cosmos DB özellikleri
Azure Cosmos DB, Azure Tablo depolama API’sinde kullanılamayan birkaç özelliği destekler. 

Bazı işlevlere, bağlantı ilkesi ve tutarlılık düzeyini belirtmeye olanak tanıyan CreateCloudTableClient’a yeni aşırı yüklemeler yapılarak erişilir.

| Tablo Bağlantı Ayarları | Açıklama |
| --- | --- |
| Bağlantı Modu  | Azure Cosmos DB iki bağlantı modunu destekler. `Gateway` modunda istekler her zaman Azure Cosmos DB ağ geçidine yapılır ve oradan ilgili veri bölümlerine iletilir. `Direct` bağlantı modunda istemci, tablo eşlemelerini bölümlere getirir ve istekler doğrudan veri bölümlerine karşı yapılır. Varsayılan `Direct` değeri önerilir.  |
| Bağlantı Protokolü | Azure Cosmos DB iki bağlantı protokolünü destekler: `Https` ve `Tcp`. `Tcp` varsayılan ayardır ve daha basit olduğu için önerilir. |
| Tercih Edilen Konumlar | Okuma için tercih edilen (çok girişli) konumların virgülle ayrılmış listesi. Her Azure Cosmos DB hesabı 1-30+ bölge ile ilişkilendirilebilir. Her istemci düşük gecikme okumaları için tercih edilen sırayla bu bölgelerin bir alt kümesini belirtebilir. Bölgeler [görünen adları](https://msdn.microsoft.com/library/azure/gg441293.aspx) kullanılarak adlandırılmalıdır, örneğin `West US`. Ayrıca bkz. [Birden çok giriş API'leri](tutorial-global-distribution-table.md). |
| Tutarlılık Düzeyi | İyi tanımlanmış beş tutarlılık düzeyi (`Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix` ve `Eventual`) arasından seçim yaparak gecikme süresi, tutarlılık ve kullanılabilirliği dengeleyebilirsiniz. `Session` varsayılan değerdir. Tutarlılık düzeyi seçimi, çok bölgeli kurulumlarda önemli bir performans farkı oluşturur. Ayrıntılar için bkz. [Tutarlılık düzeyleri](consistency-levels.md). |

Aşağıdaki `appSettings` yapılandırma değerleri ile diğer işlevler etkinleştirilebilir.

| Anahtar | Açıklama |
| --- | --- |
| TableQueryMaxItemCount | Tek gidiş dönüşte tablo sorgusu başına döndürülen en fazla öğe sayısını yapılandırın. `-1` varsayılan değeri, Azure Cosmos DB’nin çalışma zamanındaki değeri dinamik olarak belirlemesine olanak tanır. |
| TableQueryEnableScan | Sorgu herhangi bir filtre için dizini kullanamıyorsa, bir tarama ile yine de çalıştırın. `false` varsayılan değerdir.|
| TableQueryMaxDegreeOfParallelism | Çapraz bölüm sorgusunun yürütülmesi için paralellik derecesi. `0` önceden getirme olmadan seridir, `1` önceden getirme ile seridir ve değerler yükseldikçe paralellik oranını artırır. `-1` varsayılan değeri, Azure Cosmos DB’nin çalışma zamanındaki değeri dinamik olarak belirlemesine olanak tanır. |

Varsayılan değeri değiştirmek için Visual Studio'da Çözüm Gezgini'nden `app.config` dosyasını açın. `<appSettings>` öğesinin içeriğini aşağıda gösterildiği gibi ekleyin. `account-name` değerini depolama hesabınızın adıyla ve `account-key` değerini hesabınızın erişim anahtarıyla değiştirin. 

```xml
<configuration>
    ...
    <appSettings>
      <!-- Client options -->
      <add key="CosmosDBStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://account-name.table.cosmosdb.azure.com" />
      <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key; TableEndpoint=https://account-name.documents.azure.com" />

      <!-- Table query options -->
      <add key="TableQueryMaxItemCount" value="-1"/>
      <add key="TableQueryEnableScan" value="false"/>
      <add key="TableQueryMaxDegreeOfParallelism" value="-1"/>
      <add key="TableQueryContinuationTokenLimitInKb" value="16"/>
            
    </appSettings>
</configuration>
```

Uygulamada gerçekleşen işlemleri hızlıca gözden geçirelim. `Program.cs` dosyasını açtığınızda Tablo kaynaklarını bu kod satırlarının oluşturduğunu göreceksiniz. 

## <a name="create-the-table-client"></a>Tablo istemcisi oluşturma
Tablo hesabına bağlanmak için bir `CloudTableClient` başlatın.

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
Bu istemci, uygulama ayarlarında belirtilmişse `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel` ve `TablePreferredLocations` yapılandırma değerleri kullanılarak başlatılır.

## <a name="create-a-table"></a>Bir tablo oluşturma
Ardından, `CloudTable` kullanarak bir tablo oluşturun. Azure Cosmos DB’deki tablolar depolama ve aktarım hızı bakımından bağımsız olarak ölçeklendirilebilir ve bölümleme işlemi hizmet tarafından otomatik olarak gerçekleştirilir. Azure Cosmos DB hem sabit boyutlu hem de sınırsız tabloları destekler. Ayrıntılar için bkz. [Azure Cosmos DB'de Bölümleme](partition-data.md). 

```csharp
CloudTable table = tableClient.GetTableReference("people");
400
table.CreateIfNotExists(throughput: 800);
```

Tabloların oluşturulma şeklinde önemli bir fark yoktur. Azure depolamanın işlemlere yönelik tüketim temelli modelinin aksine Azure Cosmos DB, aktarım hızını ayırır. Aktarım hızını ayrılmıştır, bu nedenle istek hızınız sağladığınız aktarım hızında veya daha düşük olursa hiçbir zaman kısıtlanmazsınız.

Varsayılan aktarım hızını bir CreateIfNotExists parametresi olarak ekleyerek yapılandırabilirsiniz.

1 KB varlığın okuması 1 RU olarak normalleştirilir ve diğer işlemler CPU, bellek ve IOPS tüketimine göre sabit bir RU değerine normalleştirilir. Özellikle [Anahtar değer depoları](key-value-store-cost.md) için [Azure Cosmos DB’de istek birimleri](request-units.md) hakkında daha fazla bilgi edinin.

Ardından, Azure Tablo depolama SDK’sını kullanarak basit okuma ve yazma (CRUD) işlemlerine göz atacağız. Bu öğreticide Azure Cosmos DB tarafından sağlanan tahmin edilebilen düşük tek basamaklı milisaniye gecikme süreleri ve hızlı sorgular gösterilmektedir.

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme
Azure Tablo depolamadaki varlıklar `TableEntity` sınıfından genişletilir ve `PartitionKey` ile `RowKey` özelliklerine sahip olmalıdır. Bir müşteri varlığı için örnek bir tanım aşağıda verilmiştir.

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

Aşağıdaki kod parçacığında Azure depolama SDK'sı ile bir varlığın nasıl ekleneceği gösterilmiştir. Azure Cosmos DB dünyanın herhangi bir yerinde herhangi bir ölçekte garantili düşük gecikme süresi için tasarlanmıştır.

Azure Cosmos DB hesabıyla aynı bölgede çalışan uygulamalar için yazma işlemleri p99’da <15 ms ve p50’de yaklaşık 6 ms sürede tamamlanır. Bu süre, yazma işlemlerinin istemciye yalnızca zaman uyumlu bir şekilde çoğaltıldıktan sonra geri kabul edildiğini, dayanıklı bir şekilde işlendiğini ve tüm içeriğin dizinlendiğini hesaba katar.


```csharp
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
Azure Tablo depolama, aynı toplu işlemde güncelleştirme, silme ve ekleme işlemlerini birleştirmenize olanak tanıyan bir toplu işlem API’sini destekler.

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
table.ExecuteBatch(batchOperation);
```
## <a name="retrieve-a-single-entity"></a>Tek bir varlık alma
Azure Cosmos DB’de alma işlemleri (GET), aynı Azure bölgesinde p99’da <10 ms ve p50’de yaklaşık 1 ms sürede tamamlanır. Hesabınıza düşük gecikme okumaları için dilediğiniz kadar bölge ekleyebilir ve `TablePreferredLocations` ayarını yaparak uygulamaları yerel bölgelerinden ("birden çok girişli") okuyacak şekilde dağıtabilirsiniz. 

Aşağıdaki kod parçacığını kullanarak tek bir varlığı alabilirsiniz:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> [Birden fazla bölge ile geliştirme](tutorial-global-distribution-table.md) bölümünde birden çok giriş API’leri hakkında bilgi edinin
>

## <a name="query-entities-using-automatic-secondary-indexes"></a>Otomatik ikincil dizinleri kullanarak varlıkları sorgulama
Tablolar `TableQuery` sınıfı kullanılarak sorgulanabilir. Azure Cosmos DB, tablonuzdaki tüm sütunları otomatik olarak dizinleyen, yazma için iyileştirilmiş veritabanı altyapısına sahiptir. Azure Cosmos DB'de dizinleme şemadan bağımsızdır. Bu nedenle, şemanız satırlar arasında farklı olsa bile veya şema zaman içinde gelişse bile otomatik olarak dizinlenir. Azure Cosmos DB otomatik ikincil dizinleri desteklediğinden, herhangi bir özelliğe karşı sorgular dizini kullanabilir ve verimli bir şekilde sunulabilir.

```csharp
CloudTable table = tableClient.GetTableReference("people");

// Filter against a property that's not partition key or row key
TableQuery<CustomerEntity> emailQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.GenerateFilterCondition("Email", QueryComparisons.Equal, "Ben@contoso.com"));

foreach (CustomerEntity entity in table.ExecuteQuery(emailQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

Azure Cosmos DB, Tablo API’si için Azure Tablo depolama ile aynı sorgu işlevlerini destekler. Azure Cosmos DB ayrıca sıralama, toplamalar, jeo-uzamsal sorgu, hiyerarşi ve çok çeşitli yerleşik işlevleri de destekler. Bu özelliklere genel bakış için bkz. [Azure Cosmos DB sorgusu](how-to-sql-query.md). 

## <a name="replace-an-entity"></a>Bir varlığı değiştirme
Bir varlığı güncelleştirmek için Tablo hizmetinden alın, varlık nesnesini değiştirin ve değişiklikleri Tablo hizmetine geri kaydedin. Aşağıdaki kod mevcut bir müşterinin telefon numarasını değiştirir. 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
Benzer şekilde, `InsertOrMerge` veya `Merge` işlemlerini gerçekleştirebilirsiniz.  

## <a name="delete-an-entity"></a>Bir varlığı silme
Bir varlığı güncelleştirmek için gösterilen aynı yöntemi kullanarak, bir varlığı aldıktan sonra kolayca silebilirsiniz. Aşağıdaki kod bir müşteri girişini alır ve siler.

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a>Bir tablo silme
Son olarak aşağıdaki kod örneği bir depolama hesabından bir tablo siler. Azure Cosmos DB ile bir tabloyu silip hemen yeniden oluşturabilirsiniz.

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure Cosmos DB’yi Tablo API’si ile kullanmaya başlama konusunu ele aldık ve aşağıdaki işlemleri yaptınız: 

> [!div class="checklist"] 
> * Azure Cosmos DB hesabı oluşturma 
> * App.config dosyasında işlevi etkinleştirme 
> * Tablo oluşturma 
> * Tabloya bir varlık ekleme 
> * Varlık grubu ekleme 
> * Tek bir varlık alma 
> * Otomatik ikincil dizinleri kullanarak varlıkları sorgulama 
> * Varlığı değiştirme 
> * Varlığı silme 
> * Tabloyu silme  

Artık sonraki öğreticiye geçerek tablo verilerini sorgulama hakkında daha fazla bilgi edinebilirsiniz. 

> [!div class="nextstepaction"]
> [Tablo API’si ile sorgulama](tutorial-query-table.md)
