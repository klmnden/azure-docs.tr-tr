---
title: -Azure Search bir .NET uygulamasından Azure Search kullanma
description: Bir .NET uygulaması kullanarak Azure Search kullanma hakkında bilgi edinin C# ve .NET SDK'sı. Kod tabanlı görevler dizin içeriği hizmete bağlanın ve dizin sorgulama.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: brjohnst
ms.openlocfilehash: 25a156c4403b7a89f7a7bf7f6acf22fa34216791
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025138"
---
# <a name="how-to-use-azure-search-from-a-net-application"></a>Bir .NET uygulamasından Azure Search kullanma

> [!Important]
> Bu içerik yine de tamamlanmamıştır. Azure Search .NET SDK, sürüm 9.0, NuGet üzerinde kullanılabilir. Bu geçiş kılavuzunda en yeni sürüme yükseltme açıklayan güncelleştirmeyi çalışıyoruz. Bizi izlemeye devam edin.
>

Bu makale ile çalışmaya başlamanızı sağlayacak bir kılavuz niteliğindedir [Azure Search .NET SDK'sı](https://aka.ms/search-sdk). .NET SDK'sı, Azure Search kullanarak uygulamanızda bir zengin arama deneyimi uygulamak için kullanabilirsiniz.

## <a name="whats-in-the-azure-search-sdk"></a>Azure nedir arama SDK'sı
SDK'sı dizinlerinizi yönetmenizi sağlayan birkaç istemci kitaplıkları oluşuyorsa, veri kaynakları, dizin oluşturucular ve eş anlamlı, karşıya yükleme yanı sıra eşler ve belgeleri yönetmek ve tüm HTTP ve JSON ayrıntılarını ile uğraşmak zorunda kalmadan sorgular yürütün. Bu istemci kitaplıklarının tümü NuGet paketleri olarak dağıtılır.

Ana NuGet paketi `Microsoft.Azure.Search`, bağımlılık olarak diğer tüm paketleri içeren bir meta-package olduğu. Bu paket, yeni başlıyorsanız veya uygulamanıza Azure Search'ün tüm özellikleri gerekir biliyorsanız kullanın.

SDK diğer NuGet paketleri şunlardır:
 
  - `Microsoft.Azure.Search.Data`: Azure Search kullanarak bir .NET uygulaması geliştirmeye devam ediyoruz ve sorgu veya dizinleri belgeleri güncelleştirmek yalnızca ihtiyacınız varsa bu paketi kullanın. Ayrıca oluşturmak veya dizinleri güncelleştirme gerekirse eş anlamlı sözcük eşlemelerini veya diğer hizmet düzeyi kaynakları kullanmak `Microsoft.Azure.Search` bunun yerine paket.
  - `Microsoft.Azure.Search.Service`: Azure Search dizinlerini, eş anlamlı sözcük eşlemelerini, dizin oluşturucular, veri kaynakları veya diğer hizmet düzeyi kaynakları yönetmek için. NET'te Otomasyon geliştiriyorsanız, bu paketi kullanın. Dizinlerinizi içinde sorgu veya güncelleştirme belgelere yalnızca ihtiyacınız varsa `Microsoft.Azure.Search.Data` bunun yerine paket. Azure Search'ün tüm işlevlerine ihtiyacınız varsa, `Microsoft.Azure.Search` bunun yerine paket.
  - `Microsoft.Azure.Search.Common`: Azure Search .NET kitaplıkları tarafından gereken genel türler. Bu paket, uygulamanızda doğrudan kullanmak gerekmez; Yalnızca, bir bağımlılık olarak kullanılmak üzere tasarlanmıştır.

Sınıflar gibi çeşitli istemci kitaplıkları tanımlamak `Index`, `Field`, ve `Document`, yanı sıra operations ister `Indexes.Create` ve `Documents.Search` üzerinde `SearchServiceClient` ve `SearchIndexClient` sınıfları. Bu sınıfların şu ad alanlarından düzenlenmiştir:

* [Microsoft.Azure.Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft.Azure.Search.Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

Geçerli Azure Search .NET SDK'sı sürümü genel kullanıma sunulmuştur. Bir sonraki sürümünde birleştirmek bize geri bildirim sağlamak istiyorsanız, lütfen şu adresi ziyaret bizim [geri bildirim sayfası](https://feedback.azure.com/forums/263029-azure-search/).

.NET SDK'sı sürümünü destekleyen `2017-11-11` , [Azure Search REST API'sine](https://docs.microsoft.com/rest/api/searchservice/). Bu sürüm, artık dizin oluşturucular için artımlı iyileştirme yanı sıra, eş anlamlılar için destek içerir. 

Bu SDK'sı tarafından desteklenmeyen [yönetim işlemlerini](https://docs.microsoft.com/rest/api/searchmanagement/) oluşturma ve arama hizmetleri ölçeklendirme ve API anahtarlarını yönetme gibi. Bir .NET uygulamasından arama kaynaklarınızı yönetmek ihtiyacınız varsa, kullanabileceğiniz [Azure Search .NET Yönetim SDK'sı](https://aka.ms/search-mgmt-sdk).

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>SDK'sının en son sürüme yükseltme
Zaten Azure Search .NET SDK'sı daha eski bir sürümünü kullanıyorsanız ve genel kullanıma sunulan yeni sürüme yükseltmek istiyorsanız [bu makalede](search-dotnet-sdk-migration-version-5.md) açıklar nasıl.

## <a name="requirements-for-the-sdk"></a>SDK'sı gereksinimleri
1. Visual Studio 2017.
2. Kendi Azure Search hizmeti. SDK'yı kullanmak için hizmetinizin API anahtarlarını bir veya daha fazla ve adı gerekir. [Portalda hizmet oluşturma](search-create-service-portal.md) Bu adımlarda yardımcı olur.
3. Azure Search .NET SDK'sını indirin [NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Search) "NuGet paketlerini Yönet" Visual Studio kullanarak. Paket adı için yalnızca arama `Microsoft.Azure.Search` üzerinde NuGet.org (veya biri diğer işlevlerinin bir alt kümesini yalnızca gerekiyorsa Yukarıdaki adları paketi).

Azure Search .NET SDK'sı, .NET Framework 4.5.2'yi hedefleyen uygulamalar destekler ve üzeri, .NET Core yanı sıra.

## <a name="core-scenarios"></a>Temel senaryoları
Arama uygulamanız yapmanız gerekir birkaç şey vardır. Bu öğreticide, bu temel senaryolar şu konulara değineceğiz:

* Dizin oluşturma
* Belgelerle dizini doldurma
* Belgeler tam metin arama ve filtreler kullanılarak aranıyor

Aşağıdaki örnek kod, her birini gösterir. Kod parçacıkları, kendi uygulamanızda kullanmaktan çekinmeyin.

### <a name="overview"></a>Genel Bakış
Biz keşfetmeye örnek uygulamayı yeni bir oluşturur "hotels" adlı dizin birkaç belge ile doldurur ve ardından bazı arama sorguları yürütür. Genel akış gösteren eden ana program şöyledir:

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
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

Sonra dizininin doldurulması gerekir. Bunu yapmak için ihtiyacımız bir `SearchIndexClient`. Bir almanın iki yolu vardır: Bu oluşturmak ya da çağırma `Indexes.GetClient` üzerinde `SearchServiceClient`. İkinci kolaylık sağlamak için kullanırız.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> Genel bir arama uygulamasında, dizin yönetimi ve popülasyon, arama sorgularından ayrı bir bileşen tarafından işlenir. `Indexes.GetClient`, başka bir `SearchCredentials` sağlamanızı gerektirmediğinden, dizini doldurmak için uygundur. Yeni `SearchIndexClient` için `SearchServiceClient` oluşturmak üzere kullandığınız yönetici anahtarını geçirerek bunu yapar. Ancak uygulamanızın sorguları yürüten bölümünde, bir yönetici anahtarı yerine sorgu anahtarında geçirebilmeniz için doğrudan `SearchIndexClient` oluşturmak daha iyi olur. Bu, en az ayrıcalık ilkesini ile tutarlıdır ve uygulamanızı daha güvenli hale getirmenize yardımcı olur. Yönetici anahtarları ve sorgu anahtarları hakkında daha fazla bilgi bulabilirsiniz [burada](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).
> 
> 

Biz sahip olduğunuza göre bir `SearchIndexClient`, biz dizini doldurabilirsiniz. Bu, size daha sonra size yol gösterir, başka bir yöntem tarafından gerçekleştirilir.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

Son olarak, biz birkaç arama sorguları yürütmek ve sonuçları görüntüler. Bu süre kullandığımız farklı bir `SearchIndexClient`:

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

Biz yakından sürer `RunQueries` yöntemi daha sonra. Yeni kodu `SearchIndexClient`:

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

Şu dizine yazma erişimi gerekmediği sorgu anahtarını kullanırız. Bu bilgileri girebilirsiniz `appsettings.json` dosya [örnek uygulama](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

Bu uygulama geçerli bir hizmet adı ve API anahtarları ile çalıştırırsanız, çıkış şöyle görünmelidir:

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

Uygulamanın tam kaynak kodu, bu makalenin sonunda sağlanır.

Ardından, biz çağıran yöntemlerin her biri daha yakından bakalım sürer `Main`.

### <a name="creating-an-index"></a>Dizin oluşturma
Oluşturduktan sonra bir `SearchServiceClient`, `Main` zaten varsa, "hotels" dizini siler. Aşağıdaki yöntemi tarafından gerçekleştirilir:

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
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

Bu yöntem yeni bir oluşturur `Index` nesne listesini `Field` yeni bir dizin şemasını tanımlayan nesne. Her alanın bir adı, veri türü ve arama davranışını tanımlayan çeşitli öznitelikler vardır. `FieldBuilder` Sınıf listesini oluşturmak için yansıtma kullanır `Field` nesneler tarafından genel özelliklerini ve özniteliklerini inceleyerek bir dizin için belirtilen `Hotel` model sınıfı. Biz daha yakından göz atacağız `Hotel` daha sonra sınıfı.

> [!NOTE]
> Her zaman listesini oluşturmak `Field` kullanmak yerine doğrudan nesneleri `FieldBuilder` gerekirse. Örneğin, bir model sınıfı kullanmak isteyebileceğiniz değil veya öznitelikleri ekleyerek değiştirmek istemediğiniz var olan bir model sınıfı kullanmanız gerekebilir.
>
> 

Alanlara ek olarak (Bu örnekteki kısaltma atlanır) dizine da Puanlama profilleri, öneri araçlarını veya CORS seçenekleri ekleyebilirsiniz. Dizin nesne ve onun bağlı bölümlerinde hakkında daha fazla bilgi bulabilirsiniz [SDK başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index), yanı [Azure Search REST API'si başvurusunda](https://docs.microsoft.com/rest/api/searchservice/).

### <a name="populating-the-index"></a>Dizini doldurma
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

Bu yöntem, dört bölümden oluşur. İlk bir dizi oluşturur `Hotel` dizine yüklenecek giriş verilerimizi işlevi görecek nesneleri. Kolaylık olması için sabit kodlanmış bu verilerdir. Kendi uygulama verilerinizi büyük olasılıkla bir SQL veritabanı gibi bir dış veri kaynağından gelir.

İkinci bölümü oluşturur bir `IndexBatch` belgeleri içeren. Belirttiğiniz zaman, oluşturun, bu durumda çağırarak toplu uygulamak istediğiniz işlemi `IndexBatch.Upload`. Batch, Azure Search dizinine göre karşıya yüklendikten `Documents.Index` yöntemi.

> [!NOTE]
> Bu örnekte biz yalnızca belgeleri karşıya yükleniyor. Var olan belgelere değişiklikleri birleştirin veya belgeleri silmek isterseniz, çağırarak toplu işlemler oluşturabilirsiniz `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, veya `IndexBatch.Delete` yerine. Çağrı yaparak tek bir toplu işlemde farklı işlemler karıştırabilirsiniz `IndexBatch.New`, bir koleksiyonunu alır `IndexAction` nesneleri, her biri bir belgede belirli bir işlemi gerçekleştirmek için Azure Search söyler. Her oluşturabilirsiniz `IndexAction` gibi karşılık gelen yöntemini çağırarak kendi işlemi ile `IndexAction.Merge`, `IndexAction.Upload`ve benzeri.
> 
> 

Üçüncü kısmı olan bu yöntem, dizin oluşturma için önemli bir hata durumunu işler bir catch bloğu ' dir. Azure Search hizmetiniz toplu işlemdeki belgelerin bazılarına dizin oluşturmada başarısız olursa `Documents.Index` tarafından bir `IndexBatchException` oluşturulur. Bu durum, hizmetiniz ağır yük altındayken belgelere dizin oluşturuyorsanız oluşabilir. **Bu durumu, kodunuzda açık şekilde işlemenizi kesinlikle öneririz.** Başarısız olan belgelere dizin oluşturmayı geciktirip sonra yeniden deneyebilir veya günlük tutup örneğin devam ettiği şekilde devam edebilir veya uygulamanızın veri tutarlılığı gereksinimlerine bağlı olarak başka bir şey yapabilirsiniz.

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

Fark edilecek ilk şey her ortak özelliği olan `Hotel` dizin tanımını, ancak çok önemli bir fark bir alana karşılık gelir: Her alanın adı küçük harfle ("ortası büyük harf"), sırasında her bir genel özelliğinin adını başlar `Hotel` büyük harfle ("Pascal harf") başlar. Bu durum, hedef şemanın uygulama geliştiricisinin denetimi dışında kaldığı bir veri bağlamayı gerçekleştiren .NET uygulamalarında ortak bir senaryodur. Özellik adlarını ortası büyük harf yaparak .NET adlandırma yönergelerini bozmanın yerine, `[SerializePropertyNamesAsCamelCase]` özniteliğiyle SDK'nın özellik adlarını otomatik olarak ortası büyük harfle eşlenmesini söyleyebilirsiniz.

> [!NOTE]
> Azure Search .NET SDK'sı, özel model nesnelerinizi JSON'a ve JSON'dan seri hale getirmek ve seri durumdan çıkarmak için [NewtonSoft JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) kitaplığını kullanır. Gerekirse bu seri hale getirmeyi özelleştirebilirsiniz. Daha fazla bilgi için [JSON.NET ile özel serileştirme](#JsonDotNet).
> 
> 

Fark etmeye ikinci şey her ortak özelliği süslemek öznitelikleri olan (gibi `IsFilterable`, `IsSearchable`, `Key`, ve `Analyzer`). Bu öznitelikler doğrudan eşleme [Azure Search dizini karşılık gelen özniteliklerini](https://docs.microsoft.com/rest/api/searchservice/create-index#request). `FieldBuilder` Sınıfı dizini için alan tanımları oluşturmak için bunları kullanır.

Üçüncü önemli şey hakkında `Hotel` sınıftır genel özelliklerin veri türleri. Bu özelliklerin .NET türleri dizin tanımında eşdeğer alan türleriyle eşlenir. Örneğin, `Category` dize özelliği `Edm.String` türündeki `category` alanına eşlenir. `bool?` ve `Edm.Boolean`, `DateTimeOffset?` ve `Edm.DateTimeOffset`, vb. arasında benzer türde eşlemeler bulunur. Tür eşlemesine yönelik belirli kurallar, [Azure Search .NET SDK başvurusundaki](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.get) `Documents.Get` yönteminde belirtilmiştir. `FieldBuilder` Sınıfı bu eşlemenin sizin için üstlenir ancak yine de serileştirme sorunları gidermek gerektiği durumlarda anlamak yararlı olabilir.

Kendi sınıflarınızı belge olarak kullanabilme iki yönde de işe yarar; Ayrıca, arama sonuçlarını almak ve biz sonraki bölümde göreceğiniz gibi tercih ettiğiniz bir tür için otomatik olarak seri durumdan SDK'sına sahip.

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
SDK'sı, seri hale getirme ve belgeleri seri durumundan çıkarma için JSON.NET kullanır. Seri hale getirme özelleştirmek ve tanımlayarak kendi gerekirse seri durumdan çıkarma `JsonConverter` veya `IContractResolver` (bkz [JSON.NET belgeleri](https://www.newtonsoft.com/json/help/html/Introduction.htm) daha fazla ayrıntı için). Uygulamanızın Azure arama ve daha gelişmiş diğer senaryolar ile kullanmak için mevcut bir model sınıfı uyum sağlamak istediğinizde bu yararlı olabilir. Örneğin, özel seri hale getirme ile şunları yapabilirsiniz:

* Dahil edilecek veya belge alanları depolanan bazı özellikleri model sınıfınızın hariç.
* Kodunuzdaki özellik adları ve dizininizdeki alan adları arasındaki eşleme.
* Özellikleri belge alanlarına eşleme için kullanılan özel öznitelikler oluşturun.

Github'da Azure Search .NET SDK'sı için birim testleri özel serileştirme uygulama örnekleri bulabilirsiniz. İyi bir başlangıç noktasıdır [bu klasör](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models). Bu, özel serileştirme testler tarafından kullanılan sınıfları içerir.

### <a name="searching-for-documents-in-the-index"></a>Dizin içindeki belgeler aranıyor
Örnek uygulama son adımda dizindeki bazı belgeler için arama gerçekleştirmektir. Aşağıdaki yöntem bunu yapar:

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

Her bir sorgu yürütür, bu yöntem öncelikle yeni bir oluşturur `SearchParameters` nesne. Bu sorgunun sıralama, filtreleme, sayfalama ve model oluşturma gibi ek seçenekleri belirlemek için kullanılır. Bu yöntemde, biz tutunun `Filter`, `Select`, `OrderBy`, ve `Top` özelliği için farklı sorgular. Tüm `SearchParameters` özellikleri belgelenmiştir [burada](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).

Arama sorgusu gerçekten yürütülecek sonraki adımdır bakın. Bu yapılır kullanarak `Documents.Search` yöntemi. Her sorgu için bir dize olarak kullanılacak arama metni biz geçirin (veya `"*"` arama metni ise), ayrıca daha önce oluşturduğunuz arama parametreleri. Ayrıca belirttiğimiz `Hotel` tür parametresi için `Documents.Search`, arama sonuçlarındaki belgelerin türündeki nesneleri seri durumdan çıkarılacak SDK söyler `Hotel`.

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
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

Bu durumda, biz "budget" sözcüğü eşleşen Oteller için arama yapıyorsanız ve yalnızca otel adlarını belirtildiği gibi geri almak istiyoruz `Select` parametresi. Sonuçları şunlardır:

    Name: Roach Motel

Ardından, küçüktür $150 gecelik fiyatı ile Oteller bulmak ve yalnızca otel kimlik ve açıklama dönüş istiyoruz:

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

Bu sorgu kullanan bir OData `$filter` ifade `baseRate lt 150`, dizin belgelerde filtrelemek için. Azure Search'ü destekleyen OData sözdizimi hakkında daha fazla bilgi bulabilirsiniz [burada](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).

Sorgu sonuçlarını şunlardır:

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river

Ardından, en son renovated üst iki Oteller bulmak ve son Yenileme tarihi ve Otel adını göstermek istiyoruz. Kod aşağıdaki gibidir: 

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

Bu durumda, yeniden OData söz dizimini belirtmek için kullanıyoruz `OrderBy` parametre olarak `lastRenovationDate desc`. Ayrıca `Top` yalnızca aldığımız ilk iki belgeleri emin olmak için 2. Önce ayarlar gibi `Select` hangi alanların döndürülmesi gerektiğini belirtmek için.

Sonuçları şunlardır:

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Son olarak, "motel" sözcüğü eşleşen tüm Oteller bulmak istiyoruz:

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

Ve işte biz belirtmediğiniz olduğundan tüm alanlar içeren sonuçları `Select` özelliği:

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

Bu adım öğretici tamamlanmış, ancak burada durabilir yok. ** Sonraki adımlar, Azure arama hakkında daha fazla bilgi için ek kaynaklar sağlayın.

## <a name="next-steps"></a>Sonraki adımlar
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) ve [REST API](https://docs.microsoft.com/rest/api/searchservice/) başvurularına göz atın.
* Gözden geçirme [adlandırma kuralları](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) çeşitli adlandırma kurallarını öğrenin.
* Gözden geçirme [desteklenen veri türleri](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) Azure Search'te.
