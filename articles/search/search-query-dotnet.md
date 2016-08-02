<properties
    pageTitle=".NET SDK kullanarak Azure Search Dizininizi sorgulama | Microsoft Azure | Barındırılan bulut arama hizmeti"
    description="Azure Search'te bir arama sorgusu oluşturun ve arama sonuçlarını filtrelemek ve sıralamak için arama parametrelerini kullanın."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="05/23/2016"
    ms.author="brjohnst"/>

# .NET SDK kullanarak Azure Search dizininizi sorgulama
> [AZURE.SELECTOR]
- [Genel Bakış](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

Bu makale, [Azure Search .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)'sını kullanarak bir dizinin nasıl sorgulanacağını gösterir.

Bu kılavuzda başlamadan önce, [Azure Search dizini oluşturmuş](search-what-is-an-index.md) ve [bunu verilerle doldurmuş](search-what-is-data-import.md) olmanız gerekir.

Bu makaledeki örnek kodun tamamının C# dilinde yazıldığını unutmayın. Tam kaynak kodunu [GitHub](http://aka.ms/search-dotnet-howto)'da bulabilirsiniz.

## I. Azure Search hizmet sorgunuzun api anahtarını tanımlama
Artık bir Azure Search dizini oluşturduğunuza göre, .NET SDK kullanarak sorgu göndermeye neredeyse hazırsınız. Öncelikle, sağladığınız arama hizmeti için oluşturulan sorgu api anahtarlarından birini edinmeniz gerekir. .NET SDK, hizmetinize yönelik her istek için bu api anahtarını gönderir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

1. Hizmetinizin api anahtarlarını bulmak için [Azure Portal](https://portal.azure.com/)'da oturum açmanız gerekir
2. Azure Search hizmetinizin dikey penceresine gidin
3. "Anahtarlar" simgesine tıklayın

Hizmetiniz, *yönetici anahtarlarına* ve *sorgu anahtarlarına* sahiptir.

  - Birincil ve ikincil *yönetici anahtarlarınız*; hizmeti yönetme, dizinler, dizin oluşturucular ve veri kaynakları ekleme ve silme de dahil olmak üzere her türlü işlem için tüm hakları verir. Birincil anahtarı yeniden oluşturmaya karar verirseniz ikincil anahtarı kullanmaya devam edebilmeniz ve tam tersini yapabilmeniz için iki anahtar vardır.
  - *Sorgu anahtarları*, dizinler ve belgeler için salt okunur erişim verir ve genellikle, arama istekleri gönderen istemci uygulamalarına dağıtılır.

Bir dizini sorgulama amacıyla, sorgu anahtarlarınızdan birini kullanabilirsiniz. Yönetici anahtarlarınız da sorgular için kullanılabilir ancak uygulama kodunuzda bir sorgu anahtarı kullanmanız gerekir. Böylece [En az ayrıcalık prensibi](https://en.wikipedia.org/wiki/Principle_of_least_privilege) daha iyi takip edilmiş olur.

## II. SearchIndexClient sınıfının bir örneğini oluşturma
Azure Search .NET SDK'sıyla sorgu yürütmek için `SearchIndexClient` sınıfının bir örneğini oluşturmanız gerekir. Bu sınıfın birkaç oluşturucusu vardır. İstediğiniz oluşturucu, arama hizmeti adınızı, dizin adınızı ve `SearchCredentials` nesnesini parametre olarak alır. `SearchCredentials` api anahtarınızı sarmalar.

Aşağıdaki kod, uygulamanın yapılandırma dosyasında (`app.config` veya `web.config`) depolanan arama hizmeti adı ve api anahtarı için değerleri kullanarak "hotels" dizini ([.NET SDK kullanarak Azure Search dizini oluşturma](search-create-index-dotnet.md)'da oluşturuldu) için yeni bir `SearchIndexClient` oluşturur:

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string queryApiKey = ConfigurationManager.AppSettings["SearchServiceQueryApiKey"];

SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
```

`SearchIndexClient` bir `Documents` özelliğine sahiptir. Bu özellik Azure Search dizinlerini sorgulamanız için gereken tüm yöntemleri sağlar.

## III. Dizininizi sorgulama
.NET SDK ile arama, `SearchIndexClient` öğenizde `Documents.Search` yöntemini çağırmak kadar kolaydır. Bu yöntem, arama metni dahil olmak üzere sorguyu daha da ayrıntılandırmak için kullanılabilecek bir `SearchParameters` nesnesiyle birlikte birkaç parametre alır.

#### Sorgu Türleri
Kullanacağınız iki ana [sorgu türü](search-query-overview.md#types-of-queries) `search` ve `filter` sorgularıdır. `search` sorgusu, dizininizdeki tüm _aranabilir_ alanlarda bir veya daha çok terimi arar. `filter` sorgusu, bir dizindeki tüm _filtrelenebilir_ alanlarda bir boole ifadesini değerlendirir.

Arama ve filtrelerin her ikisi de `Documents.Search` yöntemi kullanılarak gerçekleştirilir. Bir arama sorgusu `searchText` parametresinde geçirilebilirken bir filtre ifadesi `SearchParameters` sınıfının `Filter` özelliğinden geçirilebilir. Arama yapmadan filtrelemek üzere `searchText` parametresi için `"*"` geçirmeniz yeterlidir. Filtrelemeden arama yapmak için `Filter` özelliğini ayarlamadan bırakmanız veya bir `SearchParameters` örneği geçirmemeniz yeterlidir.

#### Örnek Sorgular

Aşağıdaki örnek kod, [.NET SDK kullanarak Azure Search dizini oluşturma](search-create-index-dotnet.md#DefineIndex)'da tanımlanan "hotels" dizinini sorgulamanın birkaç farklı yolunu gösterir. Arama sonuçlarıyla döndürülen belgelerin, [.NET SDK kullanarak Azure Search'te Veri İçeri Aktarma](search-import-data-dotnet.md#HotelClass)'da tanımlanan `Hotel` sınıfının örnekleri olduğuna dikkat edin. Örnek kod, arama sonuçlarını konsola çıkarmak için bir `WriteDocuments` yöntemini kullanır. Bu yöntem bir sonraki bölümde açıklanmaktadır.

```csharp
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
```

## IV. Arama sonuçlarını işleme
`Documents.Search` yöntemi, sorgunun sonuçlarını içeren bir `DocumentSearchResult` nesnesini döndürür. Önceki bölümdeki örnek, arama sonuçlarını konsola çıkarmak için `WriteDocuments` adlı bir yöntemi kullanır:

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

Önceki bölümdeki sorguların sonuçları, "hotels" dizininin, [.NET SDK kullanarak Azure Search'te Veri İçeri Aktarma](search-import-data-dotnet.md)'da verilen örnek verilerle doldurulduğu varsayıldığında şöyledir:

```
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

```

Yukarıdaki örnek kod, arama sonuçlarını çıkarmak için konsolu kullanır. Benzer şekilde, kendi uygulamanızda arama sonuçlarını göstermeniz gerekir. ASP.NET MVC tabanlı bir web uygulamasında arama sonuçlarının nasıl işlendiğine bir örnek için [GitHub'daki bu örneğe](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) bakın.



<!--HONumber=Jun16_HO2-->


