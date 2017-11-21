---
title: "Azure Cosmos DB: .NET API tabloda geliştirme | Microsoft Docs"
description: ".NET kullanarak Azure Cosmos veritabanı tablosu API'si ile geliştirmeyi öğrenin"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4b22cb49-8ea2-483d-bc95-1172cd009498
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/20/2017
ms.author: arramac
ms.custom: mvc
ms.openlocfilehash: 08e59fda2ea439b2272121cf8bfee76fe4f96fc3
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2017
---
# <a name="azure-cosmos-db-develop-with-the-table-api-in-net"></a>Azure Cosmos DB: .NET API tabloda geliştirin

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz.

Bu öğretici, aşağıdaki görevleri içerir: 

> [!div class="checklist"] 
> * Azure Cosmos DB hesabı oluşturma 
> * App.config dosyasında işlevselliğini etkinleştirmek 
> * Kullanarak bir tablo oluşturmak [tablo API](table-introduction.md)
> * Tabloya bir varlık ekleme 
> * Toplu işlem varlık yerleştirme 
> * Tek bir varlık alma 
> * Otomatik ikincil dizinler kullanarak sorgu varlıklar 
> * Bir varlığı değiştirme 
> * Bir varlığı silme 
> * Bir tablo silme
 
## <a name="tables-in-azure-cosmos-db"></a>Azure Cosmos DB tablolarında 

Azure Cosmos DB sağlar [tablo API](table-introduction.md) bir anahtar-değer deposu Şeması daha az bir tasarım gereken uygulamalar için. Her iki Azure Cosmos DB tablo API ve [Azure Table depolama](../storage/common/storage-introduction.md) artık aynı SDK'lar ve REST API'lerini desteklemektedir. Azure Cosmos DB’yi kullanarak yüksek aktarım hızı gereksinimleri olan tablolar oluşturabilirsiniz.

Bu öğretici, Azure Table storage'ı SDK bilgi sahibiyseniz ve kullanılabilir premium özellikleri Azure Cosmos DB ile kullanmak istediğiniz geliştiriciler içindir. Bağlı olduğu [.NET kullanarak Azure Table storage ile çalışmaya başlama](table-storage-how-to-use-dotnet.md) ve ikincil dizinler, sağlanan işleme ve birden çok giriş gibi ek özellikler yararlanmak nasıl gösterir. Bu öğretici Azure portalında bir Azure Cosmos DB hesabı oluşturun ve ardından derleme ve bir tablo API uygulamasını dağıtmak için nasıl kullanılacağını açıklar. Biz de .NET örnekleri oluşturma ve tablo, silme ve ekleme, güncelleştirme, silme ve tablo verileri Sorgulama yol. 

Şu anda Azure Table depolama kullanırsanız, Azure Cosmos DB tablo API ile aşağıdaki avantajlara sahip olursunuz:

- Anahtar teslim [genel dağıtım](distribute-data-globally.md) birden çok giriş ile ve [otomatik ve el ile yük devretme](regional-failover.md)
- Otomatik şema tüm özelliklerini ("ikincil dizinler") ve hızlı sorguları karşı dizin belirsiz desteği 
- Desteği [depolama ve işleme bağımsız ölçeklendirme](partition-data.md), herhangi bir sayıda bölgeler arasında
- Desteği [tablo başına ayrılmış işleme](request-units.md) , ölçeklendirilmiş istekleri saniye başına milyonlarca yüzlerce gelen
- Desteği [beş ince ayarlanabilir tutarlılık düzeyleri](consistency-levels.md) kullanılabilirlik, gecikme ve uygulamanıza dayalı tutarlılık kapalı ticari gerekiyor
- tek bölge ve yüksek kullanılabilirlik için daha fazla bölgeler ekleme yeteneği içinde % 99,99 kullanılabilirlik ve [endüstri lideri kapsamlı SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) genel kullanılabilirliğine
- Var olan Azure depolama .NET SDK'sı ile çalışma ve uygulamanız için hiçbir kod değişiklikleri

Bu öğretici Azure Cosmos DB tablo API'si .NET SDK kullanarak kapsar. İndirebilirsiniz [Azure depolama Preview SDK](https://aka.ms/tableapinuget) NuGet gelen.

Karmaşık Azure Table depolama görevleri hakkında daha fazla bilgi için bkz:

* [Azure Cosmos DB tablo API giriş](table-introduction.md)
* Kullanılabilir API'ler ile ilgili tam Ayrıntılar için tablo hizmeti başvuru belgelerini [Azure Cosmos DB tablo API .NET SDK'sı](https://docs.microsoft.com/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)

### <a name="about-this-tutorial"></a>Bu öğretici hakkında
Bu öğretici Azure Table storage'ı SDK bilgi sahibiyseniz ve kullanılabilir premium özellikleri kullanmak istediğiniz geliştiriciler için Azure Cosmos DB kullanıyor. Bağlı olduğu [.NET kullanarak Azure Table storage ile çalışmaya başlama](table-storage-how-to-use-dotnet.md) ve ikincil dizinler, sağlanan işleme ve birden çok giriş gibi ek özellikler yararlanmak nasıl gösterir. Size bir Azure Cosmos DB hesabı oluşturun ve ardından derleme ve tablo uygulamayı dağıtmak için Azure portalını kullanmayı kapsar. Biz de .NET örnekleri oluşturma ve tablo, silme ve ekleme, güncelleştirme, silme ve tablo verileri Sorgulama yol. 

Visual Studio yüklü 2017 yoksa kullanın karşıdan yükleyip **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Azure portalında bir Azure Cosmos DB hesabı oluşturarak başlayalım.  

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Tablo uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Git bash gibi bir git terminal penceresi açın ve kullanmak `cd` örnek uygulamayı yüklemek için bir klasör olarak değiştirmek için komutu. 

    ```bash
    cd "C:\git-samples"
    ```

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulaması bir kopyasını oluşturur. 

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-dotnet-getting-started.git
    ```

3. Ardından çözüm dosyasını Visual Studio'da açın. 

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bu, barındırılan veritabanıyla iletişim kurmak uygulamanızı sağlar. 

1. İçinde [Azure portal](http://portal.azure.com/), tıklatın **bağlantı dizesi**. 

    BİRİNCİL bağlantı DİZESİNİ kopyalayın için ekranın sağ tarafta kopyalama düğmelerini kullanın.

    ![Görüntüleyin ve bağlantı dizesi Bölmesi'nde bağlantı DİZESİNİ kopyalayın](./media/create-table-dotnet/connection-string.png)

2. Visual Studio'da app.config dosyasını açın. 

3. Bu öğretici depolama öykünücüsünü kullanmaz gibi satır 8 ve yorum 7 satırındaki StorageConnectionString çıkışı StorageConnectionString açıklamadan çıkarın. Satır 7 ve 8 gibi görünmelidir:

    ```
    <!--key="StorageConnectionString" value="UseDevelopmentStorage=true;" />-->
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]" />
    ```

4. StorageConnectionString değerini satırında 8 Portalı'ndan birincil bağlantı DİZESİNİ yapıştırın. Tırnak işaretleri içine dizesini yapıştırın.
   
    > [!IMPORTANT]
    > Uç noktanız Önizleme hesabına sahip olduğunuz anlamına gelir, documents.azure.com, kullanıyorsa ve oluşturmak gereken bir [yeni tablo API hesabı](#create-a-database-account) genel olarak kullanılabilir tablo API SDK'sı ile çalışmak için. 
    >

    Satır 8 benzer şekilde görünmelidir:

    ```
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=txZACN9f...==;TableEndpoint=https://<account name>.table.cosmosdb.azure.com;" />
    ```

5. App.config dosyasını kaydedin.

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

## <a name="azure-cosmos-db-capabilities"></a>Azure Cosmos DB özellikleri
Azure Cosmos DB Azure Table storage ' API kullanılamaz özelliklerini destekler. 

Bazı işlevleri, bir bağlantı İlkesi ve tutarlılık düzeyi belirtmenizi sağlayan yeni aşırı CreateCloudTableClient için erişilir.

| Tablo bağlantı ayarları | Açıklama |
| --- | --- |
| Bağlantı modu  | Azure Cosmos DB iki bağlantı modunu destekler. İçinde `Gateway` modunda her zaman yapılan istekler Azure Cosmos DB ağ geçidi, karşılık gelen veri bölümleri iletir. İçinde `Direct` bağlantı modunu istemci tabloları eşleme bölümlere getirir ve istekleri doğrudan veri bölümlerini karşı yapılır. Öneririz `Direct`, varsayılan değer.  |
| Bağlantı Protokolü | Azure Cosmos DB destekleyen iki bağlantı protokol - `Https` ve `Tcp`. `Tcp`varsayılan ayardır ve daha basit olduğu için önerilir. |
| Tercih edilen konumları | Tercih edilen (çok girişli) konumları okuma için virgülle ayrılmış listesi. Her Azure Cosmos DB hesabı 1 ile ilişkili olabilir-30 + bölgeleri. Her bir istemci örnek bir alt kümesini Bu bölgeler düşük gecikme süresi okuma tercih edilen sırayı belirtebilirsiniz. Bölgeleri kullanma şeklinde adlandırılmalıdır kendi [görünen adları](https://msdn.microsoft.com/library/azure/gg441293.aspx), örneğin, `West US`. Ayrıca bkz. [birden çok giriş API'leri](tutorial-global-distribution-table.md). |
| Tutarlılık Düzeyi | Devre dışı gecikme, tutarlılık ve kullanılabilirlik arasında beş iyi tanımlanmış tutarlılık düzeyleri arasında seçerek ticari: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, ve `Eventual`. Varsayılan değer `Session`. Tutarlılık düzeyi seçimi önemli performans farkı bölgeli kurulumlarında yapar. Bkz: [tutarlılık düzeylerini](consistency-levels.md) Ayrıntılar için. |

Diğer işlevleri aşağıdaki aracılığıyla etkinleştirilebilir `appSettings` yapılandırma değerlerini.

| Anahtar | Açıklama |
| --- | --- |
| TableThroughput | Saniye başına istek birimleri (RU) cinsinden tablo için ayrılmış işleme. Tek tablolar 100s-RU/s milyonlarca destekleyebilir. Bkz: [istek birimleri](request-units.md). Varsayılan değer`400` |
| TableIndexingPolicy | Dizin oluşturma ilkesi belirtimine uygun JSON dizesi. Bkz: [dizin oluşturma ilkesi](indexing-policies.md) belirli sütunlardaki dahil etme/hariç tutma için dizin oluşturma ilkesi nasıl değiştiğini görmek için. |
| TableQueryMaxItemCount | Tek gidiş dönüş tablosu sorgu başına döndürülen öğe sayısını yapılandırın. Varsayılan değer `-1`, Azure Cosmos değer çalışma zamanında dinamik olarak belirleyen DB olanak sağlar. |
| TableQueryEnableScan | Sorgu için herhangi bir filtre dizini kullanamıyorsanız, ardından çalıştırın yine de bir tarama. Varsayılan değer `false`.|
| TableQueryMaxDegreeOfParallelism | Çapraz bölüm sorgusu yürütme için paralellik derecesi. `0`hiçbir önceden getirme ile seri olduğu `1` olan seri önceden getirilirken ve daha yüksek değerlerle artırmak paralellik oranı. Varsayılan değer `-1`, Azure Cosmos değer çalışma zamanında dinamik olarak belirleyen DB olanak sağlar. |

Varsayılan değeri değiştirmek için açın `app.config` Visual Studio'daki Çözüm Gezgini'nden dosya. `<appSettings>` öğesinin içeriğini aşağıda gösterildiği gibi ekleyin. Değiştir `account-name` depolama hesabınızın adıyla ve `account-key` hesap erişim anahtarı ile. 

```xml
<configuration>
    ...
    <appSettings>
      <!-- Client options -->
      <add key="CosmosDBStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://account-name.table.cosmosdb.azure.com" />
      <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key; TableEndpoint=https://account-name.documents.azure.com" />

      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
      <add key="TableIndexingPolicy" value="{""indexingMode"": ""Consistent""}"/>

      <!-- Table query options -->
      <add key="TableQueryMaxItemCount" value="-1"/>
      <add key="TableQueryEnableScan" value="false"/>
      <add key="TableQueryMaxDegreeOfParallelism" value="-1"/>
      <add key="TableQueryContinuationTokenLimitInKb" value="16"/>
            
    </appSettings>
</configuration>
```

Uygulamada gerçekleşen işlemleri hızlıca gözden geçirelim. Açık `Program.cs` dosyanız varsa ve bulacaksınız Bu kod satırları tablo kaynakları oluşturun. 

## <a name="create-the-table-client"></a>Tablo istemcisi oluşturma
Başlatır bir `CloudTableClient` tablo hesabınıza bağlanmak için.

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
Bu istemci kullanarak başlatılır `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, ve `TablePreferredLocations` uygulama ayarlarında belirtilen yapılandırma değerleri.

## <a name="create-a-table"></a>Bir tablo oluşturma
Ardından, kullanarak bir tablo oluşturun `CloudTable`. Azure Cosmos DB tablolarında depolama ve işleme açısından bağımsız olarak ölçeklendirebilirsiniz ve bölümlendirme hizmeti tarafından otomatik olarak gerçekleştirilir. Azure Cosmos DB sabit boyutlu ve sınırsız tabloları destekler. Bkz: [Azure Cosmos DB'de bölümleme](partition-data.md) Ayrıntılar için. 

```csharp
CloudTable table = tableClient.GetTableReference("people");

table.CreateIfNotExists();
```

Tabloları nasıl oluşturulduğunu, önemli bir fark yoktur. Azure Cosmos DB işlemleri için Azure storage'nın tüketim tabanlı modeli farklı verimlilik ayırır. Üretilen iş ayrılmış /, istek hızı düzeyinde veya altında sağlanan işleme ise, hiçbir zaman kısıtlanan için ayrılmış.

Varsayılan işleme ayarını yapılandırarak yapılandırabileceğiniz `TableThroughput` RU (istek birimleri) / saniye cinsinden. 

Bir 1 KB varlığı okuma 1 olarak normalleştirilmiş RU ve diğer işlemlerin, CPU, bellek ve IOPS tüketime dayanarak sabit bir RU değere normalleştirilmiş. Daha fazla bilgi edinmek [istek birimleri Azure Cosmos veritabanı](request-units.md) ve özel olarak [anahtar değer depoları](key-value-store-cost.md).

Ardından, biz basit okuyun, yol ve Azure Table depolama SDK'sını kullanarak (CRUD) işlemleridir yazma. Bu öğretici, tahmin edilebilir düşük tek basamaklı milisaniyelik gecikme ve Azure Cosmos DB tarafından sağlanan hızlı sorguları gösterir.

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme
Azure Table depolama varlıklarda genişletmek `TableEntity` sınıfı ve olmalıdır `PartitionKey` ve `RowKey` özellikleri. Bir müşteri varlığı için örnek tanımı aşağıda verilmiştir.

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

Aşağıdaki kod parçacığında, Azure depolama SDK'sı sahip bir varlık eklemek gösterilmiştir. Azure Cosmos DB herhangi ölçekli, düşük gecikme dünya genelindeki garanti için tasarlanmıştır.

Yazma tamamlamak < 15 ms p99 ve Azure Cosmos DB hesabı ile aynı bölgede çalışan uygulamalar için p50 adresindeki ~ 6 ms. Ve bu süre yalnızca bunlar zaman uyumlu olarak, bir işlemi tamamlandıktan sonra çoğaltılır ve tüm içeriğini dizine sonra yazma istemciye onaylanan, olgu için hesaplar.


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
Azure tablo depolama destekler, güncelleştirmelerinin birleştirmek olanak tanır, bir toplu işlem API, siler ve aynı toplu işlemde ekler.

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
Tam Azure Cosmos DB'de (alır) alır < p99 ve ~ 1 10 ms p50 aynı Azure bölgesinde adresindeki ms. Sayıda bölgeler için düşük gecikmeli okuma hesabınıza eklemek ve kendi yerel bölgesinden ("çok konaklı") ayarlayarak okumak için dağıtırken `TablePreferredLocations`. 

Aşağıdaki kod parçacığını kullanarak tek bir varlık alabilirsiniz:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> Çok girişli API'leri hakkında bilgi edinin [birden çok bölgeye ile geliştirme](tutorial-global-distribution-table.md)
>

## <a name="query-entities-using-automatic-secondary-indexes"></a>Otomatik ikincil dizinler kullanarak sorgu varlıklar
Tablolar sorgulanan kullanarak `TableQuery` sınıfı. Azure Cosmos DB tablonuz içindeki tüm sütunlar otomatik olarak dizinler bir yazma iyileştirilmiş veritabanı altyapısı vardır. Azure Cosmos DB'de dizin şemasına bağımsızdır. Bu nedenle, şemanızı satırlar arasında farklı olsa bile veya şema zamanla dönüşmesi varsa, otomatik olarak dizine alınır. Azure Cosmos DB otomatik ikincil dizinler desteklediğinden, herhangi bir özellik sorguları dizini kullanabilir ve verimli bir şekilde sunulması.

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

Azure Cosmos DB tablo API için Azure Table storage aynı sorgu işlevleri destekler. Azure Cosmos DB, sıralama, toplamalar, Jeo-uzamsal sorgu, hiyerarşi ve çok çeşitli yerleşik işlevler de destekler. Ek işlevsellik gelecekteki hizmeti güncelleştirmesine tablo API'sindeki sağlanır. Bkz: [Azure Cosmos DB sorgusu](documentdb-sql-query.md) bu özelliklere genel bakış. 

## <a name="replace-an-entity"></a>Bir varlığı değiştirme
Bir varlığı güncelleştirmek için Tablo hizmetinden alın, varlık nesnesini değiştirin ve değişiklikleri Tablo hizmetine geri kaydedin. Aşağıdaki kod mevcut bir müşterinin telefon numarasını değiştirir. 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
Benzer şekilde, gerçekleştirebileceğiniz `InsertOrMerge` veya `Merge` işlemleri.  

## <a name="delete-an-entity"></a>Bir varlığı silme
Bir varlığı güncelleştirmek için gösterilen aynı yöntemi kullanarak, bir varlığı aldıktan sonra kolayca silebilirsiniz. Aşağıdaki kod bir müşteri girişini alır ve siler.

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a>Bir tablo silme
Son olarak aşağıdaki kod örneği bir depolama hesabından bir tablo siler. Silin ve hemen Azure Cosmos DB içeren bir tablo oluşturun.

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, size Azure Cosmos DB tablo API ile kullanmaya başlamak nasıl ele ve aşağıdakileri yaptığınızdan: 

> [!div class="checklist"] 
> * Bir Azure Cosmos DB hesabı oluşturuldu 
> * App.config dosyasında etkin işlevi 
> * Bir tablo oluşturuldu 
> * Tabloya bir varlık eklenen 
> * Toplu işlem varlık eklenen 
> * Tek bir varlık alınan 
> * Otomatik ikincil dizinler kullanılarak sorgulanan varlıklar 
> * Bir varlık değiştirildi 
> * Bir varlık silindi 
> * Bir tablo silindi  

Şimdi, sonraki öğretici devam etmek ve tablo verileri sorgulama hakkında daha fazla bilgi edinin. 

> [!div class="nextstepaction"]
> [Tablo API sorgusu](tutorial-query-table.md)
