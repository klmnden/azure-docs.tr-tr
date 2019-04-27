---
title: 'Hızlı Başlangıç: Listedeki bir dizin oluşturma bir C# konsol uygulaması - Azure Search'
description: Tam metin arama yapılabilir bir dizin içinde oluşturmayı öğrenin C# Azure Search .NET SDK'sını kullanarak.
author: heidisteen
manager: cgronlun
ms.author: heidist
tags: azure-portal
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 04/08/2019
ms.openlocfilehash: 83842893e0ffc6bb954832cd65b6312b59bbcaa3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60516569"
---
# <a name="quickstart-1---create-an-azure-search-index-in-c"></a>Hızlı Başlangıç: 1 - Azure Search dizini oluşturmaC#
> [!div class="op_single_selector"]
> * [C#](search-create-index-dotnet.md)
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-howto-dotnet-sdk.md)
> * [Postman](search-fiddler.md)
>*

Bu makalede oluşturma işleminde size kılavuzluk eder [Azure Search dizini](search-what-is-an-index.md) kullanarak C# ve [.NET SDK'sı](https://aka.ms/search-sdk). İlk 3 parçalı alıştırma derste oluşturma, yükleme ve sorgu bir dizin için budur. Dizin oluşturma, şu görevleri gerçekleştirerek gerçekleştirilir:

> [!div class="checklist"]
> * Oluşturma bir [ `SearchServiceClient` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient?view=azure-dotnet) bir arama hizmetine bağlanmak için nesne.
> * Oluşturma bir [ `Index` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) nesnesi bir parametre olarak geçmesine `Indexes.Create`.
> * Çağrı `Indexes.Create` metodunda `SearchServiceClient` göndermek için `Index` bir hizmet.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta, aşağıdaki hizmetleri, araçları ve verileri kullanılır. 

[Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu Hızlı Başlangıç için ücretsiz bir hizmet kullanabilirsiniz.

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), herhangi bir sürümü. Örnek kodu ve yönergeleri ücretsiz Community edition üzerinde test edilmiştir.

[DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) örnek çözüm, yazılan bir .NET Core konsol uygulaması sağlar C#, Azure örnekleri GitHub deposunda bulunur. İndirin ve çözüm ayıklayın. Varsayılan olarak, çözümleri salt okunurdur. Çözüme sağ tıklayın ve dosyalarda değişiklik yapabilir, böylece salt okunur özniteliğini kaldırın. Verilerin çözümde dahil edilir.

## <a name="get-a-key-and-url"></a>Bir anahtarı ve URL alma

Hizmete çağrı, bir URL uç noktası ve her istekte bir erişim anahtarı gerektirir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinizdeki **genel bakış** sayfa olduğunda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

2. İçinde **ayarları** > **anahtarları**, hizmette tam haklarına yönelik bir yönetici anahtarını alın. Bir gece yarısında gerektiği durumlarda iş sürekliliği için sağlanan iki birbirinin yerine yönetici anahtarı mevcuttur. Ekleme, değiştirme ve silme nesneler için istekleri birincil veya ikincil anahtar kullanabilirsiniz.

![Bir HTTP uç noktası ve erişim anahtarını alma](media/search-fiddler/get-url-key.png "bir HTTP uç noktası ve erişim anahtarını alma")

Tüm istekleri hizmete gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="1---configure-and-build"></a>1 - yapılandırın ve oluşturun

1. Açık **DotNetHowTo.sln** dosyasını Visual Studio'da.

1. AppSettings.JSON dosyasındaki varsayılan örnek ile içerik değiştirin ve ardından yönetici ve hizmet adını sağlayın api anahtarını hizmetiniz için. 


   ```json
   {
       "SearchServiceName": "Put your search service name here (not the full URL)",
       "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
    }
   ```

  Hizmet adı için yalnızca adı gerekir. Örneğin, URL ise https://mydemo.search.windows.net, ekleme `mydemo` JSON dosyasına.

1. Çözümü derleyin ve konsol uygulamasını çalıştırmak için F5 tuşuna basın. Bu alıştırmada ve izleyin, kalan bu kodu nasıl çalıştığına ilişkin bir araştırma aynıdır. 

Alternatif olarak, başvurabilirsiniz [bir .NET uygulamasından Azure Search kullanma](search-howto-dotnet-sdk.md) kapsamı SDK davranışlarının ayrıntılı için. 

<a name="CreateSearchServiceClient"></a>

## <a name="2---create-a-client"></a>2 - bir istemci oluşturma

Azure Search .NET SDK'sını kullanmaya başlamak için bir örneğini oluşturmak `SearchServiceClient` sınıfı. Bu sınıfın birkaç oluşturucusu vardır. İstediğiniz oluşturucu, arama hizmeti adınızı ve `SearchCredentials` nesnesini parametre olarak alır. `SearchCredentials`, api anahtarınızı sarmalar.

Aşağıdaki kodu Program.cs dosyasında bulunabilir. Yeni bir oluşturur `SearchServiceClient` arama hizmeti adına ve uygulamanın yapılandırma dosyasında (appsettings.json) depolanan API anahtarı için değerleri kullanarak.

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

`SearchServiceClient`, `Indexes` özelliğine sahiptir. Bu özellik Azure Search dizinlerini oluşturmanız, listelemeniz, güncelleştirmeniz veya silmeniz için gereken tüm yöntemleri sağlar.

> [!NOTE]
> `SearchServiceClient` sınıfı, arama hizmetinize yönelik bağlantıları yönetir. Çok fazla bağlantı açmayı önlemek için, mümkünse uygulamanızda tek bir `SearchServiceClient` örneği paylaşmaya çalışmanız gerekir. Yöntemlerinin iş parçacığı bu tür paylaşımları etkinleştirmek için güvenlidir.
> 
> 

<a name="DefineIndex"></a>

## <a name="3---construct-index"></a>3 - dizin oluşturun
Tek bir çağrı `Indexes.Create` yöntemi, bir dizin oluşturur. Bu yöntem parametre olarak alır bir `Index` bir Azure Search dizininizi tanımlayan nesne. Oluşturma bir `Index` nesne ve şu şekilde başlatın:

1. `Name` nesnesinin `Index` özelliğini dizin adınız olarak ayarlayın.

2. `Fields` nesnesinin `Index` özelliğini `Field` nesnelerinin dizisi olarak ayarlayın. `Field` nesnelerini oluşturmanın en kolay yolu, `FieldBuilder.BuildForType` metodunu çağırmak ve tür parametresi için bir model sınıfı iletmektir. Bir model sınıfında dizininizin alanlarıyla eşlenen özellikler mevcuttur. Bu arama dizininizdeki belgeleri model sınıfınızın örneklerine bağlamanızı sağlar.

> [!NOTE]
> Bir model sınıfı kullanmayı planlamıyorsanız, `Field` nesnelerini doğrudan oluşturarak dizininizi tanımlayabilirsiniz. Oluşturucuya alan adını veri türü (veya dize alanları çözümleyicisi) ile birlikte sağlayabilirsiniz. Gibi diğer özellikleri de ayarlayabilirsiniz `IsSearchable`, `IsFilterable`, birkaçıdır.
>
>

Arama kullanıcı deneyiminizi ve iş gereksinimlerinizi dizininizi tasarlarken göz önünde bulundurmanız önemlidir. Her alanı atanmalıdır [öznitelikleri](https://docs.microsoft.com/rest/api/searchservice/Create-Index) hangi arama özelliklerinin (filtreleme, modelleme, sıralama ve benzeri) denetim hangi alanlar için geçerlidir. Açıkça ayarlamadığınız her özellik, ilgili arama özelliğini özellikle etkinleştirmediğiniz sürece `Field` sınıfı tarafından devre dışı varsayılır.

Bu örnekte, "hotels" dizini adıdır ve alan bir model sınıfı kullanarak tanımlanır. Model sınıfının her özelliği karşılık gelen dizin alanının aramayla ilgili davranışlarını belirleyen özniteliklere sahiptir. Model sınıfı şu şekilde tanımlanır:

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

Her bir özellik için öznitelikleri, bunların bir uygulamada nasıl kullanılacağını düşünerek dikkatle seçtik. Örneğin, oteller için arama yapan kişiler büyük olasılıkla `description` alanındaki anahtar sözcük eşleşmeleri ile ilgilenecektir. Bu nedenle, `Description` özelliğine `IsSearchable` özniteliğini ekleyerek bu alan için tam metin aramasını etkinleştiririz.

Lütfen `Key` özniteliğini eklediğinizde ayarladığınızda, `string` türündeki dizininizde yalnızca bir alanın *anahtar* alanı olarak belirlenmesi gerektiğini unutmayın (yukarıdaki örnekte bkz. `HotelId`).

Yukarıdaki dizin tanımı Fransızca metin depolamaya yönelik tasarlandığından, `description_fr` alanı için bir dil çözümleyicisi kullanır. Daha fazla bilgi için [dil Çözümleyicileri eklemek için Azure Search dizini](index-add-language-analyzers.md).

> [!NOTE]
> Varsayılan olarak, model sınıfınızdaki her özelliğin adı dizin alan adına karşılık gelir. Tüm özellik adlarını ortası büyük alan adlarıyla eşlemek isterseniz sınıfı `SerializePropertyNamesAsCamelCase` özniteliğiyle işaretleyin. Farklı bir ada eşlemek isterseniz yukarıdaki `DescriptionFr` özelliği gibi `JsonProperty` özniteliğini kullanabilirsiniz. `JsonProperty` özniteliği `SerializePropertyNamesAsCamelCase` özniteliğinden önceliklidir.
> 
> 

Bir model sınıfı tanımladık, şimdi bir dizin tanımını kolayca oluşturabilirsiniz:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = FieldBuilder.BuildForType<Hotel>()
};
```

## <a name="4---call-indexescreate"></a>4 - Indexes.Create çağırın
Artık başlatılan bir sahip olduğunuza göre `Index` nesne, dizin çağırarak denetlediği oluşturma `Indexes.Create` üzerinde `SearchServiceClient` nesnesi:

```csharp
serviceClient.Indexes.Create(definition);
```

Başarılı bir istek için yöntem normal olarak döndürür. İstekle ilgili bir sorun varsa (geçersiz bir parametre gibi) yöntem `CloudException` atar.

Bir dizin ile işiniz bittiğinde ve bunu silmek istediğinizde, çağrı `Indexes.Delete` yöntemi, `SearchServiceClient`. Örneğin:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [!NOTE]
> Bu makaledeki örnek kod, kolaylık amacıyla Azure Search .NET SDK'sının zaman uyumlu yöntemlerini kullanır. Kendi uygulamalarınızda zaman uyumsuz yöntemler kullanarak bunları ölçeklenebilir ve esnek tutmanızı öneririz. Örneğin, yukarıdaki örneklerde `Create` ve `Delete` yerine `CreateAsync` ve `DeleteAsync` kullanabilirsiniz.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, alan veri türleri ve davranışları tanımlar bir şemasını temel alan boş bir Azure Search dizini oluşturuldu. Dizin, bir ad ve öznitelikli alanlar koleksiyonu oluşan bir "tam kemikler" dizindir. Daha gerçekçi bir dizini gibi diğer öğeleri içerir [Puanlama profilleri](index-add-scoring-profiles.md), [öneri Araçları](index-add-suggesters.md) typeahead desteği [eş anlamlılar](search-synonyms.md)ve büyük olasılıkla [ Özel çözümleyiciler](index-add-custom-analyzers.md). Temel iş akışı anladıktan sonra bu özellikleri yeniden ziyaret öneririz.

Bu serideki sonraki hızlı dizini ile aranabilir içeriği yüklemek nasıl etkinleştireceğinizi de açıklar.

> [!div class="nextstepaction"]
> [Kullanarak bir Azure Search dizini için veri yüklemeC#](search-import-data-dotnet.md)