---
title: Kod - Azure Search .NET SDK kullanarak dizin sorgulama
description: C#Azure Search'te bir arama sorgusu oluşturmak için kod örneği. Arama sonuçlarını filtrelemek ve sıralamak için arama parametrelerini ekleyin.
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/19/2017
ms.custom: seodec2018
ms.openlocfilehash: 98a857ca0d3ecabc9deebcb177e3548ab02fd3b3
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58201458"
---
# <a name="query-your-azure-search-index-using-the-net-sdk"></a>.NET SDK kullanarak Azure Search dizininizi sorgulama
> [!div class="op_single_selector"]
> * [Genel Bakış](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Bu makale, [Azure Search .NET SDK](https://aka.ms/search-sdk)'sını kullanarak bir dizinin nasıl sorgulanacağını gösterir.

Bu kılavuzda başlamadan önce, [Azure Search dizini oluşturmuş](search-what-is-an-index.md) ve [bunu verilerle doldurmuş](search-what-is-data-import.md) olmanız gerekir.

> [!NOTE]
> Bu makaledeki örnek kodun tamamı C# dilinde yazılmıştır. Tam kaynak kodunu [GitHub](https://aka.ms/search-dotnet-howto)'da bulabilirsiniz. Ayrıca, örnek kodla ilgili daha ayrıntılı yönergeler için [Azure Search .NET SDK’sı](search-howto-dotnet-sdk.md) hakkındaki yazıları da okuyabilirsiniz.

## <a name="identify-your-azure-search-services-query-api-key"></a>Azure Search hizmet sorgunuzun api anahtarını tanımlama
Artık bir Azure Search dizini oluşturduğunuza göre, .NET SDK kullanarak sorgu göndermeye neredeyse hazırsınız. Öncelikle, sağladığınız arama hizmeti için oluşturulan sorgu api anahtarlarından birini edinmeniz gerekir. .NET SDK, hizmetinize yönelik her istek için bu api anahtarını gönderir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

1. Hizmetinizin api anahtarlarını bulmak için [Azure portalında](https://portal.azure.com/) oturum açabilirsiniz
2. Azure Search hizmetinizin dikey penceresine gidin
3. "Anahtarlar" simgesine tıklayın

Hizmetiniz, *yönetici anahtarlarına* ve *sorgu anahtarlarına* sahiptir.

* Birincil ve ikincil *yönetici anahtarlarınız*; hizmeti yönetme, dizinler, dizin oluşturucular ve veri kaynakları ekleme ve silme de dahil olmak üzere her türlü işlem için tüm hakları verir. Birincil anahtarı yeniden oluşturmaya karar verirseniz ikincil anahtarı kullanmaya devam edebilmeniz ve tam tersini yapabilmeniz için iki anahtar vardır.
* *Sorgu anahtarları*, dizinler ve belgeler için salt okunur erişim verir ve genellikle, arama istekleri gönderen istemci uygulamalarına dağıtılır.

Bir dizini sorgulama amacıyla, sorgu anahtarlarınızdan birini kullanabilirsiniz. Yönetici anahtarlarınız da sorgular için kullanılabilir ancak uygulama kodunuzda bir sorgu anahtarı kullanmanız gerekir. Böylece [En az ayrıcalık prensibi](https://en.wikipedia.org/wiki/Principle_of_least_privilege) daha iyi takip edilmiş olur.

## <a name="create-an-instance-of-the-searchindexclient-class"></a>SearchIndexClient sınıfının bir örneğini oluşturma
Azure Search .NET SDK'sıyla sorgu yürütmek için `SearchIndexClient` sınıfının bir örneğini oluşturmanız gerekir. Bu sınıfın birkaç oluşturucusu vardır. İstediğiniz oluşturucu, arama hizmeti adınızı, dizin adınızı ve `SearchCredentials` nesnesini parametre olarak alır. `SearchCredentials`, api anahtarınızı sarmalar.

Aşağıdaki kod, uygulamanın yapılandırma dosyasında ([örnek uygulama](https://aka.ms/search-dotnet-howto) olması durumunda `appsettings.json`) depolanan arama hizmeti adına ve API anahtarına ilişkin değerleri kullanarak "hotels" dizini ([.NET SDK kullanarak Azure Search dizini oluşturma](search-create-index-dotnet.md) bölümünde oluşturuldu) için yeni bir `SearchIndexClient` oluşturur:

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

`SearchIndexClient`, `Documents` özelliğine sahiptir. Bu özellik Azure Search dizinlerini sorgulamanız için gereken tüm yöntemleri sağlar.

## <a name="query-your-index"></a>Dizininizi sorgulama
.NET SDK ile arama, `SearchIndexClient` öğenizde `Documents.Search` yöntemini çağırmak kadar kolaydır. Bu yöntem, arama metni dahil olmak üzere sorguyu daha da ayrıntılandırmak için kullanılabilecek bir `SearchParameters` nesnesiyle birlikte birkaç parametre alır.

#### <a name="types-of-queries"></a>Sorgu Türleri
Kullanacağınız iki ana [sorgu türü](search-query-overview.md#types-of-queries) `search` ve `filter` sorgularıdır. `search` sorgusu, dizininizdeki tüm *aranabilir* alanlarda bir veya daha çok terimi arar. `filter` sorgusu, bir dizindeki tüm *filtrelenebilir* alanlarda bir boole ifadesini değerlendirir. Aramaları ve filtreleri birlikte veya ayrı olarak kullanabilirsiniz.

Arama ve filtrelerin her ikisi de `Documents.Search` yöntemi kullanılarak gerçekleştirilir. Bir arama sorgusu `searchText` parametresinde geçirilebilirken bir filtre ifadesi `SearchParameters` sınıfının `Filter` özelliğinden geçirilebilir. Arama yapmadan filtrelemek üzere `searchText` parametresi için `"*"` geçirmeniz yeterlidir. Filtrelemeden arama yapmak için `Filter` özelliğini ayarlamadan bırakmanız veya bir `SearchParameters` örneği geçirmemeniz yeterlidir.

#### <a name="example-queries"></a>Örnek Sorgular
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

## <a name="handle-search-results"></a>Arama sonuçlarını işleme
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

