---
title: "Verileri karşıya yükleme (.NET - Azure Search) | Microsoft Docs"
description: ".NET SDK kullanarak Azure Search'te bir dizine nasıl veri yükleneceğini öğrenin."
services: search
documentationcenter: 
author: brjohnstmsft
manager: jhubbard
editor: 
tags: 
ms.assetid: 0e0e7e7b-7178-4c26-95c6-2fd1e8015aca
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 01/13/2017
ms.author: brjohnst
ms.openlocfilehash: bdd952869143c6ca6374bb9264db5bcba1f32b50
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="upload-data-to-azure-search-using-the-net-sdk"></a>.NET SDK kullanarak Azure Search'e veri yükleme
> [!div class="op_single_selector"]
> * [Genel Bakış](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

Bu makalede, bir Azure Search dizinine veri aktarmak için [Azure Search .NET SDK](https://aka.ms/search-sdk)'sının nasıl kullanılacağı gösterilir.

Bu kılavuza başlamadan önce bir [Azure Search dizini oluşturmuş](search-what-is-an-index.md) olmanız gerekir. Ayrıca, bu makale [.NET SDK kullanarak Azure Search dizini oluşturma](search-create-index-dotnet.md#CreateSearchServiceClient) başlığı altında gösterildiği şekilde önceden bir `SearchServiceClient` nesnesi oluşturduğunuzu varsayar.

> [!NOTE]
> Bu makaledeki örnek kodun tamamı C# dilinde yazılmıştır. Tam kaynak kodunu [GitHub](http://aka.ms/search-dotnet-howto)'da bulabilirsiniz. Ayrıca, örnek kodla ilgili daha ayrıntılı yönergeler için [Azure Search .NET SDK’sı](search-howto-dotnet-sdk.md) hakkındaki yazıları da okuyabilirsiniz.

.NET SDK kullanarak dizininize belge göndermek için şunu yapmanız gerekir:

1. Arama dizininize bağlamak için bir `SearchIndexClient` nesnesi oluşturun.
2. Eklenecek, değiştirilecek veya silinecek belgeleri içeren bir `IndexBatch` oluşturun.
3. Arama dizininize `IndexBatch` göndermek için `SearchIndexClient` öğenizin `Documents.Index` yöntemini çağırın.

## <a name="create-an-instance-of-the-searchindexclient-class"></a>SearchIndexClient sınıfının bir örneğini oluşturma
Azure Search .NET SDK'sını kullanarak dizininize veri aktarmak için `SearchIndexClient` sınıfının bir örneğini oluşturmanız gerekir. Bu örneği kendiniz oluşturulabilirsiniz ancak bunun `Indexes.GetClient` yöntemini çağırmak için bir `SearchServiceClient` örneğine zaten sahipseniz daha kolay olur. Örneğin, `serviceClient` adlı bir `SearchServiceClient` öğesinden "hotels" adlı bir dizin için şu şekilde bir `SearchIndexClient` elde edebilirsiniz:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> Genel bir arama uygulamasında, dizin yönetimi ve popülasyon, arama sorgularından ayrı bir bileşen tarafından işlenir. `Indexes.GetClient`, başka bir `SearchCredentials` sağlamanızı gerektirmediğinden, dizini doldurmak için uygundur. Yeni `SearchIndexClient` için `SearchServiceClient` oluşturmak üzere kullandığınız yönetici anahtarını geçirerek bunu yapar. Ancak uygulamanızın sorguları yürüten bölümünde, bir yönetici anahtarı yerine sorgu anahtarında geçirebilmeniz için doğrudan `SearchIndexClient` oluşturmak daha iyi olur. Bu, [en az ayrıcalık ilkesiyle](https://en.wikipedia.org/wiki/Principle_of_least_privilege) tutarlıdır ve uygulamanızı daha güvenli hale getirmenize yardımcı olur. [Azure Search REST API'si başvurusunda](https://docs.microsoft.com/rest/api/searchservice/) yönetici anahtarları ve sorgu anahtarları hakkında daha fazla bilgi edinebilirsiniz.
> 
> 

`SearchIndexClient`, `Documents` özelliğine sahiptir. Bu özellik, belgeleri dizininize eklemek, bunları değiştirmek, silmek veya sorgulamak için ihtiyacınız olan tüm yöntemleri sağlar.

## <a name="decide-which-indexing-action-to-use"></a>Hangi dizin oluşturma eyleminin kullanılacağına karar verme
.NET SDK kullanarak veri içeri aktarmak için verilerinizi bir `IndexBatch` nesnesine paketlemeniz gerekir. Bir `IndexBatch`, her birinde bir belge ve söz konusu belgede hangi eylemin (karşıya yükleme, birleştirme, silme, vb.) gerçekleştirileceğini Azure Search'e söyleyen bir özellik bulunan `IndexAction` nesneleri koleksiyonunu kapsar. Yukarıdaki eylemlerden hangisini seçtiğinize bağlı olarak, her bir belgeye yalnızca belirli alanlar dahil edilmelidir:

| Eylem | Açıklama | Her bir belge için gerekli alanlar | Notlar |
| --- | --- | --- | --- |
| `Upload` |Bir `Upload` eylemi, belgenin yeni olması durumunda ekleneceği ve var olması durumunda güncelleştirileceği/değiştirileceği bir "upsert" ile benzerlik gösterir. |anahtar ve tanımlamak istediğiniz diğer alanlar |Var olan bir belgeyi güncelleştirirken/değiştirirken istekte belirtilmeyen herhangi bir alan `null` olarak ayarlanır. Bu durum, alan daha önce değersiz olmayan bir değere ayarlanmış olsa dahi gerçekleşir. |
| `Merge` |Var olan belgeyi belirtilen alanlarla güncelleştirir. Belge dizinde mevcut değilse birleştirme işlemi başarısız olur. |anahtar ve tanımlamak istediğiniz diğer alanlar |Birleştirmede belirttiğiniz herhangi bir alan belgede var olan alanın yerini alır. Buna `DataType.Collection(DataType.String)` türünde alanlar dahildir. Örneğin, belge `["budget"]` değerine sahip bir `tags` alanını içeriyorsa ve `tags` için `["economy", "pool"]` değeriyle bir birleştirme yürütürseniz `tags` alanının son değeri `["economy", "pool"]` olur. `["budget", "economy", "pool"]` olmayacaktır. |
| `MergeOrUpload` |Belirtilen anahtara sahip bir belge dizinde zaten mevcutsa bu eylem `Merge` gibi davranır. Belge mevcut değilse yeni bir belgeyle `Upload` gibi davranır. |anahtar ve tanımlamak istediğiniz diğer alanlar |- |
| `Delete` |Belirtilen belgeyi dizinden kaldırır. |yalnızca anahtar |Anahtar alanı dışında belirttiğiniz tüm alanlar yoksayılır. Bir belgeden tek bir alanı kaldırmak istiyorsanız bunun yerine `Merge` kullanıp alanı açık bir şekilde null olarak ayarlamanız yeterlidir. |

Bir sonraki bölümde gösterildiği üzere, `IndexBatch` ve `IndexAction` sınıflarının çeşitli statik yöntemleriyle hangi eylemi kullanmak istediğinizi belirtebilirsiniz.

## <a name="construct-your-indexbatch"></a>IndexBatch'inizi oluşturma
Artık belgelerinizde hangi eylemleri gerçekleştireceğinizi bildiğinize göre, `IndexBatch` oluşturmaya hazırsınız. Aşağıdaki örnek, bir toplu işlemin birkaç farklı eylemle nasıl oluşturulacağını gösterir. Örneğimizde "hotels" dizinindeki bir belgeyle eşlenen `Hotel` adlı özel bir sınıf kullandığımıza dikkat edin.

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
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
            }),
        IndexAction.Upload(
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
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

Bu durumda, `IndexAction` sınıfında çağrılan yöntemler tarafından belirtildiği üzere, arama eylemlerimiz olarak `Upload`, `MergeOrUpload` ve `Delete` kullanıyoruz.

Bu "hotels" dizini örneğinin, birçok belgeyle önceden doldurulduğunu varsayın. `MergeOrUpload` kullanırken olası tüm belge alanlarını belirtmek zorunda kalmadığımıza ve `Delete` kullanırken yalnızca belge anahtarını (`HotelId`) belirttiğimize dikkat edin.

Ayrıca, tek bir dizin oluşturma isteğine yalnızca en fazla 1000 belge dahil edebileceğinizi unutmayın.

> [!NOTE]
> Bu örnekte, farklı belgelere farklı eylemler uyguluyoruz. Toplu işlemdeki tüm belgelerde aynı eylemleri gerçekleştirmek istiyorsanız `IndexBatch.New` çağırmak yerine `IndexBatch` öğesinin diğer statik yöntemlerini kullanabilirsiniz. Örneğin; `IndexBatch.Merge`, `IndexBatch.MergeOrUpload` ve `IndexBatch.Delete` çağırarak toplu işlemler oluşturabilirsiniz. Bu yöntemler, `IndexAction` nesneleri yerine bir belge koleksiyonu (bu örnekte `Hotel` türünde nesneler) alır.
> 
> 

## <a name="import-data-to-the-index"></a>Dizine veri aktarma
Artık başlatılan bir `IndexBatch` nesneniz olduğuna göre `SearchIndexClient` nesneniz üzerinden `Documents.Index` çağrısı yaparak bunu dizininize gönderebilirsiniz. Aşağıdaki örnekte, nasıl `Index` çağrılacağının yanı sıra gerçekleştirmeniz gereken bazı ek adımlar gösterilmektedir:

```csharp
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
```

`try`/`catch` öğesinin, `Index` yöntemine yönelik çağrıyı çevrelediğine dikkat edin. Catch bloğu, dizin oluşturma için önemli bir hata durumunu işler. Azure Search hizmetiniz toplu işlemdeki belgelerin bazılarına dizin oluşturmada başarısız olursa `Documents.Index` tarafından bir `IndexBatchException` oluşturulur. Bu durum, hizmetiniz ağır yük altındayken belgelere dizin oluşturuyorsanız oluşabilir. **Bu durumu, kodunuzda açık şekilde işlemenizi kesinlikle öneririz.** Başarısız olan belgelere dizin oluşturmayı geciktirip sonra yeniden deneyebilir veya günlük tutup örneğin devam ettiği şekilde devam edebilir veya uygulamanızın veri tutarlılığı gereksinimlerine bağlı olarak başka bir şey yapabilirsiniz.

Son olarak, yukarıdaki örnekteki kod iki saniye gecikir. Azure Search hizmetinizde dizin oluşturma uyumsuz şekilde meydana gelir; bu nedenle belgelerin aramada kullanılabilir olduğundan emin olmak için örnek uygulamanızın kısa bir süre beklemesi gerekir. Bu gibi gecikmeler genellikle yalnızca gösterilerde, testlerde ve örnek uygulamalarda gereklidir.

<a name="HotelClass"></a>

### <a name="how-the-net-sdk-handles-documents"></a>.NET SDK belgeleri nasıl işler?
Azure Search .NET SDK'sının `Hotel` gibi kullanıcı tanımlı bir sınıfın örneklerini dizine nasıl yükleyebildiğini merak ediyor olabilirsiniz. Bu sorunun yanıtlanmasına yardımcı olmak için [.NET SDK kullanarak Azure Search dizini oluşturma](search-create-index-dotnet.md#DefineIndex)'da tanımlanan dizin şemasıyla eşlenen `Hotel` sınıfına bakalım:

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

Fark edilecek ilk şey, `Hotel` öğesinin her bir genel özelliğinin dizin tanımındaki bir alana karşılık geldiği ancak çok önemli bir fark olduğudur: `Hotel` öğesinin her bir genel özelliğinin adı büyük harfle ("Pascal büyük/küçük harfi") başlarken her bir alanın adı küçük harfle ("ortası büyük harf") başlar. Bu durum, hedef şemanın uygulama geliştiricisinin denetimi dışında kaldığı bir veri bağlamayı gerçekleştiren .NET uygulamalarında ortak bir senaryodur. Özellik adlarını ortası büyük harf yaparak .NET adlandırma yönergelerini bozmanın yerine, `[SerializePropertyNamesAsCamelCase]` özniteliğiyle SDK'nın özellik adlarını otomatik olarak ortası büyük harfle eşlenmesini söyleyebilirsiniz.

> [!NOTE]
> Azure Search .NET SDK'sı, özel model nesnelerinizi JSON'a ve JSON'dan seri hale getirmek ve seri durumdan çıkarmak için [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) kitaplığını kullanır. Gerekirse bu seri hale getirmeyi özelleştirebilirsiniz. Daha fazla bilgi için bkz. [JSON.NET ile Özel Serileştirme](search-howto-dotnet-sdk.md#JsonDotNet). Bunun bir örneği, yukarıdaki örnek kodda `DescriptionFr` özelliğinde `[JsonProperty]` özniteliğinin kullanılmasıdır.
> 
> 

`Hotel` sınıfı hakkında ikinci önemli şey, genel özelliklerin veri türleridir. Bu özelliklerin .NET türleri, dizin tanımında eşdeğer alan türleriyle eşlenir. Örneğin, `Category` dize özelliği `DataType.String` türündeki `category` alanına eşlenir. `bool?` ve `DataType.Boolean`, `DateTimeOffset?` ve `DataType.DateTimeOffset`, vb. arasında benzer türde eşlemeler bulunur. Tür eşlemesine yönelik belirli kurallar, [Azure Search .NET SDK başvurusundaki](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_) `Documents.Get` yönteminde belirtilmiştir.

Kendi sınıflarınızı belge olarak kullanabilme iki yönde de işe yarar: [Sonraki makalede](search-query-dotnet.md) gösterildiği üzere, arama sonuçlarını da alabilir ve SDK'nın bunları otomatik olarak istediğiniz bir türe seri durumdan çıkarmasını sağlayabilirsiniz.

> [!NOTE]
> Azure Search .NET SDK'sı, alan adlarını alan değerlerine bir anahtar/değer eşlemesi olan `Document` sınıfını kullanarak dinamik tür belirtilmiş belgeleri de destekler. Bu durum, tasarım sırasında dizin şemasını bilmediğiniz veya belirli model sınıflarına bağlamanın kullanışlı olmayacağı senaryolarda kullanışlıdır. SDK'da belgelerle ilgili tüm yöntemler, `Document` sınıfıyla çalışan aşırı yüklerin yanı sıra genel türde bir parametre alan kesin tür belirtilmiş aşırı yüklere de sahiptir. Bu makaledeki örnek kodda yalnızca ikinci durum kullanılır.
> 
> 

**Neden boş değer atanabilir türleri kullanmalısınız?**

Bir Azure Search dizinine eşlemek için yeni model sınıflarınızı tasarlarken, `bool` ve `int` gibi değer türü özelliklerinin boş değer atanabilir (örneğin, `bool` yerine `bool?`) şeklinde bildirilmesini öneririz. Boş değer atanamayan bir özellik kullanırsanız buna karşılık gelen alan için dizininizdeki hiçbir belgenin boş bir değer içermediğini **garanti etmeniz** gerekir. Bunu zorlamanıza ne SDK ne de Azure Search hizmeti yardımcı olur.

Bu yalnızca kuramsal bir sorun değildir: Var olan `DataType.Int32` türünde bir dizine yeni bir alan eklediğiniz bir senaryoyu düşünün. Dizin tanımını güncelleştirdikten sonra, tüm belgelerin bu yeni alan için boş bir değeri olur (bunun nedeni, Azure Search'te tüm türlerin boş değer atanabilir olmasıdır). Ardından bu alan için boş değer atanamayan bir `int` özelliğiyle bir model sınıfı kullanırsanız belgeleri almaya çalışırken bunun gibi bir `JsonSerializationException` alırsınız:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Bu nedenle, en iyi uygulama olarak model sınıflarınızda boş değer atanabilir türler kullanmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar
Azure Search dizininizi doldurduktan sonra, belgeleri aramak için sorgu göndermeye başlamaya hazır olursunuz. Ayrıntılı bilgi için bkz. [Azure Search Dizininizi Sorgulama](search-query-overview.md).

