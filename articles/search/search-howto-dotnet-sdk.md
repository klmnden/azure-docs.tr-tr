---
title: "Azure Search .NET uygulamasından kullanma | Microsoft Docs"
description: "Azure Search bir .NET uygulamasında kullanma"
services: search
documentationcenter: 
author: brjohnstmsft
manager: jlembicz
editor: 
ms.assetid: 93653341-c05f-4cfd-be45-bb877f964fcb
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/22/2017
ms.author: brjohnst
ms.openlocfilehash: 552a7ab193e12d2e72da494166d774e974c85d47
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-azure-search-from-a-net-application"></a>Azure Search bir .NET uygulamasında kullanma
Bu makale ile çalışır almak için bir kılavuz olan [Azure Search .NET SDK'sı](https://aka.ms/search-sdk). .NET SDK kullanarak Azure Search uygulamanızda zengin arama deneyimi uygulamak için kullanabilirsiniz.

## <a name="whats-in-the-azure-search-sdk"></a>Azure nedir arama SDK
Bir istemci Kitaplığı'nın SDK oluşur `Microsoft.Azure.Search`. Dizinleri, veri kaynakları ve dizin oluşturucular, yönetmek yanı sıra karşıya yükleme ve belgeleri yönetmek ve tüm HTTP ve JSON ayrıntıları ile mücadele etmek zorunda kalmadan sorgularını yürütmek sağlar.

İstemci Kitaplığı gibi sınıflarını tanımlayan `Index`, `Field`, ve `Document`, yanı sıra operations ister `Indexes.Create` ve `Documents.Search` üzerinde `SearchServiceClient` ve `SearchIndexClient` sınıfları. Bu sınıfların şu ad alanlarından düzenlenmiştir:

* [Microsoft.Azure.Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft.Azure.Search.Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

Geçerli sürümü Azure Search .NET SDK'sı, genellikle kullanıma sunulmuştur. Ziyaret için bir sonraki sürümde birleştirmek için bize geri bildirim sağlamak istiyorsanız, lütfen bizim [görüş sayfası](https://feedback.azure.com/forums/263029-azure-search/).

.NET SDK'sı sürüm destekler `2016-09-01` , [Azure Search REST API'sini](https://docs.microsoft.com/rest/api/searchservice/). Bu sürümü artık özel Çözümleyicileri ve Azure Blob ve Azure Table dizin oluşturucu desteği içerir. Önizleme özellikleri *değil* JSON ve CSV dosyalarını dizin oluşturma için destek gibi bu sürümü bir parçası olan [Önizleme](search-api-2015-02-28-preview.md) ve eski aracılığıyla kullanılabilen [.NET SDK'sı2.0-Önizlemesürümü](https://aka.ms/search-sdk-preview).

Bu SDK desteklemediği [yönetim işlemlerini](https://docs.microsoft.com/rest/api/searchmanagement/) oluşturma ve arama hizmetleri ölçekleme ve API anahtarlarını yönetme gibi. Bir .NET uygulamasından arama kaynaklarınızı yönetmek gerekiyorsa, kullanabileceğiniz [Azure Search .NET Yönetim SDK](https://aka.ms/search-mgmt-sdk).

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>SDK'ın en son sürüme yükseltme
Azure Search .NET SDK'sı daha eski bir sürümü zaten kullanmakta olduğunuz ve genel olarak kullanılabilir yeni sürüme yükseltmek istiyorsanız [bu makalede](search-dotnet-sdk-migration.md) açıklar nasıl.

## <a name="requirements-for-the-sdk"></a>SDK'sı gereksinimleri
1. Visual Studio 2017.
2. Kendi Azure Search hizmeti. SDK'yı kullanmak için adını hizmetiniz ve bir veya daha fazla API anahtarı gerekir. [Portalda bir hizmet oluşturma](search-create-service-portal.md) bu adımları uygularken size yardımcı olur.
3. Azure Search .NET SDK'sını indirin [NuGet paketi](http://www.nuget.org/packages/Microsoft.Azure.Search) "NuGet paketlerini Yönet" Visual Studio kullanarak. Paket adı için yalnızca arama `Microsoft.Azure.Search` NuGet.org üzerinde.

Azure Search .NET SDK'sı .NET Framework 4.6 ve .NET Core hedefleme uygulamaları destekler.

## <a name="core-scenarios"></a>Temel senaryolar
Arama uygulamanızda yapmanız gerekir birkaç şey vardır. Bu öğreticide, bu temel senaryolar şu konulara değineceğiz:

* Dizin oluşturma
* Belgelerle dizin doldurma
* Tam metin araması ve filtreleri kullanarak belgeleri için arama

Aşağıdaki örnek kod her birinde göstermektedir. Kod parçacıkları, kendi uygulamanızda kullanmak üzere çekinmeyin.

### <a name="overview"></a>Genel Bakış
Biz keşfetme örnek uygulamayı yeni bir oluşturur "hotels" adlı dizin birkaç belgelerle doldurur sonra bazı arama sorguları yürütür. Genel akış gösteren programın ana şöyledir:

```csharp
// This sample shows how to delete, create, upload documents and query an index
static void Main(string[] args)
{
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();

    SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

    Console.WriteLine("{0}", "Deleting index...\n");
    DeleteHotelsIndexIfExists(serviceClient);

    Console.WriteLine("{0}", "Creating index...\n");
    CreateHotelsIndex(serviceClient);

    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(indexClient);

    ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

    RunQueries(indexClientForQueries);

    Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
    Console.ReadKey();
}
```

> [!NOTE]
> Bu kılavuzda kullanılan örnek uygulamanın tam kaynak kodunu [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) üzerinde bulabilirsiniz.
> 
>

Biz bu adım adım adım geçireceğiz. Önce yeni bir oluşturmak ihtiyacımız `SearchServiceClient`. Bu nesne, dizinleri yönetmenize olanak sağlar. Bir oluşturmak için bir yönetici API anahtarı yanı sıra Azure Search hizmet adı sağlamanız gerekir. Bu bilgileri girebilirsiniz `appsettings.json` dosyasının [örnek uygulama](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

> [!NOTE]
> (Burada bir yönetici anahtarı gerekli Örneğin, bir sorgu anahtarı), yanlış bir tuşa sağlarsanız, `SearchServiceClient` özel durum oluşturacak bir `CloudException` şu hata ile bir işlemi yöntemi bunun üzerinde gibi ilk kez çağırdığınızda "Yasak" iletisi `Indexes.Create`. Bu, olanak olursa, bizim API anahtarı denetleyin.
> 
> 

Sonraki birkaç satır "zaten varsa, ilk silme hotels" adlı bir dizin oluşturma yöntemlerini çağırın. Biz bu yöntemlerle biraz daha sonra yol gösterir.

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

Ardından, dizin doldurulması gerekir. Bunu yapmak için ihtiyacımız bir `SearchIndexClient`. Abonelik sahibi olmak için iki yolu vardır: onu oluşturmak ya da çağırma `Indexes.GetClient` üzerinde `SearchServiceClient`. İkinci kolaylık sağlamak için kullanırız.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> Genel bir arama uygulamasında, dizin yönetimi ve popülasyon, arama sorgularından ayrı bir bileşen tarafından işlenir. `Indexes.GetClient`, başka bir `SearchCredentials` sağlamanızı gerektirmediğinden, dizini doldurmak için uygundur. Yeni `SearchIndexClient` için `SearchServiceClient` oluşturmak üzere kullandığınız yönetici anahtarını geçirerek bunu yapar. Ancak uygulamanızın sorguları yürüten bölümünde, bir yönetici anahtarı yerine sorgu anahtarında geçirebilmeniz için doğrudan `SearchIndexClient` oluşturmak daha iyi olur. Bu, en az ayrıcalık prensibi tutarlıdır ve uygulamanızı daha güvenli hale getirmenize yardımcı olur. Yönetici anahtarları ve sorgu anahtarları hakkında daha fazla bilgi için [burada](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).
> 
> 

Biz sahip olduğunuza göre bir `SearchIndexClient`, biz dizin doldurabilirsiniz. Bu size daha sonra size yol gösterir başka bir yöntem kullanarak gerçekleştirilir.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

Son olarak, biz birkaç arama sorguları çalıştırmak ve sonuçları görüntüleyin. Bu süre kullanırız farklı bir `SearchIndexClient`:

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

Biz yakından sürer `RunQueries` daha sonra yöntemi. Yeni oluşturmak için kodu işte `SearchIndexClient`:

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

Bu süre dizine yazma erişimi gerekmez bu yana bir sorgu anahtarı kullanırız. Bu bilgileri girebilirsiniz `appsettings.json` dosyasının [örnek uygulama](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

Bu uygulamayı geçerli bir hizmet adı ve API anahtarlarını çalıştırırsanız, çıktı aşağıdaki gibi görünmelidir:

    Deleting index...
    
    Creating index...
    
    Uploading documents...
    
    Waiting for documents to be indexed...
    
    Search the entire index for the term 'budget' and return only the hotelName field:
    
    Name: Roach Motel
    
    Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:
    
    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river
    
    Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:
    
    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00
    
    Search the entire index for the term 'motel':
    
    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
    
    Complete.  Press any key to end application...

Uygulamanın tam kaynak kodu, bu makalenin sonundaki sağlanır.

Ardından, biz tarafından çağrılır yöntemlerin her biri daha ayrıntılı bir bakış sürer `Main`.

### <a name="creating-an-index"></a>Dizin oluşturma
Oluşturduktan sonra bir `SearchServiceClient`, sonraki şey `Main` mu zaten varsa "hotels" dizininin olduğunda. Aşağıdaki yöntemi tarafından gerçekleştirilir:

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
    }
}
```

Bu yöntem verilen `SearchServiceClient` dizin varsa ve bu durumda denetlemek için onu silin.

> [!NOTE]
> Bu makaledeki örnek kod, kolaylık amacıyla Azure Search .NET SDK'sının zaman uyumlu yöntemlerini kullanır. Kendi uygulamalarınızda zaman uyumsuz yöntemler kullanarak bunları ölçeklenebilir ve esnek tutmanızı öneririz. Örneğin, yukarıdaki yöntemi kullanabilirsiniz `ExistsAsync` ve `DeleteAsync` yerine `Exists` ve `Delete`.
> 
> 

Ardından, `Main` bu yöntemini çağırarak yeni bir "hotels" dizini oluşturur:

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels",
        Fields = FieldBuilder.BuildForType<Hotel>()
    };

    serviceClient.Indexes.Create(definition);
}
```

Bu yöntem yeni bir oluşturur `Index` listesini nesnesiyle `Field` yeni bir dizin şemasını tanımlayan nesneleri. Her alanın bir adı, veri türü ve arama davranışını tanımlamak birkaç öznitelik vardır. `FieldBuilder` Sınıfı listesini oluşturmak için yansıma kullanır `Field` nesneler genel özellikleri ve özniteliklerini inceleyerek tarafından dizini için belirtilen `Hotel` model sınıfı. Biz yakından gitmek `Hotel` daha sonra sınıfı.

> [!NOTE]
> Her zaman listesi oluşturabilirsiniz `Field` kullanmak yerine doğrudan nesneleri `FieldBuilder` gerekiyorsa. Örneğin, bir model sınıfı kullanmak istemeyebilirsiniz veya öznitelikleri ekleyerek değiştirmeye istemediğiniz varolan bir model sınıfı kullanmanız gerekebilir.
>
> 

Alanlara ek olarak (bunlar kısaltma örnekten göz ardı edilir) dizine ayrıca Puanlama profilleri, ilgili veya CORS seçenekleri ekleyebilirsiniz. Dizin nesnesi ve onun bağlı bölümlerinde hakkında daha fazla bilgi bulabilirsiniz [SDK başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index), ın yanı [Azure Search REST API Başvurusu](https://docs.microsoft.com/rest/api/searchservice/).

### <a name="populating-the-index"></a>Dizin doldurma
Sonraki adım `Main` yeni oluşturulan dizinini doldurmak için. Bu aşağıdaki yönteminde gerçekleştirilir:

```csharp
private static void UploadDocuments(ISearchIndexClient indexClient)
{
    var hotels = new Hotel[]
    {
        new Hotel()
        { 
            HotelId = "1", 
            BaseRate = 199.0, 
            Description = "Best hotel in town",
            DescriptionFr = "Meilleur hôtel en ville",
            HotelName = "Fancy Stay",
            Category = "Luxury", 
            Tags = new[] { "pool", "view", "wifi", "concierge" },
            ParkingIncluded = false, 
            SmokingAllowed = false,
            LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero), 
            Rating = 5, 
            Location = GeographyPoint.Create(47.678581, -122.131577)
        },
        new Hotel()
        { 
            HotelId = "2", 
            BaseRate = 79.99,
            Description = "Cheapest hotel in town",
            DescriptionFr = "Hôtel le moins cher en ville",
            HotelName = "Roach Motel",
            Category = "Budget",
            Tags = new[] { "motel", "budget" },
            ParkingIncluded = true,
            SmokingAllowed = true,
            LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
            Rating = 1,
            Location = GeographyPoint.Create(49.678581, -122.131577)
        },
        new Hotel() 
        { 
            HotelId = "3", 
            BaseRate = 129.99,
            Description = "Close to town hall and the river"
        }
    };

    var batch = IndexBatch.Upload(hotels);

    try
    {
        indexClient.Documents.Index(batch);
    }
    catch (IndexBatchException e)
    {
        // Sometimes when your Search service is under load, indexing will fail for some of the documents in
        // the batch. Depending on your application, you can take compensating actions like delaying and
        // retrying. For this simple demo, we just log the failed document keys and continue.
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

    Console.WriteLine("Waiting for documents to be indexed...\n");
    Thread.Sleep(2000);
}
```

Bu yöntem, dört bölümden oluşur. Bir dizi ilk oluşturur `Hotel` dizine karşıya yüklemek için giriş veri işlevi görecek nesneleri. Bu veriler, kolaylık sağlamak için sabit kodlanmış ' dir. Kendi uygulamanızda verilerinizi büyük olasılıkla bir SQL veritabanı gibi bir dış veri kaynağından gelecektir.

İkinci bölümü oluşturur bir `IndexBatch` belgeleri içeren. Oluşturduğunuz, bu durumda çağırarak zaman toplu uygulamak istediğiniz işlem belirtin `IndexBatch.Upload`. Toplu sonra Azure Search dizini tarafından yüklenen `Documents.Index` yöntemi.

> [!NOTE]
> Bu örnekte, biz yalnızca belgeleri karşıya yükleniyor. Varolan belgelere değişiklikleri birleştirmek veya belgeleri silmek istiyorsanız, çağırarak toplu işlemler oluşturabilirsiniz `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, veya `IndexBatch.Delete` yerine. Çağırarak farklı işlemlerini tek bir toplu işte karıştırabilirsiniz `IndexBatch.New`, bir koleksiyonunu alır `IndexAction` nesneleri, her biri bir belge üzerinde belirli bir işlemi gerçekleştirmek için Azure Search söyler. Her oluşturabilirsiniz `IndexAction` gibi karşılık gelen yöntemini çağırarak kendi işlemi ile `IndexAction.Merge`, `IndexAction.Upload`ve benzeri.
> 
> 

Üçüncü bu yöntem dizin oluşturma için önemli bir hata durumunu işler bir catch bloğunun parçasıdır. Azure Search hizmetiniz toplu işlemdeki belgelerin bazılarına dizin oluşturmada başarısız olursa `Documents.Index` tarafından bir `IndexBatchException` oluşturulur. Bu durum, hizmetiniz ağır yük altındayken belgelere dizin oluşturuyorsanız oluşabilir. **Bu durumu, kodunuzda açık şekilde işlemenizi kesinlikle öneririz.** Başarısız olan belgelere dizin oluşturmayı geciktirip sonra yeniden deneyebilir veya günlük tutup örneğin devam ettiği şekilde devam edebilir veya uygulamanızın veri tutarlılığı gereksinimlerine bağlı olarak başka bir şey yapabilirsiniz.

> [!NOTE]
> Kullanabileceğiniz `FindFailedActionsToRetry` yöntemi yalnızca bir önceki çağrılamadı eylemleri içeren yeni bir toplu iş oluşturmak için `Index`. Yöntem belgelenen [burada](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) ve düzgün bir şekilde kullanma konusunda bir tartışma [StackOverflow üzerinde](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).
>
>

Son olarak, `UploadDocuments` yöntemi gecikmeler iki saniye. Azure Search hizmetinizde dizin oluşturma uyumsuz şekilde meydana gelir; bu nedenle belgelerin aramada kullanılabilir olduğundan emin olmak için örnek uygulamanızın kısa bir süre beklemesi gerekir. Bu gibi gecikmeler genellikle yalnızca gösterilerde, testlerde ve örnek uygulamalarda gereklidir.

#### <a name="how-the-net-sdk-handles-documents"></a>.NET SDK belgeleri nasıl işler?
Azure Search .NET SDK'sının `Hotel` gibi kullanıcı tanımlı bir sınıfın örneklerini dizine nasıl yükleyebildiğini merak ediyor olabilirsiniz. Bu sorunun yanıtlanmasına yardımcı olmak için bakalım `Hotel` sınıfı:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// The SerializePropertyNamesAsCamelCase attribute is defined in the Azure Search .NET SDK.
// It ensures that Pascal-case property names in the model class are mapped to camel-case
// field names in the index.
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }

    [IsSearchable]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    [IsFilterable, IsFacetable]
    public bool? SmokingAllowed { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public int? Rating { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }
}
```

Fark edilecek ilk şey, `Hotel` öğesinin her bir genel özelliğinin dizin tanımındaki bir alana karşılık geldiği ancak çok önemli bir fark olduğudur: `Hotel` öğesinin her bir genel özelliğinin adı büyük harfle ("Pascal büyük/küçük harfi") başlarken her bir alanın adı küçük harfle ("ortası büyük harf") başlar. Bu durum, hedef şemanın uygulama geliştiricisinin denetimi dışında kaldığı bir veri bağlamayı gerçekleştiren .NET uygulamalarında ortak bir senaryodur. Özellik adlarını ortası büyük harf yaparak .NET adlandırma yönergelerini bozmanın yerine, `[SerializePropertyNamesAsCamelCase]` özniteliğiyle SDK'nın özellik adlarını otomatik olarak ortası büyük harfle eşlenmesini söyleyebilirsiniz.

> [!NOTE]
> Azure Search .NET SDK'sı, özel model nesnelerinizi JSON'a ve JSON'dan seri hale getirmek ve seri durumdan çıkarmak için [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) kitaplığını kullanır. Gerekirse bu seri hale getirmeyi özelleştirebilirsiniz. Daha fazla ayrıntı için bkz: [JSON.NET ile özel serileştirme](#JsonDotNet).
> 
> 

Fark edilecek ikinci şeydir öznitelikleri gibi `IsFilterable`, `IsSearchable`, `Key`, ve `Analyzer` her ortak özelliği tasarlamanız. Bu öznitelikler doğrudan eşleme [Azure Search dizini karşılık gelen özniteliklerini](https://docs.microsoft.com/rest/api/searchservice/create-index#request). `FieldBuilder` Sınıfı dizini için alan tanımlarını oluşturmak için bunları kullanır.

Üçüncü önemli şey hakkında `Hotel` sınıfı genel özelliklerin veri türleridir. Bu özelliklerin .NET türleri dizin tanımında eşdeğer alan türleriyle eşlenir. Örneğin, `Category` dize özelliği `Edm.String` türündeki `category` alanına eşlenir. `bool?` ve `Edm.Boolean`, `DateTimeOffset?` ve `Edm.DateTimeOffset`, vb. arasında benzer türde eşlemeler bulunur. Tür eşlemesine yönelik belirli kurallar, [Azure Search .NET SDK başvurusundaki](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_) `Documents.Get` yönteminde belirtilmiştir. `FieldBuilder` Sınıfı, bu eşlemeyi mvc'deki ancak hala serileştirme sorunları gidermek gerektiğinde anlamak yararlı olabilir.

Kendi sınıflarınızı belge olarak kullanabilme iki yönde de işe yarar; Ayrıca, arama sonuçları almak ve biz sonraki bölümde göreceğiniz gibi otomatik olarak onları tercih ettiğiniz bir türe seri durumdan SDK'sına sahip.

> [!NOTE]
> Azure Search .NET SDK'sı, alan adlarını alan değerlerine bir anahtar/değer eşlemesi olan `Document` sınıfını kullanarak dinamik tür belirtilmiş belgeleri de destekler. Bu durum, tasarım sırasında dizin şemasını bilmediğiniz veya belirli model sınıflarına bağlamanın kullanışlı olmayacağı senaryolarda kullanışlıdır. SDK'da belgelerle ilgili tüm yöntemler, `Document` sınıfıyla çalışan aşırı yüklerin yanı sıra genel türde bir parametre alan kesin tür belirtilmiş aşırı yüklere de sahiptir. Örnek kodda yalnızca ikinci Bu öğreticide kullanılır. `Document` Sınıfının devraldığı `Dictionary<string, object>`. Diğer ayrıntıları bulmak [burada](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).
> 
> 

**Neden boş değer atanabilir türleri kullanmalısınız?**

Bir Azure Search dizinine eşlemek için yeni model sınıflarınızı tasarlarken, `bool` ve `int` gibi değer türü özelliklerinin boş değer atanabilir (örneğin, `bool` yerine `bool?`) şeklinde bildirilmesini öneririz. Boş değer atanamayan bir özellik kullanırsanız buna karşılık gelen alan için dizininizdeki hiçbir belgenin boş bir değer içermediğini **garanti etmeniz** gerekir. Bunu zorlamanıza ne SDK ne de Azure Search hizmeti yardımcı olur.

Bu yalnızca kuramsal bir sorun değildir: Var olan `Edm.Int32` türünde bir dizine yeni bir alan eklediğiniz bir senaryoyu düşünün. Dizin tanımını güncelleştirdikten sonra, tüm belgelerin bu yeni alan için boş bir değeri olur (bunun nedeni, Azure Search'te tüm türlerin boş değer atanabilir olmasıdır). Ardından bu alan için boş değer atanamayan bir `int` özelliğiyle bir model sınıfı kullanırsanız belgeleri almaya çalışırken bunun gibi bir `JsonSerializationException` alırsınız:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Bu nedenle, en iyi uygulama olarak model sınıflarınızda boş değer atanabilir türler kullanmanızı öneririz.

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a>JSON.NET ile özel seri hale getirme
SDK'sı, seri hale getirme ve belgeleri seri durumdan çıkarmak için JSON.NET kullanır. Seri hale getirme özelleştirebilir ve tanımlayarak, kendi gerekirse seri durumundan çıkarma `JsonConverter` veya `IContractResolver` (bkz [JSON.NET belgelerine](http://www.newtonsoft.com/json/help/html/Introduction.htm) daha fazla ayrıntı için). Uygulamanız Azure Search ve diğer daha Gelişmiş senaryolar ile kullanmak için mevcut bir model sınıfın uyum istediğinizde bu yararlı olabilir. Örneğin, özel serileştirme ile şunları yapabilirsiniz:

* İçerebilir veya belge alanlar olarak depolanan belirli özellikleri modeli sınıfınızın dışlayabilirsiniz.
* Kodunuzda özellik adları ve alan adları, dizininizdeki arasında eşleyin.
* Belge alanları özellikleri eşlemek için kullanılan özel öznitelikler oluşturun.

Github'daki Azure Search .NET SDK'sı için birim testleri özel serileştirme uygulamanın örnekleri bulabilirsiniz. İyi bir başlangıç noktasıdır [bu klasör](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models). Özel serileştirme testleri tarafından kullanılan sınıfları içerir.

### <a name="searching-for-documents-in-the-index"></a>Dizin belgelerde arama
Son örnek uygulama dizinindeki bazı belgeleri aramak için adımdır. Aşağıdaki yöntem bunu yapar:

```csharp
private static void RunQueries(ISearchIndexClient indexClient)
{
    SearchParameters parameters;
    DocumentSearchResult<Hotel> results;

    Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

    parameters =
        new SearchParameters()
        {
            Select = new[] { "hotelName" }
        };

    results = indexClient.Documents.Search<Hotel>("budget", parameters);

    WriteDocuments(results);

    Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
    Console.WriteLine("and return the hotelId and description:\n");

    parameters =
        new SearchParameters()
        {
            Filter = "baseRate lt 150",
            Select = new[] { "hotelId", "description" }
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
    Console.Write("in descending order, take the top two results, and show only hotelName and ");
    Console.WriteLine("lastRenovationDate:\n");

    parameters =
        new SearchParameters()
        {
            OrderBy = new[] { "lastRenovationDate desc" },
            Select = new[] { "hotelName", "lastRenovationDate" },
            Top = 2
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.WriteLine("Search the entire index for the term 'motel':\n");

    parameters = new SearchParameters();
    results = indexClient.Documents.Search<Hotel>("motel", parameters);

    WriteDocuments(results);
}
```

Her bir sorgu yürütülmeden, bu yöntem önce yeni bir oluşturur `SearchParameters` nesnesi. Bu, sıralama, filtreleme, disk belleği ve olduğunu gibi bir sorgu için ek seçenekleri belirtmek için kullanılır. Bu yöntemde, biz ayarladığınız `Filter`, `Select`, `OrderBy`, ve `Top` özelliği için farklı sorgular. Tüm `SearchParameters` özellikleri belgelenmiştir [burada](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).

Gerçekte arama sorguyu yürütmek için sonraki adımdır bakın. Bu yapılır kullanarak `Documents.Search` yöntemi. Her sorgu için size bir dize olarak kullanmak için arama metni geçirin (veya `"*"` arama metni ise), daha önce oluşturduğunuz arama parametrelerini artı. Biz de belirtmeniz `Hotel` türü parametre olarak `Documents.Search`, SDK'sını belgeleri arama sonuçlarında türündeki nesneleri seri durumdan söyler `Hotel`.

> [!NOTE]
> Arama sorgu ifade sözdizimi hakkında daha fazla bilgi bulabilirsiniz [burada](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).
> 
> 

Son olarak, her sorgu sonra bu yöntem her belge konsola yazdırma tüm eşleşmeleri arama sonuçlarında dolaşır:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Buna karşılık her sorguları daha yakın bir göz atalım. İlk sorguyu yürütmek için kod aşağıdaki gibidir:

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

Bu durumda, "Bütçe" sözcüğü Eşleştir Oteller için arama yapıyorsanız ve yalnızca otel adlar, belirtilen şekilde geri dönmek istiyoruz `Select` parametresi. Sonuçları şunlardır:

    Name: Roach Motel

Ardından, değerinden 150 ABD dolarını gecelik oranı ile Oteller bulmak ve yalnızca otel kimliği ve açıklama dönmek istiyoruz:

```csharp
parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

Bu sorgu bir OData kullanan `$filter` ifadesi, `baseRate lt 150`, dizin belgelerde filtrelemek için. Azure Search destekleyen OData söz dizimi hakkında daha fazla bilgi bulabilirsiniz [burada](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).

Sorgu sonuçlarını şunlardır:

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river

Ardından, en son renovated üst iki Oteller bulmak ve Otel adı ve son Yenileme tarihi göstermek istiyoruz. Kod aşağıdaki gibidir: 

```csharp
parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

Bu durumda, yeniden OData sözdizimi belirtmek için kullanıyoruz `OrderBy` parametre olarak `lastRenovationDate desc`. Ayrıca `Top` biz yalnızca get üstteki iki belgeleri emin olmak için 2. Önce ayarlar gibi `Select` hangi alanların döndürülmesi gerektiğini belirtmek için.

Sonuçları şunlardır:

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Son olarak, biz "motel" word eşleşen tüm Oteller bulmak istediğiniz:

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

Ve biz belirtmemek olduğundan, tüm alanları içeren aşağıdaki sonuçları `Select` özelliği:

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

Öğreticinin Bu adım tamamlandıktan, ancak burada Durdur yok. **Sonraki adımlar** Azure Search hakkında daha fazla bilgi için ek kaynaklar sağlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) ve [REST API](https://docs.microsoft.com/rest/api/searchservice/) başvurularına göz atın.
* Bilginiz aracılığıyla deepen [videolar ve diğer örnekler ve öğreticiler](search-video-demo-tutorial-list.md).
* Gözden geçirme [adlandırma kuralları](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) çeşitli nesneleri adlandırma kurallarını öğrenin.
* Gözden geçirme [desteklenen veri türleri](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) Azure Search'te.
