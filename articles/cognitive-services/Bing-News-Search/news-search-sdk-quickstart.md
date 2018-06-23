---
title: Haber arama SDK C# hızlı başlangıç | Microsoft Docs
description: Haber Ara SDK konsol uygulaması ayarlayın.
titleSuffix: Azure cognitive services News search SDK C# quickstart
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 01/30/2018
ms.author: v-gedod
ms.openlocfilehash: e803fd579c6b71b8b1754546446715795a12087a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355276"
---
# <a name="news-search-sdk-c-quickstart"></a>Haber arama SDK C# hızlı başlangıç

Bing Haberler arama SDK haber sorgular ve ayrıştırma sonuçları için REST API işlevselliğini içerir. 

[Kaynak kodu C# Bing Haberler arama SDK örnekleri için](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingNewsSearch) Git hub'da kullanılabilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları

Bing Haberler arama SDK'yı kullanarak bir konsol uygulaması ayarlamak için Gözat `Manage NuGet Packages` Visual Studio'daki Çözüm Gezgini'nden seçeneği.  Ekleme `Microsoft.Azure.CognitiveServices.Search.NewsSearch` paket.

Yükleme [NuGet haber arama SDK paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.NewsSearch/1.2.0) de dahil olmak üzere bağımlılıkları yükler:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="news-search-client"></a>Haber Arama İstemcisi
Örneği oluşturmak için `NewsSearchAPI` istemci, yönergesi kullanarak ekleyin:
```
using Microsoft.Azure.CognitiveServices.Search.NewsSearch;

```
Ardından, istemci örneği:
```
var client = new NewsSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));


```
İstemci ile bir sorgu metni aramak için kullanın:
```
var newsResults = client.News.SearchAsync(query: "Quantum  Computing", market: "en-us", count: 10).Result;
Console.WriteLine("Search news for query \"Quantum  Computing\" with market and count");

```
Önceki sorgu sonuçlarında döndürülen haber ayrıştırılamıyor:
```
if (newsResults.Value.Count > 0)
{
    var firstNewsResult = newsResults.Value.First();

    Console.WriteLine($"TotalEstimatedMatches value: {newsResults.TotalEstimatedMatches}");
    Console.WriteLine($"News result count: {newsResults.Value.Count}");
    Console.WriteLine($"First news name: {firstNewsResult.Name}");
    Console.WriteLine($"First news url: {firstNewsResult.Url}");
    Console.WriteLine($"First news description: {firstNewsResult.Description}");
    Console.WriteLine($"First news published time: {firstNewsResult.DatePublished}");
    Console.WriteLine($"First news provider: {firstNewsResult.Provider.First().Name}");
}

else
{
    Console.WriteLine("Couldn't find news results!");
}

```
## <a name="complete-console-application"></a>Tam konsol uygulaması

Aşağıdaki konsol uygulaması önceden tanımlanmış sorgu yürütür ve "Zamanlayıcının hesaplanmasında" haber arar. İstek içerir `market` ve `count` parametreleri. Kod sonuç sayısı doğrular ve yazdırır `totalEstimatedMatches`, `name`, `url`, `description`, `published time` ve `name` , `provider` ilk haber sonucu.

```
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.NewsSearch;

namespace NewsSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {
            var client = new NewsSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));

            try
            {
                var newsResults = client.News.SearchAsync(query: "Quantum  Computing", market: "en-us", count: 10).Result;
                Console.WriteLine("Search news for query \"Quantum  Computing\" with market and count");

                if (newsResults == null)
                {
                    Console.WriteLine("Didn't see any news result data..");
                }
                else
                {
                    if (newsResults.Value.Count > 0)
                    {
                        var firstNewsResult = newsResults.Value.First();

                        Console.WriteLine($"TotalEstimatedMatches value: {newsResults.TotalEstimatedMatches}");
                        Console.WriteLine($"News result count: {newsResults.Value.Count}");
                        Console.WriteLine($"First news name: {firstNewsResult.Name}");
                        Console.WriteLine($"First news url: {firstNewsResult.Url}");
                        Console.WriteLine($"First news description: {firstNewsResult.Description}");
                        Console.WriteLine($"First news published time: {firstNewsResult.DatePublished}");
                        Console.WriteLine($"First news provider: {firstNewsResult.Provider.First().Name}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find news results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

            // Include these methods to run queries explained under headings following.
            // NewsSearchWithFilters(client);
            // NewsCategory(client);
            // TrendingTopics(client);

            Console.WriteLine("Any key to exit...");
            Console.ReadKey();
        }

    }
}

```
## <a name="recent-news-freshness-and-sortby-parameters"></a>En son haberler, yenilik ve sortBy parametreleri
Aşağıdaki kod en son haberleri "İçin yapay zeka" ile yapılan aramalar `freshness` ve `sortBy` parametreleri. Sonuç sayısı doğrular ve yazdırır `totalEstimatedMatches`, `name`, `url`, `description`, `published time`, ve `name` ilk haber sonucunun sağlayıcısı.
```
        public static void NewsSearchWithFilters(NewsSearchAPI client)
        {
            try
            {
                var newsResults = client.News.SearchAsync(query: "Artificial Intelligence", market: "en-us", freshness: "Week", sortBy: "Date").Result;
                Console.WriteLine("Search most recent news for query \"Artificial Intelligence\" with freshness and sortBy");

                if (newsResults == null)
                {
                    Console.WriteLine("Didn't see any news result data..");
                }
                else
                {
                    if (newsResults.Value.Count > 0)
                    {
                        var firstNewsResult = newsResults.Value.First();

                        Console.WriteLine($"\r\nTotalEstimatedMatches value: {newsResults.TotalEstimatedMatches}");
                        Console.WriteLine($"News result count: {newsResults.Value.Count}");
                        Console.WriteLine($"First news name: {firstNewsResult.Name}");
                        Console.WriteLine($"First news url: {firstNewsResult.Url}");
                        Console.WriteLine($"First news description: {firstNewsResult.Description}");
                        Console.WriteLine($"First news published time: {firstNewsResult.DatePublished}");
                        Console.WriteLine($"First news provider: {firstNewsResult.Provider.First().Name}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find news results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```

## <a name="category-news-safe-search"></a>Kategori Haberler, güvenli arama
Aşağıdaki kod ile güvenli arama film ve TV eğlence için kategori haber arar.  Sonuç sayısı doğrular ve yazdırır `category`, `name`, `url`, `description`, `published time`, ve `name` ilk haber sonucunun sağlayıcısı.
```
        public static void NewsCategory(NewsSearchAPI client)
        {
            try
            {
                var newsResults = client.News.CategoryAsync(category: "Entertainment_MovieAndTV", market: "en-us", safeSearch: "strict").Result;
                Console.WriteLine("Search category news for movie and TV entertainment with safe search");

                if (newsResults == null)
                {
                    Console.WriteLine("Didn't see any news result data..");
                }
                else
                {
                    if (newsResults.Value.Count > 0)
                    {
                        var firstNewsResult = newsResults.Value.First();

                        Console.WriteLine($"\r\nNews result count: {newsResults.Value.Count}");
                        Console.WriteLine($"First news category: {firstNewsResult.Category}");
                        Console.WriteLine($"First news name: {firstNewsResult.Name}");
                        Console.WriteLine($"First news url: {firstNewsResult.Url}");
                        Console.WriteLine($"First news description: {firstNewsResult.Description}");
                        Console.WriteLine($"First news published time: {firstNewsResult.DatePublished}");
                        Console.WriteLine($"First news provider: {firstNewsResult.Provider.First().Name}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find news results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```
## <a name="trending-topics"></a>Oluşturan eğilim konuları
Aşağıdaki kod Bing Haberler oluşturan eğilim konuları arar. Sonuç sayısı doğrular ve yazdırır `name`, `text of query`, `webSearchUrl`, `newsSearchUrl`, ve `image.Url` ilk haber sonuç.
```
        public static void TrendingTopics(NewsSearchAPI client)
        {
            try
            {
                var trendingTopics = client.News.TrendingAsync(market: "en-us").Result;
                Console.WriteLine("Search news trending topics in Bing");

                if (trendingTopics == null)
                {
                    Console.WriteLine("Didn't see any news trending topics..");
                }
                else
                {
                    if (trendingTopics.Value.Count > 0)
                    {
                        var firstTopic = trendingTopics.Value.First();

                        Console.WriteLine($"\r\nTrending topics count: {trendingTopics.Value.Count}");
                        Console.WriteLine($"First topic name: {firstTopic.Name}");
                        Console.WriteLine($"First topic query: {firstTopic.Query.Text}");
                        Console.WriteLine($"First topic image url: {firstTopic.Image.Url}");
                        Console.WriteLine($"First topic webSearchUrl: {firstTopic.WebSearchUrl}");
                        Console.WriteLine($"First topic newsSearchUrl: {firstTopic.NewsSearchUrl}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find news trending topics!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler .NET SDK'sı örneği](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)