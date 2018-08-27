---
title: 'Hızlı Başlangıç: Bing Web araması SDK için C# kullanın'
description: Web araması SDK C# konsol uygulaması için kurulumu.
titleSuffix: Azure cognitive services Web search SDK C# quickstart
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 08/16/2018
ms.author: v-gedod, erhopf
ms.openlocfilehash: ef54487a1df7303fa92a78e4f3219f40f558da2b
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42887328"
---
# <a name="quickstart-use-the-bing-web-search-sdk-for-c"></a>Hızlı Başlangıç: Bing Web araması SDK için C# kullanın

Bing Web araması SDK'sı, web isteklerini ve ayrıştırma sonuçları için REST API işlevselliğini içerir.

[Kaynak kodu için C# Bing Web araması SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/BingSearchv7/BingWebSearch/WebSearchSamples.cs) Github'da kullanılabilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları

Bing Web araması SDK'sını kullanarak bir konsol uygulaması oluşturma için göz atın `Manage NuGet Packages` Visual Studio'daki Çözüm Gezgini'nden seçeneği.  Ekleme `Microsoft.Azure.CognitiveServices.Search.WebSearch` paket.

Yükleme [Web Search SDK'sı NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.WebSearch/1.2.0) de dahil olmak üzere, bağımlılıkları yükler:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="web-search-client"></a>Web araması istemcisi
Bir örneğini oluşturmak için `WebSearchAPI` istemci, using yönergelerini ekleyin:
```
using Microsoft.Azure.CognitiveServices.Search.WebSearch;
using Microsoft.Azure.CognitiveServices.Search.WebSearch.Models;

```
Ardından, istemci örneği:
```
var client = new WebSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));


```
İstemci ile bir sorgu metni aramak için kullanın:
```
// Search for "Yosemite National Park"
var webData = client.Web.Search(query: "Yosemite National Park");
Console.WriteLine("Searched for Query# \" Yosemite National Park \"");

```
Önceki sorgu sonuçlarında çıkmadıysa web sayfaları ayrıştırılamıyor:
```
//WebPages
if (webData?.WebPages?.Value?.Count > 0)
{
    // find the first web page
    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

    if (firstWebPagesResult != null)
    {
        Console.WriteLine("Webpage Results #{0}", webData.WebPages.Value.Count);
        Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
        Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
    }
    else
    {
        Console.WriteLine("Couldn't find web results!)";
    }
}
else
{
    Console.WriteLine("Didn't see any Web data..");
}

```
## <a name="complete-console-application"></a>Eksiksiz bir konsol uygulaması

Aşağıdaki konsol uygulamasında önceden tanımlanmış sorgu yürütülür ve web sayfaları, görüntüler, haber ve sonuçlarda yer alan videoları ayrıştırır:
```
using System;
using System.Collections.Generic;
using Microsoft.Azure.CognitiveServices.Search.WebSearch;
using Microsoft.Azure.CognitiveServices.Search.WebSearch.Models;
using System.Linq;

namespace WebSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {
            var client = new WebSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));

            WebResults(client);
            // Include the following calls to use queries defined under following headings.
            //WebResultsWithCountAndOffset(client);  
            //WebSearchWithResponseFilter(client);
            //WebSearchWithAnswerCountPromoteAndSafeSearch(client);

            Console.WriteLine("Any key to exit... ");
            Console.ReadKey();
        }

        public static void WebResults(WebSearchAPI client)
        {
            try
            {
                // Search for (Yosemite National Park"), print the number of results and print out name and url of first result.
                var webData = client.Web.Search(query: "Yosemite National Park");
                Console.WriteLine("Searched for Query# \" Yosemite National Park \"");

                //WebPages
                if (webData?.WebPages?.Value?.Count > 0)
                {
                    // find the first web page
                    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

                    if (firstWebPagesResult != null)
                    {
                        Console.WriteLine("Webpage Results #{0}", webData.WebPages.Value.Count);
                        Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
                        Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find web results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any Web data..");
                }

                //Images
                if (webData?.Images?.Value?.Count > 0)
                {
                    // find the first image result
                    var firstImageResult = webData.Images.Value.FirstOrDefault();

                    if (firstImageResult != null)
                    {
                        Console.WriteLine("Image Results #{0}", webData.Images.Value.Count);
                        Console.WriteLine("First Image result name: {0} ", firstImageResult.Name);
                        Console.WriteLine("First Image result URL: {0} ", firstImageResult.ContentUrl);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find first image results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any image data..");
                }

                //News
                if (webData?.News?.Value?.Count > 0)
                {
                    // find the first news result
                    var firstNewsResult = webData.News.Value.FirstOrDefault();

                    if (firstNewsResult != null)
                    {
                        Console.WriteLine("\r\nNews Results #{0}", webData.News.Value.Count);
                        Console.WriteLine("First news result name: {0} ", firstNewsResult.Name);
                        Console.WriteLine("First news result URL: {0} ", firstNewsResult.Url);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find any News results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see first news data..");
                }

                //Videos
                if (webData?.Videos?.Value?.Count > 0)
                {
                    // find the first video result
                    var firstVideoResult = webData.Videos.Value.FirstOrDefault();

                    if (firstVideoResult != null)
                    {
                        Console.WriteLine("\r\nVideo Results #{0}", webData.Videos.Value.Count);
                        Console.WriteLine("First Video result name: {0} ", firstVideoResult.Name);
                        Console.WriteLine("First Video result URL: {0} ", firstVideoResult.ContentUrl);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find first video results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any video data..");
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

        }
    }
}

```

Bing arama örnekleri SDK'ın çeşitli özelliklerini gösterir.  Önceden tanımlanmış aşağıdaki işlevleri ekleyin `WebSrchSDK` sınıfı.

## <a name="count-and-offset-parameters"></a>Parametre sayısı ve uzaklık

Aşağıdaki kod, "İstanbul'da en iyi restoranlar" arar, sonuç sayısı doğrular ve adını ve URL'sini ilk sonuç yazdırır.

```
       public static void WebResultsWithCountAndOffset(WebSearchAPI client)
        {
            try
            {
                var webData = client.Web.SearchAsync(query: "Best restaurants in Seattle", offset: 10, count: 20).Result;
                Console.WriteLine("\r\nSearched for Query# \" Best restaurants in Seattle \"");

                if (webData?.WebPages?.Value?.Count > 0)
                {
                    // find the first web page
                    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

                    if (firstWebPagesResult != null)
                    {
                        Console.WriteLine("Web Results #{0}", webData.WebPages.Value.Count);
                        Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
                        Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find first web result!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any Web data..");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```
## <a name="response-filter"></a>Yanıt filtresi

Aşağıdaki sorguda arar ayarlamak bir yanıt filtresi kullanarak "Microsoft" terimi `news` ve ardından sonuçlarının ayrıntılarını yazdırır.
```
        public static void WebSearchWithResponseFilter(WebSearchAPI client)
        {
            //var client = new WebSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                IList<string> responseFilterstrings = new List<string>() { "news" };
                var webData = client.Web.SearchAsync(query: "Microsoft", responseFilter: responseFilterstrings).Result;
                Console.WriteLine("\r\nSearched for Query# \" Microsoft \" with response filters \"news\"");

                //News
                if (webData?.News?.Value?.Count > 0)
                {
                    // find the first news result
                    var firstNewsResult = webData.News.Value.FirstOrDefault();

                    if (firstNewsResult != null)
                    {
                        Console.WriteLine("News Results #{0}", webData.News.Value.Count);
                        Console.WriteLine("First news result name: {0} ", firstNewsResult.Name);
                        Console.WriteLine("First news result URL: {0} ", firstNewsResult.Url);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find first News results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any News data..");
                }

            }
            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```
## <a name="query-parameters---count-promotion-safe-search"></a>Sorgu parametreleri - sayısı, yükseltme, güvenli arama

Bu sorgu "Gaga", çalışanı kullanarak arar `answerCount` ve `promote` parametreleri ardından sonuçlarının ayrıntılarını yazdırır.

```
        public static void WebSearchWithAnswerCountPromoteAndSafeSearch(WebSearchAPI client)
        {
            //var client = new WebSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                IList<string> promoteAnswertypeStrings = new List<string>() { "videos" };
                var webData = client.Web.SearchAsync(query: "Lady Gaga", answerCount: 2, promote: promoteAnswertypeStrings, safeSearch: SafeSearch.Strict).Result;
                Console.WriteLine("\r\nSearched for Query# \" Lady Gaga \"");

                if (webData?.Videos?.Value?.Count > 0)
                {
                    var firstVideosResult = webData.Videos.Value.FirstOrDefault();

                    if (firstVideosResult != null)
                    {
                        Console.WriteLine("Video Results #{0}", webData.Videos.Value.Count);
                        Console.WriteLine("First Video result name: {0} ", firstVideosResult.Name);
                        Console.WriteLine("First Video result URL: {0} ", firstVideosResult.ContentUrl);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find videos results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any data..");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }
    }
```

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler .NET SDK'sı örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7).
