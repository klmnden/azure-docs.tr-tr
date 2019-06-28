---
title: 'Öğretici: Birden çok veri kaynağına - Azure Search dizini'
description: Birden çok veri kaynağından tek bir Azure Search dizinine veri aktarmak hakkında bilgi edinin.
author: RobDixon22
manager: HeidiSteen
services: search
ms.service: search
ms.topic: tutorial
ms.date: 06/21/2019
ms.author: v-rodixo
ms.custom: seodec2018
ms.openlocfilehash: 4186c422836771de4f8a283616d77214b91bfc02
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462705"
---
# <a name="c-tutorial-combine-data-from-multiple-data-sources-in-one-azure-search-index"></a>C#Öğretici: Bir Azure Search dizini içinde birden çok veri kaynaklarından alınan verileri birleştirin

Azure arama, içeri aktarma, analiz ve birden çok veri kaynaklarından alınan verileri tek bir birleştirilmiş aramada dizine dizin. Bu durumlarda burada yapılandırılmış verileri toplanır şekilli ve hatta düz metin verilerle metin, HTML, gibi diğer kaynaklardan destekler veya JSON belgeleri.

Bu öğreticide, Azure Blob Depolama belgelerini çizilmiş otel odası ayrıntıları birleştirmek ve Otel verileri bir Azure Cosmos DB veri kaynağından dizin açıklar. Sonuç, karmaşık veri türlerini içeren bir birleşik otel arama dizini olacaktır.

Bu öğreticide C#, .NET SDK'sı Azure Search ve Azure portalı aşağıdaki görevleri gerçekleştirmek:

> [!div class="checklist"]
> * Örnek verileri karşıya yükleme ve veri kaynakları oluşturma
> * Belge anahtarını tanımlama
> * Tanımlama ve dizin oluşturma
> * CosmosDb otel verileri dizini
> * Blob depolamadan otelinden oda verilerini birleştirme

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta, aşağıdaki hizmetleri, araçları ve verileri kullanılır. 

- [Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu öğretici için ücretsiz bir hizmet kullanabilirsiniz.

- [Bir Azure Cosmos DB hesabı oluşturmayı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) örnek otel verileri depolamak için.

- [Bir Azure depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) örnek JSON depolamak için blob verileri.

- [Visual Studio yükleme](https://visualstudio.microsoft.com/) IDE kullanılacak.

### <a name="install-the-project-from-github"></a>Projeyi Github'dan yükleyin

1. GitHub üzerinde örnek depoyu bulun: [azure-search-dotnet-samples](https://github.com/Azure-Samples/azure-search-dotnet-samples).
1. Seçin **Kopyala veya indir** ve özel yerel kopyanızı deponun yapın.
1. Visual Studio'yu açın ve zaten yüklü değilse, Microsoft Azure arama NuGet paketini yükleyin. İçinde **Araçları** menüsünde **NuGet Paket Yöneticisi** ardından **çözüm için NuGet paketlerini Yönet...** . Seçin **Gözat** sekmesinde ve arama kutusuna "Azure Search" yazın. Yükleme **Microsoft.Azure.Search** listesinde göründüğünde (9.0.1, sürümü veya üzeri). Yüklemeyi tamamlamak için ek iletişim kutularının tıklamanız gerekir.

    ![NuGet kullanarak Azure kitaplıkları eklemek](./media/tutorial-csharp-create-first-app/azure-search-nuget-azure.png)

1. Visual Studio kullanarak, yerel depoya gidin ve çözüm dosyasını açın **AzureSearchMultipleDataSources.sln**.

## <a name="get-a-key-and-url"></a>Bir anahtarı ve URL alma

Azure Search hizmetiniz ile etkileşim için hizmet URL'si ve erişim anahtarı gerekir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinizdeki **genel bakış** sayfa olduğunda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. İçinde **ayarları** > **anahtarları**, hizmette tam haklarına yönelik bir yönetici anahtarını alın. Bir gece yarısında gerektiği durumlarda iş sürekliliği için sağlanan iki birbirinin yerine yönetici anahtarı mevcuttur. Ekleme, değiştirme ve silme nesneler için istekleri birincil veya ikincil anahtar kullanabilirsiniz.

![Bir HTTP uç noktası ve erişim anahtarını alma](media/search-fiddler/get-url-key.png "bir HTTP uç noktası ve erişim anahtarını alma")

Tüm istekleri hizmete gönderilen her istekte bir API anahtarı gerektirir. Geçerli bir anahtar, istek başına temelinde, uygulama gönderme isteği ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="prepare-sample-azure-cosmos-db-data"></a>Örnek Azure Cosmos DB verileri hazırlama

Bu örnek iki küçük yedi kurgusal hotels açıklayan bir veri kümesini kullanır. Bir kümenin hotels açıklar ve bir Azure Cosmos DB veritabanına yüklenir. Başka bir küme otel odası ayrıntılarını içerir ve Azure Blob depolamaya yüklenecek yedi ayrı JSON dosyası olarak sağlanır.

1. [Azure portalında oturum açın](https://portal.azure.com)ve ardından Azure Cosmos DB hesap genel bakış sayfanıza gidin.

1. Kapsayıcı Ekle menü çubuğundan tıklayın. "Yeni veritabanı oluştur" belirtin ve adını kullanın **otelinden oda db**. Girin **otelinden oda** için koleksiyon adı ve **/HotelId** bölüm anahtarı. Tıklayın **Tamam** veritabanı ve kapsayıcı oluşturmak için.

   ![Azure Cosmos DB kapsayıcısı Ekle](media/tutorial-multiple-data-sources/cosmos-add-container.png "bir Azure Cosmos DB kapsayıcısı Ekle")

1. Cosmos DB veri Gezgini'ne gidin ve seçin **öğeleri** öğesi altında **hotels** kapsayıcıda **otelinden oda db** veritabanı. Ardından **karşıya yükle öğesi** komut çubuğunda.

   ![Azure Cosmos DB koleksiyonuna](media/tutorial-multiple-data-sources/cosmos-upload.png "Cosmos DB koleksiyonu için karşıya yükleme")

1. Karşıya yükleme panelinde klasör düğmesine tıklayın ve ardından dosyaya gidin **cosmosdb/HotelsDataSubset_CosmosDb.json** proje klasöründeki. Tıklayın **Tamam** karşıya yüklemeyi başlatmak için.

   ![Karşıya yüklenecek Dosya Seç](media/tutorial-multiple-data-sources/cosmos-upload2.png "karşıya yüklenecek dosya seçin")

1. Hotels koleksiyondaki öğelerin görünümünü yenilemek için Yenile düğmesini kullanın. Listelenen yedi yeni veritabanı belgeleri görmeniz gerekir.

## <a name="prepare-sample-blob-data"></a>Örnek blob verileri hazırlama

1. [Azure portalında oturum açın](https://portal.azure.com), Azure depolama hesabınıza gidin, tıklayın **Blobları**ve ardından **+ kapsayıcı**.

1. [Bir blob kapsayıcısı oluşturursunuz](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) adlı **otelinden oda** örnek otel odası JSON dosyalarını depolamak için. Genel erişim düzeyi geçerli değerleri birini ayarlayabilirsiniz.

   ![Bir blob kapsayıcısı oluşturursunuz](media/tutorial-multiple-data-sources/blob-add-container.png "blob kapsayıcısı oluşturma")

1. Kapsayıcıyı oluşturduktan sonra dosyayı açın ve seçin **karşıya** komut çubuğunda.

   ![Komut çubuğunda karşıya](media/search-semi-structured-data/upload-command-bar.png "komut çubuğunda karşıya yükleme")

1. Örnek dosyalarını içeren klasöre gidin. Bunların tümünü seçin ve ardından **karşıya**.

   ![Dosyaları karşıya yükleme](media/tutorial-multiple-data-sources/blob-upload.png "dosyaları karşıya yükle")

Karşıya yükleme tamamlandıktan sonra dosyalar veri kapsayıcısı için listede görünmelidir.

## <a name="set-up-connections"></a>Bağlantıları ayarlama

Arama hizmeti ve veri kaynakları için bağlantı bilgilerini belirtilen **appsettings.json** çözümdeki dosyası. 

1. Visual Studio'da açın **AzureSearchMultipleDataSources.sln** dosya.

1. Çözüm Gezgini'nde Düzenle **appsettings.json** dosya.  

```json
{
  "SearchServiceName": "Put your search service name here",
  "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
  "BlobStorageAccountName": "Put your Azure Storage account name here",
  "BlobStorageConnectionString": "Put your Azure Blob Storage connection string here",
  "CosmosDBConnectionString": "Put your Cosmos DB connection string here",
  "CosmosDBDatabaseName": "hotel-rooms-db"
}
```

İlk iki girişe, Azure Search hizmetinizin URL'si ve yönetici tuşlarını kullanın. Bir uç nokta, verilen `https://mydemo.search.windows.net`, örneğin, sağlamak için hizmet adı; `mydemo`.

Sonraki girişleri hesap adlarını ve Azure Blob Depolama ve Azure Cosmos DB veri kaynakları için bağlantı dizesi bilgilerini belirtin.

### <a name="identify-the-document-key"></a>Belge anahtarını tanımlama

Azure Search'te anahtar alanı dizindeki her belgenin benzersiz olarak tanımlar. Her arama dizini türünde tam olarak bir anahtar alan olmalıdır `Edm.String`. Bu anahtar alanı dizine eklenen bir veri kaynağındaki her belge için mevcut olmalıdır. (Aslında, yalnızca gerekli bir alandır.)

Birden çok veri kaynaklarından alınan verileri için dizin oluşturulurken, her veri kaynağı anahtar değeri aynı anahtar alan olarak birleştirilmiş eşlenmelidir. Bu genellikle bazı ön anlamlı belge anahtarını dizininiz için tanımlamak ve her bir veri kaynağında mevcut olduğundan emin planlama yapmak gerekir.

Kaynak verileri için doğru dizin alanı yönlendirilebilir böylece azure Search dizin oluşturucularında alan eşlemeleri yeniden adlandırmak ve veri alanlarını dizin oluşturma işlemi sırasında bile yeniden biçimlendirmek için kullanabilirsiniz.

Örneğin, bizim örnek CosmosDB verilerde otel tanımlayıcısı çağrılır **HotelId**. Ancak otelinden oda için JSON blob dosyalarını otel tanımlayıcı adlı **kimliği**. Program bu eşleyerek işler **kimliği** blob'lara alanını **HotelId** anahtar alan olarak.

> [!NOTE]
> Çoğu durumda, iyi belge anahtarları birleşik dizinler için varsayılan olarak bazı Dizin Oluşturucu tarafından oluşturulanlar gibi otomatik olarak oluşturulan belge anahtarları yapmayın. Genel olarak zaten var. bir anlamlı ve benzersiz anahtar değer kullanmak istemeniz veya veri kaynaklarınız için kolayca eklenebilir.

## <a name="understand-the-code"></a>Kodu anlama

Verileri ve yapılandırma ayarları yerinde olduğunda, örnek program **AzureSearchMultipleDataSources.sln** derlemek ve çalıştırmak hazır olmanız gerekir.

Bu basit C#/.NET konsol uygulaması, aşağıdaki görevleri gerçekleştirir:
* Veri yapısına dayalı yeni bir Azure Search dizini oluşturur C# (aynı zamanda adres ve alan sınıfları başvuran) otel sınıfı.
* Bir Azure Cosmos DB veri kaynağı ve dizin alanları için Azure Cosmos DB veri eşleyen bir dizin oluşturur.
* CosmosDB dizin oluşturucu, otel verileri yüklemek için çalışır.
* Bir Azure Blob Depolama veri kaynağı ve dizin alanları için JSOn Blob verilerini eşleyen bir dizin oluşturur.
* Azure blob depolama dizin oluşturucu odaları verileri yüklemek için çalışır.

 Programı çalıştırmadan önce kod ve bu örnek için dizin ve dizin oluşturucu tanımlarını incelemek için bir dakikanızı ayırın. İlgili kod iki dosyada yer alır:

  + **Hotel.cs** dizini tanımlayan şemayı içerir
  + **Program.cs** Azure Search dizini, veri kaynağı ve dizin oluşturma ve birleştirilmiş sonuçlar dizine yükleme işlevler içerir.

### <a name="define-the-index"></a>Bir dizin tanımla

Bu örnek program tanımlayın ve Azure Search dizini oluşturmak için .NET SDK'sını kullanır. Avantajlarından yararlanır [FieldBuilder](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.fieldbuilder) bir dizin yapısının oluşturmak için sınıf bir C# veri model sınıfı.

Veri modeli, aynı zamanda adres ve alan sınıfları başvuruları içeren otel sınıfı tarafından tanımlanır. Dizini için bir karmaşık veri yapısı oluşturmak için birden fazla sınıf tanımları aracılığıyla FieldBuilder tatbikatları aşağı. Meta veri etiketleri aranabilir veya sıralanabilir olup gibi her alanın özniteliklerini tanımlamak için kullanılır.

Aşağıdaki kod parçacıklarının gelen **Hotel.cs** dosyasını Göster nasıl tek bir alan ve başka bir veri modeli sınıfı, başvuru belirtilebilir.

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
public Room[] Rooms { get; set; }
. . .
```

İçinde **Program.cs** bir ad ve bir alan koleksiyon tarafından üretilen ile tanımlanan dosya, dizin `FieldBuilder.BuildForType<Hotel>()` yöntemi ve şu şekilde oluşturulur:

```csharp
private static async Task CreateIndex(string indexName, SearchServiceClient searchService)
{
    // Create a new search index structure that matches the properties of the Hotel class.
    // The Address and Room classes are referenced from the Hotel class. The FieldBuilder
    // will enumerate these to create a complex data structure for the index.
    var definition = new Index()
    {
        Name = indexName,
        Fields = FieldBuilder.BuildForType<Hotel>()
    };
    await searchService.Indexes.CreateAsync(definition);
}
```

### <a name="create-azure-cosmos-db-data-source-and-indexer"></a>Azure Cosmos DB veri kaynağı ve dizin oluşturucu oluşturma

Sonraki ana program hotels veriler için Azure Cosmos DB veri kaynağı oluşturmak için mantığı içerir.

İlk Azure Cosmos DB veritabanı adını bağlantı dizesine birleştirir. Ardından Azure Cosmos DB kaynaklarına [useChangeDetection] özelliği gibi belirli ayarlar dahil olmak üzere veri kaynağı nesnesi tanımlar.

  ```csharp
private static async Task CreateAndRunCosmosDbIndexer(string indexName, SearchServiceClient searchService)
{
    // Append the database name to the connection string
    string cosmosConnectString = 
        configuration["CosmosDBConnectionString"]
        + ";Database=" 
        + configuration["CosmosDBDatabaseName"];

    DataSource cosmosDbDataSource = DataSource.CosmosDb(
        name: configuration["CosmosDBDatabaseName"], 
        cosmosDbConnectionString: cosmosConnectString,
        collectionName: "hotels",
        useChangeDetection: true);

    // The Azure Cosmos DB data source does not need to be deleted if it already exists,
    // but the connection string might need to be updated if it has changed.
    await searchService.DataSources.CreateOrUpdateAsync(cosmosDbDataSource);
  ```

Veri kaynağı oluşturulduktan sonra program adlı bir Azure Cosmos DB dizinleyici ayarlar **Otel-odaları-cosmos-dizin oluşturucu**.

```csharp
    Indexer cosmosDbIndexer = new Indexer(
        name: "hotel-rooms-cosmos-indexer",
        dataSourceName: cosmosDbDataSource.Name,
        targetIndexName: indexName,
        schedule: new IndexingSchedule(TimeSpan.FromDays(1)));
    
    // Indexers keep metadata about how much they have already indexed.
    // If we already ran this sample, the indexer will remember that it already
    // indexed the sample data and not run again.
    // To avoid this, reset the indexer if it exists.
    bool exists = await searchService.Indexers.ExistsAsync(cosmosDbIndexer.Name);
    if (exists)
    {
        await searchService.Indexers.ResetAsync(cosmosDbIndexer.Name);
    }
    await searchService.Indexers.CreateOrUpdateAsync(cosmosDbIndexer);
```
Bu örnek birden çok kez çalıştırmak istediğiniz durumda program yeni bir tane oluşturmadan önce aynı ada sahip mevcut tüm dizin oluşturucular siler.

Bu örnek dizin oluşturucu için bir zamanlama tanımlar günde bir kez çalışır. Gelecekte otomatik olarak yeniden çalıştırmak için dizin oluşturucuyu istemiyorsanız, bu çağrıdan schedule özelliği kaldırabilirsiniz.

### <a name="index-azure-cosmos-db-data"></a>Azure Cosmos DB dizin verileri

Veri kaynağı ve dizin oluşturucu oluşturulduktan sonra Dizin Oluşturucu çalışan kodu kısa:

```csharp
    try
    {
        await searchService.Indexers.RunAsync(cosmosDbIndexer.Name);
    }
    catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
    {
        Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
    }
```

Bu örnek, yürütme sırasında oluşabilecek hataları bildirmek için bir basit try-catch bloğu içerir.

Azure Cosmos DB dizinleyici çalıştırdıktan sonra arama dizini tam bir örnek otel belge kümesini içerir. Ancak, Azure Cosmos DB veri kaynağı yok odası ayrıntıları yer alan bu yana yer alan her otel için boş bir dizi olacaktır. Ardından, programı yüklemek ve oda verilerini birleştirmek için Blob depolama alanından çeker.

### <a name="create-blob-storage-data-source-and-indexer"></a>BLOB Depolama veri kaynağı ve dizin oluşturucu oluşturma

Oda ayrıntıları almak için program ilk bir Blob Depolama veri kaynağını yedeklemeye JSON blob dosyaları tek tek başvurmak için ayarlar.

```csharp
private static async Task CreateAndRunBlobIndexer(string indexName, SearchServiceClient searchService)
{
    DataSource blobDataSource = DataSource.AzureBlobStorage(
        name: configuration["BlobStorageAccountName"],
        storageConnectionString: configuration["BlobStorageConnectionString"],
        containerName: "hotel-rooms");

    // The blob data source does not need to be deleted if it already exists,
    // but the connection string might need to be updated if it has changed.
    await searchService.DataSources.CreateOrUpdateAsync(blobDataSource);
```

Veri kaynağı oluşturulduktan sonra program adlı bir blob dizin oluşturucu ayarlar **Otel-odaları-blob-dizin oluşturucu**.

```csharp
    // Add a field mapping to match the Id field in the documents to 
    // the HotelId key field in the index
    List<FieldMapping> map = new List<FieldMapping> {
        new FieldMapping("Id", "HotelId")
    };

    Indexer blobIndexer = new Indexer(
        name: "hotel-rooms-blob-indexer",
        dataSourceName: blobDataSource.Name,
        targetIndexName: indexName,
        fieldMappings: map,
        parameters: new IndexingParameters().ParseJson(),
        schedule: new IndexingSchedule(TimeSpan.FromDays(1)));

    // Reset the indexer if it already exists
    bool exists = await searchService.Indexers.ExistsAsync(blobIndexer.Name);
    if (exists)
    {
        await searchService.Indexers.ResetAsync(blobIndexer.Name);
    }
    await searchService.Indexers.CreateOrUpdateAsync(blobIndexer);
```

Adlı bir anahtar alan JSON bloblarını içeren **kimliği** yerine **HotelId**. Kod `FieldMapping` yönlendirmek için dizin oluşturucuyu bildirmek için sınıf **kimliği** alan için değer **HotelId** belge anahtarında dizini.

BLOB Depolama dizin oluşturucu, kullanılacak ayrıştırma modu tanımlayan parametrelerini kullanabilirsiniz. Ayrıştırma modu, tek bir belge ya da aynı blob içinde birden çok belge temsil bloblar için farklıdır. Kod için bu örnekte, her blob bir tek dizin belgeyi temsil eder. `IndexingParameters.ParseJson()` parametresi.

Dizin Oluşturucu parametreler için JSON BLOB'ları ayrıştırma hakkında daha fazla bilgi için bkz. [dizin JSON BLOB'ları](search-howto-index-json-blobs.md). .NET SDK kullanarak bu parametreleri belirtme hakkında daha fazla bilgi için bkz. [IndexerParametersExtension](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexingparametersextensions) sınıfı.

Bu örnek birden çok kez çalıştırmak istediğiniz durumda program yeni bir tane oluşturmadan önce aynı ada sahip mevcut tüm dizin oluşturucular siler.

Bu örnek dizin oluşturucu için bir zamanlama tanımlar günde bir kez çalışır. Gelecekte otomatik olarak yeniden çalıştırmak için dizin oluşturucuyu istemiyorsanız, bu çağrıdan schedule özelliği kaldırabilirsiniz.

### <a name="index-blob-data"></a>Dizin blob verileri

Blob Depolama veri kaynağı ve dizin oluşturucu oluşturulduktan sonra Dizin Oluşturucu çalışan kod basittir:

```csharp
    try
    {
        await searchService.Indexers.RunAsync(cosmosDbIndexer.Name);
    }
    catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
    {
        Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
    }
```

Dizin zaten Azure Cosmos DB veritabanındaki otel verilerle doldurulmuş olan olduğundan, blob dizin oluşturucu dizinde varolan belgeleri güncelleştirir ve odası ayrıntılarını ekler.

> [!NOTE]
> Aynı anahtar olmayan alanları hem veri kaynaklarınızı ve bu alanları içindeki verileri varsa hangi dizin oluşturucu son çoğu çalıştırılmadan gelen değerlerin dizinini içerecektir sonra eşleşmiyor. Bizim örneğimizde, hem veri kaynaklarını içeren bir **HotelName** alan. Bazı verileri nedenden dolayı bu alan için aynı anahtar değerine sahip belgelerde farklı ise, ardından **HotelName** dizini veri kaynağındaki en son olacak dizinde depolanan değeri.

## <a name="search-your-json-files"></a>JSON dosyalarınızı arama

Programını çalıştırdıktan sonra doldurulmuş arama dizinini kullanarak keşfedebilirsiniz [ **arama Gezgini** ](search-explorer.md) portalında.

Azure portalında arama hizmeti **genel bakış** sayfasında ve bulma **otelinden oda örnek** içinde dizin **dizinleri** listesi.

  ![Azure Search dizinlerini listesi](media/tutorial-multiple-data-sources/index-list.png "liste, Azure Search dizinlerini")

Listenin otelinden oda örnek dizinde tıklayın. Dizini için bir arama Gezgini arabirimi görürsünüz. Terim "Lüks" gibi bir sorgu girin. En az bir belgede sonuçları görmeniz gerekir ve bu belgede yer nesnelerin bir listesini, odaları dizisinde göstermesi gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir öğretici tamamlandıktan sonra temizlemenin en hızlı yolu, Azure Search hizmetini içeren kaynak grubunu silmektir. Kaynak grubunu silerek içindeki her şeyi kalıcı olarak silebilirsiniz. Portalda Azure Search hizmeti genel bakış sayfasında kaynak grubu adı var.

## <a name="next-steps"></a>Sonraki adımlar

Çeşitli yaklaşımlar ve JSON bloblarını dizine ekleme için birden çok seçenek vardır. Veri kaynağınızı JSON içeriği içeriyorsa, hangi senaryonuz için en iyi çalıştığını görmek için bu seçenekleri gözden geçirebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Search Blob Dizin Oluşturucu kullanarak JSON bloblarını dizinleme](search-howto-index-json-blobs.md)

Yapılandırılmamış BLOB'ları veya tam metin içeriği oynatabilir zenginleştirilmiş verilerle bir veri kaynağından yapılandırılmış dizin verileri genişletmek isteyebilirsiniz. Aşağıdaki öğreticiye, .NET SDK kullanarak Azure Search ile birlikte Bilişsel Hizmetler'in nasıl kullanılacağını gösterir.

> [!div class="nextstepaction"]
> [Bir Azure Search dizini oluşturma ardışık düzen Bilişsel hizmetler API'lerini çağırma](cognitive-search-tutorial-blob-dotnet.md)
