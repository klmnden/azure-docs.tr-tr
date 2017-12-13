---
title: "Azure Cosmos DB: .NET içinde SQL API geliştirme | Microsoft Docs"
description: ".NET kullanarak Azure Cosmos veritabanı SQL API'si ile geliştirmeyi öğrenin"
services: cosmos-db
documentationcenter: 
author: rafats
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: rafats
ms.custom: mvc
ms.openlocfilehash: 9209d815cadcb3abfacdc765c503851ba63863bc
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="azure-cosmosdb-develop-with-the-sql-api-in-net"></a>Azure CosmosDB: .NET içinde SQL API geliştirin

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu öğreticide Azure portalını kullanarak bir Azure Cosmos DB hesabı oluşturmak ve bir belge veritabanı ve koleksiyonu oluşturmak nasıl gösteren bir [bölüm anahtarı](documentdb-partition-data.md#partition-keys) kullanarak [SQL .NET API](documentdb-introduction.md). Bir koleksiyon oluşturduğunuzda bir bölüm anahtarı tanımlayarak, uygulamanızın verilerinizi büyüdükçe harcamadan ölçeklendirmek için hazırlanır. 

Bu öğretici kullanarak aşağıdaki görevleri kapsar [SQL .NET API](documentdb-sdk-dotnet.md):

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma
> * Bir bölüm anahtarının bir veritabanınızı ve koleksiyonunuzu oluşturun
> * JSON belgeleri oluşturma
> * Bir belgeyi güncelleştirme
> * Bölümlenmiş koleksiyonlar sorgulama
> * Saklı yordamları çalıştırma
> * Bir belgeyi silme
> * Bir veritabanını silin

## <a name="prerequisites"></a>Ön koşullar
Lütfen aşağıdakilere sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Azure portalında bir Azure Cosmos DB hesabı oluşturarak başlayalım.

> [!TIP]
> * Zaten Azure Cosmos DB hesabınız var mı? Bu durumda, İleri için atlayabilirsiniz [, Visual Studio çözümü ayarlama](#SetupVS)
> * Azure Cosmos DB öykünücüsü kullanıyorsanız, lütfen bölümündeki adımları izleyin [Azure Cosmos DB öykünücüsü](local-emulator.md) öykünücü kurulması ve için İleri atlayabilirsiniz [, Visual Studio çözümünü kurmak](#SetupVS). 
>
>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Visual Studio çözümünüzü kurma
1. Bilgisayarınızda **Visual Studio**'yu açın.
2. **Dosya** menüsünde **Yeni**'yi seçin ve ardından **Proje**'yi seçin.
3. İçinde **yeni proje** iletişim kutusunda **şablonları** / **Visual C#** / **konsol uygulaması (.NET Framework)** , projenizi adlandırın ve ardından **Tamam**.
   ![Yeni Proje penceresinin ekran görüntüsü](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)

4. **Çözüm Gezgini**'nde Visual Studio çözümünüzün altındaki yeni konsol uygulamanıza sağ tıklayın ve **NuGet Paketlerini Yönet...** öğesine tıklayın.
    
    ![Proje için Sağ Tıklama Menüsünün ekran görüntüsü](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. İçinde **NuGet** sekmesini tıklatın, **Gözat**ve türü **documentdb** arama kutusuna.
<!---stopped here--->
6. Sonuçlarda **Microsoft.Azure.DocumentDB**'yi bulun ve **Yükle**'ye tıklayın.
   Azure Cosmos DB istemci kitaplığı için paket kimliği [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).
   ![Azure Cosmos DB istemci SDK'sını bulmak için NuGet menüsünün ekran görüntüsü](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)

    Çözümdeki değişiklikleri gözden geçirme hakkında iletiler alırsanız **Tamam**'a tıklayın. Lisans kabulü hakkında bir ileti alırsanız **Kabul ediyorum**'a tıklayın.

## <a id="Connect"></a>Başvuruları projenize ekleme
Bu öğreticide kalan adımlar oluşturmak ve Azure Cosmos DB kaynakları projenize güncelleştirmek için gereken SQL API kod parçacıkları sağlar.

İlk olarak, uygulamanız bu başvurular ekleyin.
<!---These aren't added by default when you install the pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <a id="add-references"></a>Uygulamanızı bağlama

Ardından, bu iki sabitleri ekleyin ve *istemci* uygulamanızda değişken.

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

Head ardından, yeniden [Azure portal](https://portal.azure.com) uç noktasının URL'sini ve birincil anahtar alınamadı. Uç nokta URL’si ve birincil anahtar, uygulamanızın nereye bağlanacağını anlaması ve Azure Cosmos DB’nin uygulamanızın bağlantısına güvenmesi için gereklidir.

Azure portalında Azure Cosmos DB hesabınıza gidin, tıklatın **anahtarları**ve ardından **okuma-yazma anahtarları**.

URI Portal'dan kopyalayın ve üzerinden yapıştırın `<your endpoint URL>` program.cs dosyasındaki. Ardından portaldan birincil anahtarı kopyalayın ve üzerinden yapıştırın `<your primary key>`. Kaldırdığınızdan emin olun `<` ve `>` , değerlerinden.

![C# konsol uygulaması oluşturmak için NoSQL Öğreticisi tarafından kullanılan Azure portal ekran görüntüsü. Azure Cosmos DB hesabı dikey penceresinde ANAHTARLARI ve anahtarlar dikey penceresinde URI ve birincil anahtar değerleri içeren bir Azure Cosmos DB hesabını gösterir](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-keys.png)

## <a id="instantiate"></a>DocumentClient örneği

Şimdi, yeni bir örneğini oluşturmak **DocumentClient**.

```csharp
DocumentClient client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
```

## <a id="create-database"></a>Bir veritabanı oluşturun

Ardından, bir Azure Cosmos DB Oluştur [veritabanı](documentdb-resources.md#databases) kullanarak [Documentclient](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) yöntemi veya [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) yöntemi  **DocumentClient** sınıfıyla [SQL .NET SDK'sı](documentdb-sdk-dotnet.md). Veritabanı, koleksiyonlar genelinde bölümlenmiş JSON belgesi depolama alanının mantıksal bir kapsayıcısıdır.

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a>Bir bölüm anahtarı karar verin 

Koleksiyonlar, belgeleri depolamak için kapsayıcılardır. Mantıksal kaynaklar ve yapabilirsiniz [bir veya daha fazla fiziksel bölüm span](partition-data.md). A [bölüm anahtarı](documentdb-partition-data.md) verilerinizi sunucuları veya bölümleri arasında dağıtmak için kullanılan belgelerinizi içinde bir özellik (veya yol) olduğu. Aynı bölüm anahtarına sahip tüm belgeleri aynı bölümünde depolanır. 

Bölüm anahtarı belirleme koleksiyonu oluşturmadan önce yapmak için önemli bir karardır. Bölüm anahtarlarını özelliği (veya yol) birden fazla sunucu veya bölümleri arasında verilerinizi dağıtmak için Azure Cosmos DB tarafından kullanılan belgelerinizi ağdadır. Cosmos DB bölüm anahtarı değerini karma hale getirir ve karma hale getirilen sonuç belge depolanacağı bölüm belirlemek için kullanır. Aynı bölüm anahtarına sahip tüm belgeleri aynı bölümünde depolanır ve bir koleksiyon oluşturulduktan sonra bölüm anahtarlarını değiştirilemez. 

Bu öğretici için bölüm anahtarı kümesine oluşturacağız `/deviceId` tek bir bölüm böylece tüm verileri tek bir cihaz için depolanır. Değerlerin her biri en aynı frekansı hakkında verilerinizi büyür ve koleksiyon tam verimini elde Cosmos DB yükünü dengelemek sağlamak için kullanılır, çok sayıda sahip bir bölüm anahtarı seçmek istediğiniz. 

Bölümleme hakkında daha fazla bilgi için bkz: [bölümleme ve Azure Cosmos veritabanı ölçek?](partition-data.md) 

## <a id="CreateColl"></a>Bir koleksiyon oluşturma 

Biz bizim bölüm anahtarı bildiğinize göre `/deviceId`, oluşturma sağlayan bir [koleksiyonu](documentdb-resources.md#collections) kullanarak [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) yöntemi veya [CreateDocumentCollectionIfNotExistsAsync ](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) yöntemi **DocumentClient** sınıfı. Koleksiyon, JSON belgelerinin ve tüm ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır. 

> [!WARNING]
> Uygulamanın Azure Cosmos DB ile iletişim kurmak işleme ayırma gibi bir koleksiyon oluşturma, fiyatlandırmaya vardır. Daha fazla ayrıntı için lütfen ziyaret bizim [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

```csharp
// Collection for device telemetry. Here the JSON property deviceId is used  
// as the partition key to spread across partitions. Configured for 2500 RU/s  
// throughput and an indexing policy that supports sorting against any  
// number or string property. .
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 2500 });
```

Bu yöntem için Azure Cosmos DB ve hizmet hükümleri istenen işlemeyi temel alan bölüm sayısı çağırın bir REST API sağlar. Performansınızı SDK'sını kullanarak geliştikçe koleksiyonu verimini değiştirebilir veya [Azure portal](set-throughput.md).

## <a id="CreateDoc"></a>JSON belgeleri oluşturma
Şimdi bazı JSON belgeleri Azure Cosmos Veritabanına ekler. Bir [belge](documentdb-resources.md#documents), **DocumentClient** sınıfının [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) yöntemi kullanılarak oluşturulabilir. Belgeler, kullanıcı tanımlı (rastgele) JSON içerikleridir. Bu örnek sınıfı okuma bir aygıtı ve Documentclient bir koleksiyona okuma yeni aygıt eklemek için bir çağrı içerir.

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here the partition key is extracted 
// as "XMS-0001" based on the collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```
## <a name="read-data"></a>Verileri okuma

Şimdi ReadDocumentAsync yöntemi kullanılarak kimliği ve bölüm anahtarı tarafından belgeyi okuyun. Okumaları PartitionKey değeri içerdiğini unutmayın (karşılık gelen `x-ms-documentdb-partitionkey` REST API istek üstbilgisinde).

```csharp
// Read document. Needs the partition key and the Id to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a>Verileri güncelleştirme

Şimdi şimdi ReplaceDocumentAsync yöntemini kullanarak bazı verileri güncelleştirin.

```csharp
// Update the document. Partition key is not required, again extracted from the document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);
```

## <a name="delete-data"></a>Verileri silme

Şimdi bir belgenin bölüm anahtarı kimliği DeleteDocumentAsync yöntemini kullanarak silip olanak sağlar.

```csharp
// Delete a document. The partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a>Bölümlenmiş koleksiyonlar sorgulama

Bölümlenmiş koleksiyonlar verileri sorguladığınızda Azure Cosmos DB sorgu otomatik olarak (varsa) filtrede belirtilen bölüm anahtarı değerine karşılık gelen bölümleri yönlendirir. Örneğin, bu sorgu "XMS-0001" Bölüm anahtarı içeren bölümün yalnızca yönlendirilir.

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
Aşağıdaki sorgu bir filtre bölüm anahtarı (DeviceID) sahip değil ve bölümün dizin karşı yürütüldüğü tüm bölümler için Dağıtılmış. EnableCrossPartitionQuery belirtmek zorunda Not (`x-ms-documentdb-query-enablecrosspartition` REST API'sindeki) SDK'sını bölümler bir sorguyu çalıştırmak için.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a>Paralel sorgu yürütme
Azure Cosmos DB SQL SDK 1.9.0 ve hatta bunlar çok sayıda bölüm touch gerektiğinde bölümlenmiş koleksiyonlar, düşük gecikme süresi sorguları gerçekleştirmesine izin destek paralel sorgu yürütme seçenekleri üstünde. Örneğin, aşağıdaki sorguyu bölümler paralel olarak çalıştırmak için yapılandırılır.

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
Aşağıdaki parametreleri ayarlama tarafından paralel sorgu yürütme yönetebilirsiniz:

* Ayarlayarak `MaxDegreeOfParallelism`, yani, en fazla eşzamanlı ağ bağlantı sayısı koleksiyonunun bölümlere paralellik derecesini kontrol edebilirsiniz. Bu ayar, -1 olarak paralellik derecesini SDK tarafından yönetilir. Varsa `MaxDegreeOfParallelism` varsayılan değer, belirtilen veya ayarlanmış 0 değil, tek bir ağ bağlantısı koleksiyonunun bölümlere olacaktır.
* Ayarlayarak `MaxBufferedItemCount`, sorgu gecikme süresi ve istemci tarafı bellek kullanımı devre dışı ticari. Bu parametreyi veya bu ayarlarsanız -1 olarak paralel sorgu yürütme sırasında arabelleğe alınan öğe sayısı SDK tarafından yönetilir.

Koleksiyon aynı durumu verildiğinde, paralel sorgu sonuçları seri yürütme olduğu gibi aynı sırada döndürür. Sıralama (ORDER BY ve/veya üst) içeren bir çapraz bölüm sorgusu gerçekleştirirken, SQL SDK'sı paralel sorguda bölümler sorunları ve genel olarak sipariş edilen sonuçlar için istemci tarafı kısmen sıralanmış sonuçları birleştirir.

## <a name="execute-stored-procedures"></a>Saklı yordam yürütme
Son olarak, aynı aygıt kimliği belgelerle karşı atomik işlemleri örneğin yürütebilir, toplamalar veya bir cihazı tek bir belgenin en son durumunu projenize aşağıdaki kodu ekleyerek koruma durumunda.

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```

Ve bu kadar! ana veri dağıtım bölümler ölçeklendirilmesine olanak bölüm anahtarı kullanan bir Azure Cosmos DB uygulama bileşenlerinin olanlardır.  

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmek için değil kullanacaksanız, bu öğreticide aşağıdaki adımlarla Azure portal tarafından oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden **kaynak grupları** ve oluşturduğunuz kaynak benzersiz adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakileri yaptığınızdan: 

> [!div class="checklist"]
> * Bir Azure Cosmos DB hesabı oluşturuldu
> * Bir veritabanı ve koleksiyonu bir bölüm anahtarı ile oluşturulan
> * Oluşturulan JSON belgeleri
> * Bir belge güncelleştirildi
> * Sorgulanan bölümlenmiş koleksiyonlar
> * Saklı yordam çalıştı
> * Bir belge silindi
> * Bir veritabanı silindi

Şimdi, sonraki öğretici devam ve Cosmos DB hesabınıza ek verileri alın. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)
