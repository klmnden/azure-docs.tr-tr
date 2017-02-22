---
title: ".NET SDK kullanarak Azure Search dizini oluşturma | Microsoft Belgeleri"
description: "Azure Search .NET SDK&quot;sını kullanarak kod içinde bir dizin oluşturun."
services: search
documentationcenter: 
author: brjohnstmsft
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 3a851647-fc7b-4fb6-8506-6aaa519e77cd
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 01/13/2017
ms.author: brjohnst
translationtype: Human Translation
ms.sourcegitcommit: 1f06a7197cc1a6dcf7a39c91183a4317bef126bb
ms.openlocfilehash: 3a5131323f438109d94137cb4f577054ec13227f


---
# <a name="create-an-azure-search-index-using-the-net-sdk"></a>.NET SDK'yı kullanarak Azure Search dizini oluşturma
> [!div class="op_single_selector"]
> * [Genel Bakış](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Bu makale, [Azure Search .NET SDK](https://aka.ms/search-sdk)'sını kullanarak Azure Search [dizini](https://docs.microsoft.com/rest/api/searchservice/Create-Index) oluşturma işlemi konusunda size yol gösterecektir.

Bu kılavuzu izlemeden ve dizin oluşturmadan önce, [Azure Search hizmeti oluşturmuş](search-create-service-portal.md) olmanız gerekir.

Bu makaledeki örnek kodun tamamının C# dilinde yazıldığını unutmayın. Tam kaynak kodunu [GitHub](http://aka.ms/search-dotnet-howto)'da bulabilirsiniz.

## <a name="identify-your-azure-search-services-admin-api-key"></a>Azure Search hizmet yöneticinizin api anahtarını tanımlama
Şimdi bir Azure Search hizmeti sağlamış olduğunuza göre, .NET SDK'yı kullanarak hizmet uç noktanıza istek göndermeye neredeyse hazırsınız. Öncelikle, sağladığınız arama hizmeti için oluşturulan yönetici api anahtarlarından birini edinmeniz gerekir. .NET SDK, hizmetinize yönelik her istek için bu api anahtarını gönderir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

1. Hizmetinizin api anahtarlarını bulmak için [Azure portalında](https://portal.azure.com/) oturum açın
2. Azure Search hizmetinizin dikey penceresine gidin
3. "Anahtarlar" simgesine tıklayın

Hizmetiniz, *yönetici anahtarlarına* ve *sorgu anahtarlarına* sahiptir.

* Birincil ve ikincil *yönetici anahtarlarınız*; hizmeti yönetme, dizinler, dizin oluşturucular ve veri kaynakları ekleme ve silme de dahil olmak üzere her türlü işlem için tüm hakları verir. Birincil anahtarı yeniden oluşturmaya karar verirseniz ikincil anahtarı kullanmaya devam edebilmeniz ve tam tersini yapabilmeniz için iki anahtar vardır.
* *Sorgu anahtarları*, dizinler ve belgeler için salt okunur erişim verir ve genellikle, arama istekleri gönderen istemci uygulamalarına dağıtılır.

Dizin oluşturma amacıyla, birincil ya da ikincil yönetici anahtarınızı kullanabilirsiniz.

<a name="CreateSearchServiceClient"></a>

## <a name="create-an-instance-of-the-searchserviceclient-class"></a>SearchServiceClient sınıfının örneğini oluşturma
Azure Search .NET SDK'sını kullanmaya başlamak için `SearchServiceClient` sınıfının bir örneğini oluşturmanız gerekir. Bu sınıfın birkaç oluşturucusu vardır. İstediğiniz oluşturucu, arama hizmeti adınızı ve `SearchCredentials` nesnesini parametre olarak alır. `SearchCredentials`, api anahtarınızı sarmalar.

Aşağıdaki kod, uygulamanın yapılandırma dosyasında (`app.config` veya `web.config`) depolanan arama hizmeti adı ve api anahtarı için değerler kullanarak yeni bir `SearchServiceClient` oluşturur:

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient`, `Indexes` özelliğine sahiptir. Bu özellik Azure Search dizinlerini oluşturmanız, listelemeniz, güncelleştirmeniz veya silmeniz için gereken tüm yöntemleri sağlar.

> [!NOTE]
> `SearchServiceClient` sınıfı, arama hizmetinize yönelik bağlantıları yönetir. Çok fazla bağlantı açmayı önlemek için, mümkünse uygulamanızda tek bir `SearchServiceClient` örneği paylaşmaya çalışmanız gerekir. Yöntemlerinin iş parçacığı bu tür paylaşımları etkinleştirmek için güvenlidir.
> 
> 

<a name="DefineIndex"></a>

## <a name="define-your-azure-search-index"></a>Azure Search dizininizi tanımlama
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
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [Key]
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

    // ToString() method omitted for brevity...
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

## <a name="create-the-index"></a>Dizini oluşturma
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




<!--HONumber=Feb17_HO3-->


