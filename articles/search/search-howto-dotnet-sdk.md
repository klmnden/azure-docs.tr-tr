---
title: -Azure Search bir .NET uygulamasından Azure Search kullanma
description: Bir .NET uygulaması kullanarak Azure Search kullanma hakkında bilgi edinin C# ve .NET SDK'sı. Kod tabanlı görevler dizin içeriği hizmete bağlanın ve dizin sorgulama.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: brjohnst
ms.openlocfilehash: 9f0af40d442747181636b50612f7d2162ead6a86
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450007"
---
# <a name="how-to-use-azure-search-from-a-net-application"></a>Bir .NET uygulamasından Azure Search kullanma

Bu makale ile çalışmaya başlamanızı sağlayacak bir kılavuz niteliğindedir [Azure Search .NET SDK'sı](https://aka.ms/search-sdk). .NET SDK'sı, Azure Search kullanarak uygulamanızda bir zengin arama deneyimi uygulamak için kullanabilirsiniz.

## <a name="whats-in-the-azure-search-sdk"></a>Azure nedir arama SDK'sı
SDK'sı dizinlerinizi yönetmenizi sağlayan birkaç istemci kitaplıkları oluşuyorsa, veri kaynakları, dizin oluşturucular ve eş anlamlı, karşıya yükleme yanı sıra eşler ve belgeleri yönetmek ve tüm HTTP ve JSON ayrıntılarını ile uğraşmak zorunda kalmadan sorgular yürütün. Bu istemci kitaplıklarının tümü NuGet paketleri olarak dağıtılır.

Ana NuGet paketi `Microsoft.Azure.Search`, bağımlılık olarak diğer tüm paketleri içeren bir meta-package olduğu. Bu paket, yeni başlıyorsanız veya uygulamanıza Azure Search'ün tüm özellikleri gerekir biliyorsanız kullanın.

SDK diğer NuGet paketleri şunlardır:
 
  - `Microsoft.Azure.Search.Data`: Azure Search kullanarak bir .NET uygulaması geliştirmeye devam ediyoruz ve sorgu veya dizinleri belgeleri güncelleştirmek yalnızca ihtiyacınız varsa bu paketi kullanın. Ayrıca oluşturmak veya dizinleri güncelleştirme gerekirse eş anlamlı sözcük eşlemelerini veya diğer hizmet düzeyi kaynakları kullanmak `Microsoft.Azure.Search` bunun yerine paket.
  - `Microsoft.Azure.Search.Service`: Azure Search dizinlerini, eş anlamlı sözcük eşlemelerini, dizin oluşturucular, veri kaynakları veya diğer hizmet düzeyi kaynakları yönetmek için. NET'te Otomasyon geliştiriyorsanız, bu paketi kullanın. Dizinlerinizi içinde sorgu veya güncelleştirme belgelere yalnızca ihtiyacınız varsa `Microsoft.Azure.Search.Data` bunun yerine paket. Azure Search'ün tüm işlevlerine ihtiyacınız varsa, `Microsoft.Azure.Search` bunun yerine paket.
  - `Microsoft.Azure.Search.Common`: Azure Search .NET kitaplıkları tarafından gereken genel türler. Bu paket, uygulamanızda doğrudan kullanmak gerekmez. Yalnızca, bir bağımlılık olarak kullanılmak üzere tasarlanmıştır.

Sınıflar gibi çeşitli istemci kitaplıkları tanımlamak `Index`, `Field`, ve `Document`, yanı sıra operations ister `Indexes.Create` ve `Documents.Search` üzerinde `SearchServiceClient` ve `SearchIndexClient` sınıfları. Bu sınıfların şu ad alanlarından düzenlenmiştir:

* [Microsoft.Azure.Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft.Azure.Search.Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

SDK'ın gelecekteki bir güncelleştirmesi geribildirim sağlamak istiyorsanız, bkz. bizim [geri bildirim sayfası](https://feedback.azure.com/forums/263029-azure-search/) veya bir sorun oluşturun [GitHub](https://github.com/azure/azure-sdk-for-net/issues) ve "Azure Search" sorun başlığında bahsedebilirsiniz.

.NET SDK'sı sürümünü destekleyen `2019-05-06` , [Azure Search REST API'sine](https://docs.microsoft.com/rest/api/searchservice/). Bu sürümü için destek içerir [karmaşık türler](search-howto-complex-data-types.md), [bilişsel arama](cognitive-search-concept-intro.md), [otomatik tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete), ve [modu ayrıştırılırken JsonLines](search-howto-index-json-blobs.md) olduğunda Azure Bloblarını dizine ekleme. 

Bu SDK'sı tarafından desteklenmeyen [yönetim işlemlerini](https://docs.microsoft.com/rest/api/searchmanagement/) oluşturma ve arama hizmetleri ölçeklendirme ve API anahtarlarını yönetme gibi. Bir .NET uygulamasından arama kaynaklarınızı yönetmek ihtiyacınız varsa, kullanabileceğiniz [Azure Search .NET Yönetim SDK'sı](https://aka.ms/search-mgmt-sdk).

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>SDK'sının en son sürüme yükseltme
Zaten Azure Search .NET SDK'sı daha eski bir sürümünü kullanıyorsanız ve en son sürüme yükseltmek istiyorsanız genel kullanıma sunulan sürümü [bu makalede](search-dotnet-sdk-migration-version-9.md) açıklar nasıl.

## <a name="requirements-for-the-sdk"></a>SDK'sı gereksinimleri
1. Visual Studio 2017 veya üstü.
2. Kendi Azure Search hizmeti. SDK'yı kullanmak için hizmetinizin API anahtarlarını bir veya daha fazla ve adı gerekir. [Portalda hizmet oluşturma](search-create-service-portal.md) Bu adımlarda yardımcı olur.
3. Azure Search .NET SDK'sını indirin [NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Search) "NuGet paketlerini Yönet" Visual Studio kullanarak. Paket adı için yalnızca arama `Microsoft.Azure.Search` üzerinde NuGet.org (veya biri diğer işlevlerinin bir alt kümesini yalnızca gerekiyorsa Yukarıdaki adları paketi).

Azure Search .NET SDK'sı, .NET Framework 4.5.2'yi hedefleyen uygulamalar destekler ve daha iyi olarak .NET Core 2.0 ve üzeri.

## <a name="core-scenarios"></a>Temel senaryoları
Arama uygulamanız yapmanız gerekir birkaç şey vardır. Bu öğreticide, bu temel senaryolar şu konulara değineceğiz:

* Dizin oluşturma
* Belgelerle dizini doldurma
* Belgeler tam metin arama ve filtreler kullanılarak aranıyor

Aşağıdaki örnek kod, bu senaryoların her biri gösterilmektedir. Kod parçacıkları, kendi uygulamanızda kullanmaktan çekinmeyin.

### <a name="overview"></a>Genel Bakış
Biz keşfetmeye örnek uygulamayı yeni bir oluşturur "hotels" adlı dizin birkaç belge ile doldurur ve ardından bazı arama sorguları yürütür. Genel akış gösteren eden ana program şöyledir:

```csharp
// This sample shows how to delete, create, upload documents and query an index
static void Main(string[] args)
{
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();

    SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

    string indexName = configuration["SearchIndexName"];

    Console.WriteLine("{0}", "Deleting index...\n");
    DeleteIndexIfExists(indexName, serviceClient);

    Console.WriteLine("{0}", "Creating index...\n");
    CreateIndex(indexName, serviceClient);

    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);

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

Bu bir adım adım alacağız. Önce yeni bir ihtiyacımız `SearchServiceClient`. Bu nesne, dizin yönetmenizi sağlar. Bir oluşturmak için bir yönetici API anahtarını yanı sıra, Azure arama hizmeti adınızı sağlamak gerekir. Bu bilgileri girebilirsiniz `appsettings.json` dosya [örnek uygulama](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

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
> Yanlış bir anahtar (burada bir yönetici anahtarı gerekli gibi bir sorgu anahtarı), sağlarsanız `SearchServiceClient` oluşturmaz bir `CloudException` şu hata ile çağrı işlemi yöntemi, aşağıdaki gibi ilk kez "Yasak" iletisi `Indexes.Create`. Böyle bir durumla, bizim API anahtarı denetleyin.
> 
> 

Sonraki birkaç satırı "hotels" zaten varsa önce silinmesi, adlı bir dizin oluşturmak için yöntemleri çağırın. Biz bu yöntemler arasında biraz daha sonra size yol gösterir.

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteIndexIfExists(indexName, serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateIndex(indexName, serviceClient);
```

Sonra dizininin doldurulması gerekir. Dizinini doldurmak için ihtiyacımız bir `SearchIndexClient`. Bir almanın iki yolu vardır: Bu oluşturmak ya da çağırma `Indexes.GetClient` üzerinde `SearchServiceClient`. İkinci kolaylık sağlamak için kullanırız.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);
```

> [!NOTE]
> Tipik arama uygulamasında, dizin yönetimi ve popülasyon arama sorgularından ayrı bir bileşen tarafından işlenebilir. `Indexes.GetClient` sağlama ek derdinden kurtarır olduğundan dizini doldurmak için uygun olan `SearchCredentials`. Yeni `SearchIndexClient` için `SearchServiceClient` oluşturmak üzere kullandığınız yönetici anahtarını geçirerek bunu yapar. Ancak uygulamanızın sorguları yürüten bölümünde, oluşturmak daha iyidir `SearchIndexClient` doğrudan yalnızca bir yönetici anahtarı yerine veri okumanıza izin verir sorgu anahtarında geçirebilmeniz. Bu, en az ayrıcalık ilkesini ile tutarlıdır ve uygulamanızı daha güvenli hale getirmenize yardımcı olur. Yönetici anahtarları ve sorgu anahtarları hakkında daha fazla bilgi bulabilirsiniz [burada](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).
> 
> 

Biz sahip olduğunuza göre bir `SearchIndexClient`, biz dizini doldurabilirsiniz. Dizin oluşturma işlemi, biz daha sonra size yol gösterir, başka bir yöntem tarafından gerçekleştirilir.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

Son olarak, biz birkaç arama sorguları yürütmek ve sonuçları görüntüler. Bu süre kullandığımız farklı bir `SearchIndexClient`:

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(indexName, configuration);

RunQueries(indexClientForQueries);
```

Biz yakından sürer `RunQueries` yöntemi daha sonra. Yeni kodu `SearchIndexClient`:

```csharp
private static SearchIndexClient CreateSearchIndexClient(string indexName, IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, indexName, new SearchCredentials(queryApiKey));
    return indexClient;
}
```

Şu dizine yazma erişimi gerekmediği sorgu anahtarını kullanırız. Bu bilgileri girebilirsiniz `appsettings.json` dosya [örnek uygulama](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

Bu uygulama geçerli bir hizmet adı ve API anahtarları ile çalıştırırsanız, çıkış şu örnekteki gibi görünmelidir: (Bazı konsol çıktısı değiştirilmiştir "..." ile gösterim amacıyla.)

    Deleting index...

    Creating index...

    Uploading documents...

    Waiting for documents to be indexed...

    Search the entire index for the term 'motel' and return only the HotelName field:

    Name: Secret Point Motel

    Name: Twin Dome Motel


    Apply a filter to the index to find hotels with a room cheaper than $100 per night, and return the hotelId and description:

    HotelId: 1
    Description: The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Times Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.

    HotelId: 2
    Description: The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.


    Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

    Name: Triple Landscape Hotel
    Last renovated on: 9/20/2015 12:00:00 AM +00:00

    Name: Twin Dome Motel
    Last renovated on: 2/18/1979 12:00:00 AM +00:00


    Search the hotel names for the term 'hotel':

    HotelId: 3
    Name: Triple Landscape Hotel
    ...

    Complete.  Press any key to end application... 

Uygulamanın tam kaynak kodu, bu makalenin sonunda sağlanır.

Ardından, biz çağıran yöntemlerin her biri daha yakından bakalım sürer `Main`.

### <a name="creating-an-index"></a>Dizin oluşturma
Oluşturduktan sonra bir `SearchServiceClient`, `Main` zaten varsa, "hotels" dizini siler. Silme işlemi, aşağıdaki yöntemi tarafından gerçekleştirilir:

```csharp
private static void DeleteIndexIfExists(string indexName, SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists(indexName))
    {
        serviceClient.Indexes.Delete(indexName);
    }
}
```

Bu yöntemde belirtilen `SearchServiceClient` dizin varsa ve bu durumda denetlemek için silin.

> [!NOTE]
> Bu makaledeki örnek kod, kolaylık amacıyla Azure Search .NET SDK'sının zaman uyumlu yöntemlerini kullanır. Kendi uygulamalarınızda zaman uyumsuz yöntemler kullanarak bunları ölçeklenebilir ve esnek tutmanızı öneririz. Örneğin, yukarıdaki yöntemi kullanabilirsiniz `ExistsAsync` ve `DeleteAsync` yerine `Exists` ve `Delete`.
> 
> 

Ardından, `Main` bu yöntemi çağırarak yeni bir "hotels" dizini oluşturur:

```csharp
private static void CreateIndex(string indexName, SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = indexName,
        Fields = FieldBuilder.BuildForType<Hotel>()
    };
    
    serviceClient.Indexes.Create(definition);
}
```

Bu yöntem yeni bir oluşturur `Index` nesne listesini `Field` yeni bir dizin şemasını tanımlayan nesne. Her alanın bir adı, veri türü ve arama davranışını tanımlayan çeşitli öznitelikler vardır. `FieldBuilder` Sınıf listesini oluşturmak için yansıtma kullanır `Field` nesneler tarafından genel özelliklerini ve özniteliklerini inceleyerek bir dizin için belirtilen `Hotel` model sınıfı. Biz daha yakından göz atacağız `Hotel` daha sonra sınıfı.

> [!NOTE]
> Her zaman listesini oluşturmak `Field` kullanmak yerine doğrudan nesneleri `FieldBuilder` gerekirse. Örneğin, bir model sınıfı kullanmak isteyebileceğiniz değil veya öznitelikleri ekleyerek değiştirmek istemediğiniz var olan bir model sınıfı kullanmanız gerekebilir.
>
> 

Alanlara ek olarak (Bu parametreleri kısaltma örnekten atlanır) dizine da Puanlama profilleri, öneri araçlarını veya CORS seçenekleri ekleyebilirsiniz. Dizin nesne ve onun bağlı bölümlerinde hakkında daha fazla bilgi bulabilirsiniz [SDK başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index), yanı [Azure Search REST API'si başvurusunda](https://docs.microsoft.com/rest/api/searchservice/).

### <a name="populating-the-index"></a>Dizini doldurma
Sonraki adım `Main` yeni oluşturulan dizin doldurur. Bu dizin oluşturma işlemi aşağıdaki yönteminde gerçekleştirilir: (Bazı kod yerine "..." gösterim amacıyla.  Tam veri doldurma kod için tam örnek çözümü bakın.)

```csharp
private static void UploadDocuments(ISearchIndexClient indexClient)
{
    var hotels = new Hotel[]
    {
        new Hotel()
        {
            HotelId = "1",
            HotelName = "Secret Point Motel",
            ...
            Address = new Address()
            {
                StreetAddress = "677 5th Ave",
                ...
            },
            Rooms = new Room[]
            {
                new Room()
                {
                    Description = "Budget Room, 1 Queen Bed (Cityside)",
                    ...
                },
                new Room()
                {
                    Description = "Budget Room, 1 King Bed (Mountain View)",
                    ...
                },
                new Room()
                {
                    Description = "Deluxe Room, 2 Double Beds (City View)",
                    ...
                }
            }
        },
        new Hotel()
        {
            HotelId = "2",
            HotelName = "Twin Dome Motel",
            ...
            {
                StreetAddress = "140 University Town Center Dr",
                ...
            },
            Rooms = new Room[]
            {
                new Room()
                {
                    Description = "Suite, 2 Double Beds (Mountain View)",
                    ...
                },
                new Room()
                {
                    Description = "Standard Room, 1 Queen Bed (City View)",
                    ...
                },
                new Room()
                {
                    Description = "Budget Room, 1 King Bed (Waterfront View)",
                    ...
                }
            }
        },
        new Hotel()
        {
            HotelId = "3",
            HotelName = "Triple Landscape Hotel",
            ...
            Address = new Address()
            {
                StreetAddress = "3393 Peachtree Rd",
                ...
            },
            Rooms = new Room[]
            {
                new Room()
                {
                    Description = "Standard Room, 2 Queen Beds (Amenities)",
                    ...
                },
                new Room ()
                {
                    Description = "Standard Room, 2 Double Beds (Waterfront View)",
                    ...
                },
                new Room()
                {
                    Description = "Deluxe Room, 2 Double Beds (Cityside)",
                    ...
                }
            }
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

Bu yöntem, dört bölümden oluşur. İlk 3 bir dizi oluşturur `Hotel` her 3 nesneleri `Room` dizine yüklenecek giriş verilerimizi işlevi görecek nesneleri. Kolaylık olması için sabit kodlanmış bu verilerdir. Kendi uygulama verilerinizi büyük olasılıkla bir SQL veritabanı gibi bir dış veri kaynağından gelir.

İkinci bölümü oluşturur bir `IndexBatch` belgeleri içeren. Belirttiğiniz zaman, oluşturun, bu durumda çağırarak toplu uygulamak istediğiniz işlemi `IndexBatch.Upload`. Batch, Azure Search dizinine göre karşıya yüklendikten `Documents.Index` yöntemi.

> [!NOTE]
> Bu örnekte biz yalnızca belgeleri karşıya yükleniyor. Var olan belgelere değişiklikleri birleştirin veya belgeleri silmek isterseniz, çağırarak toplu işlemler oluşturabilirsiniz `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, veya `IndexBatch.Delete` yerine. Çağrı yaparak tek bir toplu işlemde farklı işlemler karıştırabilirsiniz `IndexBatch.New`, bir koleksiyonunu alır `IndexAction` nesneleri, her biri bir belgede belirli bir işlemi gerçekleştirmek için Azure Search söyler. Her oluşturabilirsiniz `IndexAction` gibi karşılık gelen yöntemini çağırarak kendi işlemi ile `IndexAction.Merge`, `IndexAction.Upload`ve benzeri.
> 
> 

Üçüncü kısmı olan bu yöntem, dizin oluşturma için önemli bir hata durumunu işler bir catch bloğu ' dir. Azure Search hizmetiniz toplu işlemdeki belgelerin bazılarına dizin oluşturmada başarısız olursa `Documents.Index` tarafından bir `IndexBatchException` oluşturulur. Bu durum, hizmetiniz ağır yük altında olsa da, belgelere dizin oluşturuyorsanız oluşabilir. **Bu durumu, kodunuzda açık şekilde işlemenizi kesinlikle öneririz.** Başarısız olan belgelere dizin oluşturmayı geciktirip sonra yeniden deneyebilir veya günlük tutup örneğin devam ettiği şekilde devam edebilir veya uygulamanızın veri tutarlılığı gereksinimlerine bağlı olarak başka bir şey yapabilirsiniz.

> [!NOTE]
> Kullanabileceğiniz [ `FindFailedActionsToRetry` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception.findfailedactionstoretry) önceki çağrıda başarısız olan eylemleri içeren yeni bir toplu iş oluşturmak için gereken yöntemini `Index`. Düzgün bir şekilde kullanma hakkında ayrıntılı bilgi yok [StackOverflow üzerinde](https://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).
>
>

Son olarak, `UploadDocuments` yöntemi iki saniye gecikir. Azure Search hizmetinizde dizin oluşturma uyumsuz şekilde meydana gelir; bu nedenle belgelerin aramada kullanılabilir olduğundan emin olmak için örnek uygulamanızın kısa bir süre beklemesi gerekir. Bu gibi gecikmeler genellikle yalnızca gösterilerde, testlerde ve örnek uygulamalarda gereklidir.

<a name="how-dotnet-handles-documents"></a>

#### <a name="how-the-net-sdk-handles-documents"></a>.NET SDK belgeleri nasıl işler?
Azure Search .NET SDK'sının `Hotel` gibi kullanıcı tanımlı bir sınıfın örneklerini dizine nasıl yükleyebildiğini merak ediyor olabilirsiniz. Bu sorunun yanıtlanmasına yardımcı olmak için göz atalım `Hotel` sınıfı:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsSearchable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.EnLucene)]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("Description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    // SmokingAllowed reflects whether any room in the hotel allows smoking.
    // The JsonIgnore attribute indicates that a field should not be created 
    // in the index for this property and it will only be used by code in the client.
    [JsonIgnore]
    public bool? SmokingAllowed => (Rooms != null) ? Array.Exists(Rooms, element => element.SmokingAllowed == true) : (bool?)null;

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? Rating { get; set; }

    public Address Address { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }

    public Room[] Rooms { get; set; }
}
```

Fark edilecek ilk şey her bir genel özelliğinin adı olan `Hotel` sınıfı dizin tanımında aynı ada sahip bir alan eşleştirme. Küçük harfle ("ortası büyük harf") başlatmak için her bir alan isterseniz SDK'sı ile otomatik olarak ortası büyük özellik adlarını eşleştirmek için söyleyebilirsiniz `[SerializePropertyNamesAsCamelCase]` sınıfı özniteliği. Bu senaryoda, hedef şemanın uygulama geliştiricisinin denetimi dışında "Pascal harf adlandırma kuralları. NET'te" ihlal gerek kalmadan olduğu veri bağlamayı gerçekleştiren .NET uygulamalarında yaygındır.

> [!NOTE]
> Azure Search .NET SDK'sı, özel model nesnelerinizi JSON'a ve JSON'dan seri hale getirmek ve seri durumdan çıkarmak için [NewtonSoft JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) kitaplığını kullanır. Gerekirse bu seri hale getirmeyi özelleştirebilirsiniz. Daha fazla bilgi için [JSON.NET ile özel serileştirme](#JsonDotNet).
> 
> 

Her bir özellik düzenlenmiş özniteliklerle gibi ikinci bir şey fark etmeye olan `IsFilterable`, `IsSearchable`, `Key`, ve `Analyzer`. Bu öznitelikler doğrudan eşleme [karşılık gelen alan öznitelikleri Azure Search dizini](https://docs.microsoft.com/rest/api/searchservice/create-index#request). `FieldBuilder` Sınıfı dizini için alan tanımları oluşturmak için bu özellikleri kullanır.

Üçüncü önemli şey hakkında `Hotel` sınıftır genel özelliklerin veri türleri. Bu özelliklerin .NET türleri, dizin tanımında eşdeğer alan türleriyle eşlenir. Örneğin, `Category` dize özelliği `Edm.String` türündeki `category` alanına eşlenir. Arasında benzer türde eşlemeler bulunur `bool?`, `Edm.Boolean`, `DateTimeOffset?`, ve `Edm.DateTimeOffset` ve benzeri. Tür eşlemesine yönelik belirli kurallar, [Azure Search .NET SDK başvurusundaki](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.get) `Documents.Get` yönteminde belirtilmiştir. `FieldBuilder` Sınıfı bu eşlemenin sizin için üstlenir ancak yine de serileştirme sorunları gidermek gerektiği durumlarda anlamak yararlı olabilir.

Fark etmeye ortaya çıktı `SmokingAllowed` özelliği?

```csharp
[JsonIgnore]
public bool? SmokingAllowed => (Rooms != null) ? Array.Exists(Rooms, element => element.SmokingAllowed == true) : (bool?)null;
```

`JsonIgnore` Özniteliği bu özellikte söyler `FieldBuilder` , bir alan olarak dizine serileştirilecek değil.  Bu, uygulamanızda Yardımcıları kullanabileceğiniz istemci tarafı hesaplanan özellikleri oluşturmak için harika bir yoludur.  Bu durumda, `SmokingAllowed` özelliği yansıtır herhangi gerekmediğini `Room` içinde `Rooms` koleksiyon İçilmez sağlar.  Tüm bunlar false ise, tüm otel İçilmez izin vermiyor gösterir.

Bazı özellikler gibi `Address` ve `Rooms` .NET sınıfları örnekleridir.  Bu özellikler daha karmaşık veri yapılarını temsil eder ve sonuç olarak, alanlarla gerektiren bir [karmaşık veri türü](https://docs.microsoft.com/azure/search/search-howto-complex-data-types) dizinde.

`Address` Özelliğini temsil eder, birden fazla değer bir dizi `Address` sınıfı, aşağıda tanımlanmıştır:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Newtonsoft.Json;

namespace AzureSearch.SDKHowTo
{
    public partial class Address
    {
        [IsSearchable]
        public string StreetAddress { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string City { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string StateProvince { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string PostalCode { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string Country { get; set; }
    }
}
```

Bu sınıf, Amerika Birleşik Devletleri ve Kanada'da adreslerini tanımlamak için kullanılan standart değerleri içerir. Bu gibi türleri mantıksal alanları dizine gruplamak için kullanabilirsiniz.

`Rooms` Özelliğini temsil eden bir dizi `Room` nesneler:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Newtonsoft.Json;

namespace AzureSearch.SDKHowTo
{
    public partial class Room
    {
        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.EnMicrosoft)]
        public string Description { get; set; }

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.FrMicrosoft)]
        [JsonProperty("Description_fr")]
        public string DescriptionFr { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string Type { get; set; }

        [IsFilterable, IsFacetable]
        public double? BaseRate { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string BedOptions { get; set; }

        [IsFilterable, IsFacetable]
        public int SleepsCount { get; set; }

        [IsFilterable, IsFacetable]
        public bool? SmokingAllowed { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string[] Tags { get; set; }
    }
}
```

Veri modelinizde .NET ve karşılık gelen dizin şeması, son kullanıcıya vermek istediğiniz arama deneyimini destekleyecek şekilde tasarlanmalıdır. .NET, IE belge dizinde her bir üst düzey nesnenin, kullanıcı arabiriminde göstermek bir arama sonucunun karşılık gelir. Örneğin, bir otel arama uygulamasında, otel özelliklerini veya belirli bir oda özelliklerini otel adına göre aramak son kullanıcılarınızın isteyebilirsiniz. Biz bazı sorgu örnekleri biraz daha sonra değineceğiz.

Kendi sınıflarınızı belge dizinde ile etkileşim kurmak için kullanma yeteneğini bu her iki yönde de çalışır; Ayrıca, arama sonuçlarını almak ve biz sonraki bölümde göreceğiniz gibi tercih ettiğiniz bir tür için otomatik olarak seri durumdan SDK'sına sahip.

> [!NOTE]
> Azure Search .NET SDK'sı, alan adlarını alan değerlerine bir anahtar/değer eşlemesi olan `Document` sınıfını kullanarak dinamik tür belirtilmiş belgeleri de destekler. Bu durum, tasarım sırasında dizin şemasını bilmediğiniz veya belirli model sınıflarına bağlamanın kullanışlı olmayacağı senaryolarda kullanışlıdır. SDK'da belgelerle ilgili tüm yöntemler, `Document` sınıfıyla çalışan aşırı yüklerin yanı sıra genel türde bir parametre alan kesin tür belirtilmiş aşırı yüklere de sahiptir. Örnek kodda yalnızca ikinci durum Bu öğreticide kullanılır. [ `Document` Sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document) devraldığı `Dictionary<string, object>`.
> 
>

**Neden boş değer atanabilir türleri kullanmalısınız?**

Bir Azure Search dizinine eşlemek için yeni model sınıflarınızı tasarlarken, `bool` ve `int` gibi değer türü özelliklerinin boş değer atanabilir (örneğin, `bool` yerine `bool?`) şeklinde bildirilmesini öneririz. Boş değer atanamayan bir özellik kullanırsanız buna karşılık gelen alan için dizininizdeki hiçbir belgenin boş bir değer içermediğini **garanti etmeniz** gerekir. Bunu zorlamanıza ne SDK ne de Azure Search hizmeti yardımcı olur.

Bu yalnızca kuramsal bir sorun değildir: Eklediğiniz yeni alan türü var olan bir dizin için bir senaryoyu düşünün `Edm.Int32`. Dizin tanımını güncelleştirdikten sonra, tüm belgelerin bu yeni alan için boş bir değeri olur (bunun nedeni, Azure Search'te tüm türlerin boş değer atanabilir olmasıdır). Ardından bu alan için boş değer atanamayan bir `int` özelliğiyle bir model sınıfı kullanırsanız belgeleri almaya çalışırken bunun gibi bir `JsonSerializationException` alırsınız:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Bu nedenle, en iyi uygulama olarak model sınıflarınızda boş değer atanabilir türler kullanmanızı öneririz.

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a>JSON.NET ile özel serileştirme
SDK'sı, seri hale getirme ve belgeleri seri durumundan çıkarma için JSON.NET kullanır. Seri hale getirme özelleştirmek ve tanımlayarak kendi gerekirse seri durumdan çıkarma `JsonConverter` veya `IContractResolver`. Daha fazla bilgi için [JSON.NET belgeleri](https://www.newtonsoft.com/json/help/html/Introduction.htm). Uygulamanızın Azure arama ve daha gelişmiş diğer senaryolar ile kullanmak için mevcut bir model sınıfı uyum sağlamak istediğinizde bu yararlı olabilir. Örneğin, özel seri hale getirme ile şunları yapabilirsiniz:

* Dahil edilecek veya belge alanları depolanan bazı özellikleri model sınıfınızın hariç.
* Kodunuzdaki özellik adları ve dizininizdeki alan adları arasındaki eşleme.
* Özellikleri belge alanlarına eşleme için kullanılan özel öznitelikler oluşturun.

Github'da Azure Search .NET SDK'sı için birim testleri özel serileştirme uygulama örnekleri bulabilirsiniz. İyi bir başlangıç noktasıdır [bu klasör](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models). Bu, özel serileştirme testler tarafından kullanılan sınıfları içerir.

### <a name="searching-for-documents-in-the-index"></a>Dizin içindeki belgeler aranıyor
Örnek uygulama son adımda dizindeki bazı belgeler için arama gerçekleştirmektir:

```csharp
private static void RunQueries(ISearchIndexClient indexClient)
{
    SearchParameters parameters;
    DocumentSearchResult<Hotel> results;

    Console.WriteLine("Search the entire index for the term 'motel' and return only the HotelName field:\n");

    parameters =
        new SearchParameters()
        {
            Select = new[] { "HotelName" }
        };

    results = indexClient.Documents.Search<Hotel>("motel", parameters);

    WriteDocuments(results);

    Console.Write("Apply a filter to the index to find hotels with a room cheaper than $100 per night, ");
    Console.WriteLine("and return the hotelId and description:\n");

    parameters =
        new SearchParameters()
        {
            Filter = "Rooms/any(r: r/BaseRate lt 100)",
            Select = new[] { "HotelId", "Description" }
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
    Console.Write("in descending order, take the top two results, and show only hotelName and ");
    Console.WriteLine("lastRenovationDate:\n");

    parameters =
        new SearchParameters()
        {
            OrderBy = new[] { "LastRenovationDate desc" },
            Select = new[] { "HotelName", "LastRenovationDate" },
            Top = 2
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.WriteLine("Search the entire index for the term 'hotel':\n");

    parameters = new SearchParameters();
    results = indexClient.Documents.Search<Hotel>("hotel", parameters);

    WriteDocuments(results);
}
```

Her bir sorgu yürütür, bu yöntem öncelikle yeni bir oluşturur `SearchParameters` nesne. Bu nesne, sıralama, filtreleme, sayfalama ve modelleme gibi sorgu için ek seçenekleri belirlemek için kullanılır. Bu yöntemde, biz tutunun `Filter`, `Select`, `OrderBy`, ve `Top` özelliği için farklı sorgular. Tüm `SearchParameters` özellikleri belgelenmiştir [burada](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).

Arama sorgusu gerçekten yürütülecek sonraki adımdır bakın. Arama çalıştıran yapılır kullanarak `Documents.Search` yöntemi. Her sorgu için bir dize olarak kullanılacak arama metni biz geçirin (veya `"*"` arama metni ise), ayrıca daha önce oluşturduğunuz arama parametreleri. Ayrıca belirttiğimiz `Hotel` tür parametresi için `Documents.Search`, arama sonuçlarındaki belgelerin türündeki nesneleri seri durumdan çıkarılacak SDK söyler `Hotel`.

> [!NOTE]
> Arama sorgu ifadesi söz dizimi hakkında daha fazla bilgi bulabilirsiniz [burada](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).
> 
> 

Son olarak, her sorgu sonra bu yöntem her konsola yazdırma tüm eşleşmeleri arama sonuçlarında gezinir:

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

Sırayla her bir sorgu daha yakın bir göz atalım. İlk sorgu yürütmek için kod aşağıdaki gibidir:

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "HotelName" }
    };

results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

Bu durumda, tüm dizinde "motel" sözcüğü için aranabilir alanların herhangi birinde aradığınız ve yalnızca belirtildiği gibi otel adlarını almak istiyoruz `Select` parametresi. Sonuçları şunlardır:

    Name: Secret Point Motel

    Name: Twin Dome Motel

Sonraki sorgu biraz daha ilginçtir.  Biz, gecelik bir oranda $100'den küçük bir oda ve yalnızca otel kimlik ve açıklama dönüş herhangi Oteller bulmak istiyorsanız:

```csharp
parameters =
    new SearchParameters()
    {
        Filter = "Rooms/any(r: r/BaseRate lt 100)",
        Select = new[] { "HotelId", "Description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

Bu sorgu kullanan bir OData `$filter` ifade `Rooms/any(r: r/BaseRate lt 100)`, dizin belgelerde filtrelemek için. Bu kullanır [herhangi bir işleç](https://docs.microsoft.com/azure/search/search-query-odata-collection-operators) uygulamak için ' BaseRate lt 100' odaları koleksiyondaki her öğe için. Azure Search'ü destekleyen OData sözdizimi hakkında daha fazla bilgi bulabilirsiniz [burada](https://docs.microsoft.com/azure/search/query-odata-filter-orderby-syntax).

Sorgu sonuçlarını şunlardır:

    HotelId: 1
    Description: The hotel is ideally located on the main commercial artery of the city in the heart of New York...

    HotelId: 2
    Description: The hotel is situated in a nineteenth century plaza, which has been expanded and renovated to...

Ardından, en son renovated üst iki Oteller bulmak ve son Yenileme tarihi ve Otel adını göstermek istiyoruz. Kod aşağıdaki gibidir: 

```csharp
parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "LastRenovationDate desc" },
        Select = new[] { "HotelName", "LastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

Bu durumda, yeniden OData söz dizimini belirtmek için kullanıyoruz `OrderBy` parametre olarak `lastRenovationDate desc`. Ayrıca `Top` yalnızca aldığımız ilk iki belgeleri emin olmak için 2. Önce ayarlar gibi `Select` hangi alanların döndürülmesi gerektiğini belirtmek için.

Sonuçları şunlardır:

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Son olarak, "otel" sözcüğü eşleşen tüm hotels adlarını bulmak istiyoruz:

```csharp
parameters = new SearchParameters()
{
    SearchFields = new[] { "HotelName" }
};
results = indexClient.Documents.Search<Hotel>("hotel", parameters);

WriteDocuments(results);
```

Ve işte biz belirtmediğiniz olduğundan tüm alanlar içeren sonuçları `Select` özelliği:

    HotelId: 3
    Name: Triple Landscape Hotel
    ...

Bu adım öğretici tamamlanmış, ancak burada durabilir yok. ** Sonraki adımlar, Azure arama hakkında daha fazla bilgi için ek kaynaklar sağlayın.

## <a name="next-steps"></a>Sonraki adımlar
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) ve [REST API](https://docs.microsoft.com/rest/api/searchservice/) başvurularına göz atın.
* Gözden geçirme [adlandırma kuralları](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) çeşitli adlandırma kurallarını öğrenin.
* Gözden geçirme [desteklenen veri türleri](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) Azure Search'te.
