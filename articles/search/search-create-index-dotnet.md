---
title: .NET API - Azure Search kullanarak kod içinde bir dizin oluşturun
description: Azure Search .NET SDK'sını kullanarak bir tam metin arama yapılabilir bir dizin oluşturmayı öğrenin ve C# örnek kodu.
author: brjohnstmsft
manager: jlembicz
tags: azure-portal
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/14/2019
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: e0aa7dce0b3cc4609a995097d8e70f8ec4336809
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226377"
---
# <a name="quickstart-create-load-and-query-an-azure-search-index-using-the-net-sdk"></a>Hızlı Başlangıç: .NET SDK kullanarak Azure Search dizini sorgulama oluşturma ve yükleme
> [!div class="op_single_selector"]
> * [C#](search-create-index-dotnet.md)
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [Postman (REST)](search-fiddler.md)
> * [Portal](search-create-index-portal.md)
> 

Bu makalede bir Azure Search sorgulama oluşturma ve yükleme işleminde size yol gösterecektir [dizin](search-what-is-an-index.md) kullanarak [Azure Search .NET SDK'sı](https://aka.ms/search-sdk).

> [!NOTE]
> Bu makaledeki örnek kodun tamamı C# dilinde yazılmıştır. Tam kaynak kodunu [GitHub](https://aka.ms/search-dotnet-howto)'da bulabilirsiniz. Ayrıca, örnek kodla ilgili daha ayrıntılı yönergeler için [Azure Search .NET SDK’sı](search-howto-dotnet-sdk.md) hakkındaki yazıları da okuyabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

+ [Azure Search hizmeti oluşturma](search-create-service-portal.md). Bu Hızlı Başlangıç için ücretsiz bir hizmet kullanabilirsiniz.

+ [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), herhangi bir sürümü. Örnek kodu ve yönergeleri ücretsiz Community edition üzerinde test edilmiştir.

+ URL uç noktasını ve yönetici, arama hizmetinizin api anahtarı. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

  1. Azure portalında [hizmetinizi bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Hizmet listesinde.

  2. İçinde **genel bakış**, URL'yi alın. Örnek uç nokta `https://my-service-name.search.windows.net` şeklinde görünebilir.

  3. İçinde **ayarları** > **anahtarları**, hizmette tam haklarına yönelik bir yönetici anahtarını alın. Bir gece yarısında gerektiği durumlarda iş sürekliliği için sağlanan iki birbirinin yerine yönetici anahtarı mevcuttur. İsteğiniz üzerine ya da birincil veya ikincil anahtarı kullanabilirsiniz.

     ![Bir HTTP uç noktası ve erişim anahtarını alma](media/search-fiddler/get-url-key.png "bir HTTP uç noktası ve erişim anahtarını alma")

     Tüm istekleri hizmete gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="1---create-a-new-project"></a>1 - yeni proje oluşturma

Visual Studio'da yeni bir görsel oluşturun C# proje. Bu Hızlı Başlangıç için iyi bir şablon görsel ise C# > kullanmaya başlayın > Web uygulaması. Bu şablon, appsettings.json dosyasını sağlar.  

AppSettings.JSON dosyasındaki varsayılan örnek ile içerik değiştirin ve ardından yönetici ve hizmet adını sağlayın api anahtarını hizmetiniz için. Hizmet adı için yalnızca adı gerekir. Örneğin, URL ise https://mydemo.search.windows.net, ekleme `mydemo` JSON dosyasına.


```json
{
    "SearchServiceName": "Put your search service name here",
    "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
}
```

<a name="CreateSearchServiceClient"></a>

## <a name="2---create-an-instance-of-the-searchserviceclient-class"></a>2 - SearchServiceClient sınıfının bir örneğini oluşturma
Azure Search .NET SDK'sını kullanmaya başlamak için `SearchServiceClient` sınıfının bir örneğini oluşturmanız gerekir. Bu sınıfın birkaç oluşturucusu vardır. İstediğiniz oluşturucu, arama hizmeti adınızı ve `SearchCredentials` nesnesini parametre olarak alır. `SearchCredentials`, api anahtarınızı sarmalar.

Aşağıdaki kodu Program.cs dosyasına kopyalayın. Aşağıdaki kod yeni bir oluşturur `SearchServiceClient` arama hizmeti adına ve uygulamanın yapılandırma dosyasında (appsettings.json) depolanan API anahtarı için değerleri kullanarak.

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

## <a name="3---define-an-index-schema"></a>3 - bir dizin şemasını tanımlama
`Indexes.Create` yöntemine yönelik tek bir çağrı dizininizi oluşturur. Bu yöntem, Azure Search dizininizi tanımlayan bir `Index` nesnesini parametre olarak alır. Bir `Index` nesnesi oluşturmanız ve bunu aşağıdaki gibi başlatmanız gerekir:

1. `Name` nesnesinin `Index` özelliğini dizin adınız olarak ayarlayın.

2. `Fields` nesnesinin `Index` özelliğini `Field` nesnelerinin dizisi olarak ayarlayın. `Field` nesnelerini oluşturmanın en kolay yolu, `FieldBuilder.BuildForType` metodunu çağırmak ve tür parametresi için bir model sınıfı iletmektir. Bir model sınıfında dizininizin alanlarıyla eşlenen özellikler mevcuttur. Bu arama dizininizdeki belgeleri model sınıfınızın örneklerine bağlamanızı sağlar.

> [!NOTE]
> Bir model sınıfı kullanmayı planlamıyorsanız, `Field` nesnelerini doğrudan oluşturarak dizininizi tanımlayabilirsiniz. Oluşturucuya alan adını veri türü (veya dize alanları çözümleyicisi) ile birlikte sağlayabilirsiniz. `IsSearchable`, `IsFilterable` vb. gibi başka özellikler de ayarlayabilirsiniz.
>
>

Her bir alan için [uygun özellikler](https://docs.microsoft.com/rest/api/searchservice/Create-Index) atanması gerektiğinden, dizininizi tasarlarken arama kullanıcı deneyiminizi ve iş gereksinimlerinizi göz önünde bulundurmanız önemlidir. Bu özellikler, hangi alanlar için hangi arama özelliklerinin (filtreleme, modelleme, tam metin araması sıralama vb.) geçerli olduğunu denetler. Açıkça ayarlamadığınız her özellik, ilgili arama özelliğini özellikle etkinleştirmediğiniz sürece `Field` sınıfı tarafından devre dışı varsayılır.

Bizim örneğimizde, dizinimizi "oteller" olarak adlandırdık ve alanlarımızı model sınıfı kullanarak tanımladık. Model sınıfının her özelliği karşılık gelen dizin alanının aramayla ilgili davranışlarını belirleyen özniteliklere sahiptir. Model sınıfı şu şekilde tanımlanır:

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

Yukarıdaki dizin tanımı Fransızca metin depolamaya yönelik tasarlandığından, `description_fr` alanı için bir dil çözümleyicisi kullanır. Dil çözümleyicileri hakkında daha fazla bilgi için ilgili [blog yazısının](https://azure.microsoft.com/blog/language-support-in-azure-search/) yanı sıra [Dil desteği konu başlığına](https://docs.microsoft.com/rest/api/searchservice/Language-support) bakın.

> [!NOTE]
> Varsayılan olarak model sınıfınızdaki her özelliğin adı, dizinde karşılık gelen alanın adı olarak kullanılır. Tüm özellik adlarını ortası büyük alan adlarıyla eşlemek isterseniz sınıfı `SerializePropertyNamesAsCamelCase` özniteliğiyle işaretleyin. Farklı bir ada eşlemek isterseniz yukarıdaki `DescriptionFr` özelliği gibi `JsonProperty` özniteliğini kullanabilirsiniz. `JsonProperty` özniteliği `SerializePropertyNamesAsCamelCase` özniteliğinden önceliklidir.
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

## <a name="4---create-the-index-on-the-service"></a>4 - hizmette dizini oluşturma
Şimdi, başlatılan bir `Index` nesneniz olduğuna göre `Indexes.Create` nesneniz üzerinden `SearchServiceClient` çağrısı yaparak dizininizi oluşturabilirsiniz:

```csharp
serviceClient.Indexes.Create(definition);
```

Başarılı bir istek için yöntem normal olarak döndürür. İstekle ilgili bir sorun varsa (geçersiz bir parametre gibi) yöntem `CloudException` atar.

Dizin ile işiniz bittiğinde ve bunu silmek istediğinizde nesneniz `SearchServiceClient` üzerinden `Indexes.Delete` yöntemini çağırın. Örneğin, "oteller" dizinini aşağıdaki şekilde sileriz:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [!NOTE]
> Bu makaledeki örnek kod, kolaylık amacıyla Azure Search .NET SDK'sının zaman uyumlu yöntemlerini kullanır. Kendi uygulamalarınızda zaman uyumsuz yöntemler kullanarak bunları ölçeklenebilir ve esnek tutmanızı öneririz. Örneğin, yukarıdaki örneklerde `Create` ve `Delete` yerine `CreateAsync` ve `DeleteAsync` kullanabilirsiniz.
> 
> 


## <a name="next-steps"></a>Sonraki adımlar
Azure Search dizini oluşturduktan sonra, [içeriğinizi dizine yüklemek](search-what-is-data-import.md) için hazır olursunuz. Böylece, verilerinizi aramaya başlayabilirsiniz.

