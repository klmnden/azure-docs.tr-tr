---
title: İçinde bir Azure Search dizinine veri sorgulama C# (.NET SDK'sı) - Azure Search
description: C#Azure Search'te bir arama sorgusu oluşturmak için kod örneği. Arama sonuçlarını filtrelemek ve sıralamak için arama parametrelerini ekleyin.
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/20/2019
ms.openlocfilehash: 1194407122123797c2564c96ac452b9582b017a4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61283390"
---
# <a name="quickstart-3---query-an-azure-search-index-in-c"></a>Hızlı Başlangıç: 3 - Azure Search dizini sorgulamaC#

Bu makale, sorgulama [Azure Search dizini](search-what-is-an-index.md) kullanarak C# ve [.NET SDK'sı](https://aka.ms/search-sdk). Bu görevleri gerçekleştirerek dizininizdeki belgeleri arama gerçekleştirilir:

> [!div class="checklist"]
> * Oluşturma bir [ `SearchIndexClient` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) bir arama dizini salt okunur haklarıyla bağlanmak için nesne.
> * Oluşturma bir [ `SearchParameters` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters?view=azure-dotnet) arama veya filtre tanımını içeren nesne.
> * Çağrı `Documents.Search` metodunda `SearchIndexClient` bir dizine sorgu göndermek için.

## <a name="prerequisites"></a>Önkoşullar

[Azure Search dizini yük](search-import-data-dotnet.md) hotels örnek verilerle.

Belgeler için salt okunur erişim için kullanılan bir sorgu anahtarı edinin. Bir arama hizmeti üzerinde nesneleri ve içeriği oluşturabilmeniz, şimdiye kadar bir yönetici API anahtarını kullandınız. Ancak uygulamalarında sorgu desteği için kesin bir sorgu anahtarı kullanmanızı öneririz. Yönergeler için [bir sorgu anahtarı oluşturma](search-security-api-keys.md#create-query-keys).

## <a name="create-a-client"></a>Bir istemci oluşturma
Bir örneğini oluşturmak `SearchIndexClient` salt okunur erişim için bir sorgu anahtarı verebilir böylece sınıfı (üzerine ağır basıp basmadığını yazma erişim haklarını aksine `SearchServiceClient` Önceki derste kullanılan).

Bu sınıfın birkaç oluşturucusu vardır. Arama hizmeti adınızı, dizin adı istediğinizi alır ve bir [ `SearchCredentials` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchcredentials?view=azure-dotnet) nesnesini parametre olarak. `SearchCredentials`, api anahtarınızı sarmalar.

Aşağıdaki kod yeni bir oluşturur `SearchIndexClient` arama hizmeti adına ve uygulamanın yapılandırma dosyasında depolanan API anahtarı için değerleri kullanarak "hotels" dizini için (`appsettings.json` durumunda [örnek uygulama](https://aka.ms/search-dotnet-howto)):

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

`SearchIndexClient` sahip bir [ `Documents` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient.documents?view=azure-dotnet) özelliği. Bu özellik Azure Search dizinlerini sorgulamanız için gereken tüm yöntemleri sağlar.

## <a name="construct-searchparameters"></a>Yapı kullanılması
.NET SDK ile arama, `SearchIndexClient` öğenizde `Documents.Search` yöntemini çağırmak kadar kolaydır. Bu yöntem, arama metni dahil olmak üzere sorguyu daha da ayrıntılandırmak için kullanılabilecek bir `SearchParameters` nesnesiyle birlikte birkaç parametre alır.

### <a name="types-of-queries"></a>Sorgu Türleri
Kullanacağınız iki ana [sorgu türü](search-query-overview.md#types-of-queries) `search` ve `filter` sorgularıdır. `search` sorgusu, dizininizdeki tüm *aranabilir* alanlarda bir veya daha çok terimi arar. `filter` sorgusu, bir dizindeki tüm *filtrelenebilir* alanlarda bir boole ifadesini değerlendirir. Aramaları ve filtreleri birlikte veya ayrı olarak kullanabilirsiniz.

Arama ve filtrelerin her ikisi de `Documents.Search` yöntemi kullanılarak gerçekleştirilir. Bir arama sorgusu `searchText` parametresinde geçirilebilirken bir filtre ifadesi `SearchParameters` sınıfının `Filter` özelliğinden geçirilebilir. Arama yapmadan filtrelemek üzere `searchText` parametresi için `"*"` geçirmeniz yeterlidir. Filtrelemeden arama yapmak için `Filter` özelliğini ayarlamadan bırakmanız veya bir `SearchParameters` örneği geçirmemeniz yeterlidir.

### <a name="example-queries"></a>Örnek Sorgular
Aşağıdaki örnek kod içinde tanımlanan "hotels" dizinini sorgulamanın birkaç farklı yolunu gösterir [içinde bir Azure Search dizini oluşturma C# ](search-create-index-dotnet.md#DefineIndex). Arama sonuçlarıyla döndürülen belgelerin örnekleri olduğuna dikkat edin `Hotel` sınıfı içinde tanımlanan [içinde bir Azure Search dizinine verileri içeri aktarma C# ](search-import-data-dotnet.md#construct-indexbatch). Örnek kod, arama sonuçlarını konsola çıkarmak için bir `WriteDocuments` yöntemini kullanır. Bu yöntem bir sonraki bölümde açıklanmaktadır.

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

Sonuçları "hotels" dizininin örnek verilerle doldurulmuştur varsayılarak sorgular için önceki bölümdeki göründüğünü aşağıda verilmiştir:

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

Yukarıdaki örnek kod, arama sonuçlarını çıkarmak için konsolu kullanır. Benzer şekilde, kendi uygulamanızda arama sonuçlarını göstermeniz gerekir. Bir ASP.NET MVC tabanlı web uygulamasında arama sonuçları işlemek nasıl bir örnek için bkz [DotNetSample proje](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) GitHub üzerinde.

## <a name="next-steps"></a>Sonraki adımlar

Bunu henüz yapmadıysanız örnek kodu inceleyin [DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) github'da ile birlikte [bir .NET uygulamasından Azure Search kullanma](search-howto-dotnet-sdk.md) daha ayrıntılı örnek kod açıklamaları için. 