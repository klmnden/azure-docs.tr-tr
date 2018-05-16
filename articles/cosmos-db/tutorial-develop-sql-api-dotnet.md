---
title: "Azure Cosmos DB: .NET'te SQL API ile geliştirme | Microsoft Docs"
description: .NET kullanarak Azure Cosmos DB SQL API'si ile geliştirmeyi öğrenin
services: cosmos-db
documentationcenter: ''
author: rafats
manager: kfile
editor: ''
tags: ''
ms.assetid: ''
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 05/10/2017
ms.author: rafats
ms.custom: mvc
ms.openlocfilehash: 528832473d68fa90e6383873b1e0491f5abe09c7
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="azure-cosmos-db-develop-with-the-sql-api-in-net"></a>Azure Cosmos DB: .NET'te SQL API ile geliştirme

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz.

Bu öğreticide Azure portalı kullanılarak Azure Cosmos DB hesabı oluşturma, ardından [SQL .NET API](sql-api-introduction.md) kullanılarak [bölüm anahtarı](sql-api-partition-data.md#partition-keys) ile belge veritabanı ve koleksiyon oluşturma işlemleri gösterilir. Koleksiyon oluştururken bölüm anahtarı tanımlandığında, verileriniz arttıkça uygulamanız zorlanmadan ölçeklendirilmeye hazırlanır.

Bu öğretici [SQL. NET API](sql-api-sdk-dotnet.md) kullanılarak aşağıda görevlerin yerine getirilmesini kapsar:

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma
> * Bölüm anahtarıyla veritabanı ve koleksiyon oluşturma
> * JSON belgeleri oluşturma
> * Bir belgeyi güncelleştirme
> * Bölümlenmiş koleksiyonları sorgulama
> * Saklı yordamları çalıştırma
> * Bir belgeyi silme
> * Veritabanı silme

## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce şunlara sahip olduğunuzdan emin olun:

* Bir Azure Cosmos DB hesabına erişme

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

  [Ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) için kaydolarak kendi Azure aboneliğinizi de kullanabilirsiniz. Bundan sonra, [Azure Cosmos DB hesabı oluşturabilirsiniz](create-sql-api-dotnet.md#create-a-database-account).

* Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.


> [!TIP]
> * Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız öykünücünün kurulumunu gerçekleştirmek için lütfen [Azure Cosmos DB Öykünücüsü](local-emulator.md) konusundaki adımları izleyin
>
>


## <a id="SetupVS"></a>Visual Studio çözümünüzü kurma
1. Bilgisayarınızda **Visual Studio**'yu açın.
2. **Dosya** menüsünde **Yeni**'yi seçin ve ardından **Proje**'yi seçin.
3. **Yeni Proje** iletişim kutusunda, **Şablonlar** / **Visual C#** / **Konsol Uygulaması (.NET Framework)** öğesini seçin, projenizi adlandırın ve ardından **Tamam**'a tıklayın.
   ![Yeni Proje penceresinin ekran görüntüsü](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-new-project-2.png)

4. **Çözüm Gezgini**'nde Visual Studio çözümünüzün altındaki yeni konsol uygulamanıza sağ tıklayın ve **NuGet Paketlerini Yönet...** öğesine tıklayın.

    ![Proje için Sağ Tıklama Menüsünün ekran görüntüsü](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. **NuGet** sekmesinde **Gözat**'a tıklayın ve arama kutusuna **documentdb** yazın.
<!---stopped here--->
6. Sonuçlarda **Microsoft.Azure.DocumentDB**'yi bulun ve **Yükle**'ye tıklayın.
   Azure Cosmos DB İstemci Kitaplığı'nın paket kimliği [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB)’dir.
   ![Azure Cosmos DB İstemci SDK'sını bulmak için NuGet Menüsünün ekran görüntüsü](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)

    Çözümdeki değişiklikleri gözden geçirme hakkında iletiler alırsanız **Tamam**'a tıklayın. Lisans kabulü hakkında bir ileti alırsanız **Kabul ediyorum**'a tıklayın.

## <a id="Connect"></a>Projenize başvurular ekleme
Bu öğreticinin kalan adımları, projenizde Azure Cosmos DB kaynaklarını oluşturmak ve güncelleştirmek için gereken SQL API kod parçacıklarını sağlar.

İlk olarak, bu başvuruları uygulamanıza ekleyin.
<!---These aren't added by default when you install the pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <a id="add-references"></a>Uygulamanızı bağlama

Sonra bu iki sabiti ve *client* değişkeninizi uygulamanıza ekleyin.

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

Ardından, uç nokta URL’nizi ve birincil anahtarınızı almak için tekrar [Azure portalına](https://portal.azure.com) gidin. Uç nokta URL’si ve birincil anahtar, uygulamanızın nereye bağlanacağını anlaması ve Azure Cosmos DB’nin uygulamanızın bağlantısına güvenmesi için gereklidir.

Azure portalda Azure Cosmos DB hesabınıza gidin. Sol taraftaki menüde, **Anahtarlar**’ı ve ardından **Okuma-Yazma Anahtarları**’nı seçin.

Portaldaki URI’yi kopyalayın ve program.cs dosyasındaki `<your endpoint URL>` üzerine yapıştırın. Ardından portaldan BİRİNCİL ANAHTARI kopyalayın ve `<your primary key>` üzerine yapıştırın. Değerlerinizden `<` ve `>` bölümünü kaldırdığınızdan emin olun.

![Bir C# konsol uygulaması oluşturmak için NoSQL öğreticisi tarafından kullanılan Azure portalının ekran görüntüsü. Azure Cosmos DB hesabı bölümünde ANAHTARLAR vurgulanmış ve Anahtarlar bölümünde URI ve BİRİNCİL ANAHTAR değerleri vurgulanmış bir Azure Cosmos DB hesabını gösterir](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-keys.png)

## <a id="instantiate"></a>DocumentClient örneği oluşturma

Şimdi yeni bir **DocumentClient** örneği oluşturun.

```csharp
DocumentClient client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
```

## <a id="create-database"></a>Veritabanı oluşturma

Ardından, [SQL .NET SDK](sql-api-sdk-dotnet.md)'dan **DocumentClient** sınıfının [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) yöntemini veya [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) yöntemini kullanarak bir Azure Cosmos DB [veritabanı](sql-api-resources.md#databases) oluşturun. Veritabanı, koleksiyonlar genelinde bölümlenmiş JSON belgesi depolama alanının mantıksal bir kapsayıcısıdır.

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a>Bölüm anahtarına karar verme

Koleksiyonlar, belgelerin depolandığı kapsayıcılardır. Bunlar mantıksal kaynaklardır ve [bir veya birden çok fiziksel bölüme yayılabilir](partition-data.md). [Bölüm anahtarı](sql-api-partition-data.md), belgelerinizin içinde yer alan ve verilerinizi sunucular veya bölümler arasında dağıtmak için kullanılan bir özelliktir (veya yol). Aynı bölüm anahtarına sahip olan tüm belgeler aynı bölümü paylaşır.

Bölüm anahtarı belirlemek, koleksiyon oluşturmadan önce verilmesi gereken önemli bir karardır. Bölüm anahtarları, belgelerinizin içinde yer alan ve Azure Cosmos DB tarafından verilerinizi birden çok sunucu veya bölüm arasında dağıtmak için kullanılabilen bir özelliktir (veya yol). Cosmos DB bölüm anahtarı değerini karma haline getirir ve belgeyi hangi bölümde depolayacağını belirlemek için karma sonucu kullanır. Aynı bölüm anahtarına sahip tüm belgeler aynı bölümünde depolanır ve koleksiyon oluşturulduktan sonra bölüm anahtarları değiştirilemez.

Bu öğretici için, tek bir cihazdaki tüm verilerin tek bölümde depolanmasını sağlayacak şekilde bölüm anahtarını `/deviceId` olarak ayarlamanız gerekecek. Verileriniz arttıkça Cosmos DB'nin yükü dengeleyebilmesi ve koleksiyonda tam aktarım hızına ulaşılabilmesi için, her biri yaklaşık aynı sıklıkta kullanılan çok fazla sayıda değeri olan bir bölüm anahtarı seçmek istersiniz.

Bölümleme hakkında daha fazla bilgi için bkz. [Azure Cosmos DB'de bölümleme ve ölçeklendirme](partition-data.md)

## <a id="CreateColl"></a>Koleksiyon oluşturma

Bölüm anahtarı ile (`/deviceId`), **DocumentClient** sınıfının [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) yöntemini veya [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) yöntemini kullanarak bir [koleksiyon](sql-api-resources.md#collections) oluşturabilirsiniz. Koleksiyon, JSON belgelerinin ve ilişkili her JavaScript uygulama mantığının kapsayıcısıdır.

> [!WARNING]
> Koleksiyon oluşturmanın bir fiyatı vardır çünkü Azure Cosmos DB'yle iletişim kurması için uygulamaya aktarım hızı ayırırsınız. Daha ayrıntılı bilgi için lütfen [fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin
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

Bu yöntemde Azure Cosmos DB'ye bir REST API çağrısı yapılır ve hizmet, istenen aktarım hızı temelinde belirli bir sayıda bölüm sağlar. Performans gereksinimleriniz arttıkça, SDK'yı veya [Azure portalını](set-throughput.md) kullanarak bir koleksiyonun veya koleksiyon kümesinin aktarım hızını değiştirebilirsiniz.

## <a id="CreateDoc"></a>JSON belgeleri oluşturma
Şimdi de Azure Cosmos DB'ye bazı JSON belgeleri ekleyelim. Bir [belge](sql-api-resources.md#documents), **DocumentClient** sınıfının [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) yöntemi kullanılarak oluşturulabilir. Belgeler, kullanıcı tanımlı (rastgele) JSON içerikleridir. Bu örnek sınıf bir cihaz okuması ve koleksiyona yeni cihaz okuması eklemek için CreateDocumentAsync çağrısı içerir.

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

Şimdi ReadDocumentAsync yöntemini kullanarak bölüm anahtarına ve kimliğine göre belgeyi okuyalım. Okumaların bir PartitionKey değeri (REST API'de `x-ms-documentdb-partitionkey` istek üst bilgisine karşılık gelir) içerdiğine dikkat edin.

```csharp
// Read document. Needs the partition key and the Id to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"),
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a>Verileri güncelleştirme

Şimdi ReplaceDocumentAsync yöntemini kullanarak bazı verileri güncelleştirelim.

```csharp
// Update the document. Partition key is not required, again extracted from the document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"),
  reading);
```

## <a name="delete-data"></a>Verileri silme

Şimdi DeleteDocumentAsync yöntemini kullanarak bölüm anahtarına ve kimliğine göre bir belgeyi silelim.

```csharp
// Delete a document. The partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"),
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a>Bölümlenmiş koleksiyonları sorgulama

Bölümlenmiş koleksiyonlardaki verileri sorguladığınızda, Azure Cosmos DB sorguyu otomatik olarak filtrede belirtilen bölüm anahtarı değerlerine (varsa) karşılık gelen bölümlere yönlendirir. Örneğin, bu sorgu doğrudan "XMS-0001" bölüm anahtarını içeren bölüme yönlendirilir.

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```

Aşağıdaki sorgunun bölüm anahtarında (DeviceId) filtresi yoktur ve bölümün dizinine göre yürütüldüğü tüm bölümleri yayılır. SDK'nın bölümler arasında sorgu yürütmesini sağlamak için EnableCrossPartitionQuery (REST API'de `x-ms-documentdb-query-enablecrosspartition`) belirtmeniz gerektiğini aklınızda bulundurun.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a>Paralel sorgu yürütme
Azure Cosmos DB SQL SDK 1.9.0 ve üstü, çok fazla sayıda bölüme dokunması gerekse bile, bölümlenmiş sorgularda düşük gecikme süreli sorgular yürütmenize olanak tanıyan paralel sorgu yürütme seçeneklerini destekler. Örneğin, aşağıdaki sorgu bölümler arasında paralel çalıştırılacak şekilde yapılandırılmıştır.

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```

Aşağıdaki parametreleri ayarlayarak paralel sorgu yürütme işlemini yönetebilirsiniz:

* `MaxDegreeOfParallelism` parametresini ayarlayarak paralellik derecesini, başka bir deyişle koleksiyonun bölümlerine yönelik eşzamanlı ağ bağlantısı sayısı üst sınırını denetleyebilirsiniz. Bu parametreyi -1 olarak ayarlarsanız, paralellik derecesi SDK tarafından yönetilir. `MaxDegreeOfParallelism` belirtilmezse veya 0 olarak ayarlanırsa, koleksiyonun bölümlerine tek ağ bağlantısı kurulur.
* `MaxBufferedItemCount` parametresini ayarlayarak, sorgu gecikme süresiyle istemci tarafı bellek kullanımını dengeleyebilirsiniz. Bu parametreyi atlarsanız veya -1 olarak ayarlarsanız, paralel sorgu yürütme işlemi sırasında arabelleğe alınan öğelerin sayısı SDK tarafından yönetilir.

Koleksiyonun durumu aynı olduğunda, paralel sorgu sonuçları seri yürütme ile aynı sırada döndürür. Sıralama (ORDER BY ve/veya TOP) içeren bir bölümler arası sorgu gerçekleştirirken, SQL SDK sorguyu bölümler arasında paralel olarak gönderir ve genel olarak sıralanmış sonuçları oluşturmak için sunucu tarafında kısmen sıralanmış sonuçları birleştirir.

## <a name="execute-stored-procedures"></a>Saklı yordamları yürütme
Son olarak, cihaz kimliği aynı olan belgelerde (örneğin cihazın toplamlarını veya son durumunu tek belgede tutuyorsanız) projenize aşağıdaki kodu ekleyerek atomik işlemler yürütebilirsiniz.

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") },
    "XMS-001-FE24C");
```

Hepsi bu! Bölümler arasında veri dağılımını verimli bir şekilde ölçeklendirmek için bölüm anahtarı kullanan bir Azure Cosmos DB uygulamasının ana bileşenleri bunlardır.  

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu öğretici tarafından oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın benzersiz adına tıklayın.
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma
> * Bölüm anahtarıyla veritabanı ve koleksiyon oluşturma
> * JSON belgeleri oluşturma
> * Belgeyi güncelleştirme
> * Bölümlenmiş koleksiyonları sorgulama
> * Saklı yordam çalıştırma
> * Belgeyi silme
> * Veritabanını silme

Şimdi bir sonraki öğreticiye geçebilir ve Cosmos DB hesabınıza ek veri aktarabilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)
