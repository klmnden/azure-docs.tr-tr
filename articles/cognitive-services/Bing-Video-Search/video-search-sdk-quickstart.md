---
title: Video arama SDK C# hızlı başlangıç | Microsoft Docs
description: Video arayın SDK konsol uygulaması ayarlayın.
titleSuffix: Azure cognitive services setup News search SDK C# console application
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 01/29/2018
ms.author: v-gedod
ms.openlocfilehash: f53e2d0f0052ccfabb6d750556cb532f069c9121
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355294"
---
# <a name="video-search-sdk-c-quickstart"></a>Video arama SDK C# hızlı başlangıç

Bing Video arama SDK web isteklerini ve ayrıştırma sonuçları için REST API işlevselliğini içerir.

[Kaynak kodu C# Bing Video arama SDK örnekleri için](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingVideoSearch) Git hub'da kullanılabilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları

Bing Video arama SDK'yı kullanarak bir konsol uygulaması ayarlamak için Gözat `Manage NuGet Packages` Visual Studio'daki Çözüm Gezgini'nden seçeneği.  Ekleme `Microsoft.Azure.CognitiveServices.Search.VideoSearch` paket.

Yükleme [[NuGet Video arama SDK paketi]](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.VideoSearch/1.2.0) de dahil olmak üzere bağımlılıkları yükler:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json


## <a name="video-search-client"></a>Video arama istemci
Örneği oluşturmak için `VideoSearchAPI` istemci, yönergeleri kullanarak ekleyin:
```
using Microsoft.Azure.CognitiveServices.Search.VideoSearch;
using Microsoft.Azure.CognitiveServices.Search.VideoSearch.Models;

```
Ardından, istemci örneği:
```
var client = new VideoSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));


```
İstemci, videolar için bir sorgu metinle "SwiftKey" aramak için kullanın.
```
var videoResults = client.Videos.SearchAsync(query: "SwiftKey").Result;
Console.WriteLine("Search videos for query \"SwiftKey\"");

```

Sonuçları ayrıştırma sonra sonuçları sayısını doğrulayın ve kimliği, adı ve url ilk video sonucunun yazdırın.
```
if (videoResults == null)
{
    Console.WriteLine("Didn't see any video result data..");
}
else
{
    if (videoResults.Value.Count > 0)
    {
        var firstVideoResult = videoResults.Value.First();

        Console.WriteLine($"\r\nVideo result count: {videoResults.Value.Count}");
        Console.WriteLine($"First video id: {firstVideoResult.VideoId}");
        Console.WriteLine($"First video name: {firstVideoResult.Name}");
        Console.WriteLine($"First video url: {firstVideoResult.ContentUrl}");
    }
    else
    {
        Console.WriteLine("Couldn't find video results!");
    }
}

```
## <a name="complete-console-application"></a>Tam konsol uygulaması

Aşağıdaki konsol uygulaması önceden tanımlanmış sorguyu çalıştırır ve sonuçları ayrıştırır.

```
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.VideoSearch;
using Microsoft.Azure.CognitiveServices.Search.VideoSearch.Models;

namespace VideoSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {
            var client = new VideoSearchAPI(new ApiKeyServiceClientCredentials("19aa718a79d6444daaa415981d9f54ad"));

            try
            {
                var videoResults = client.Videos.SearchAsync(query: "SwiftKey").Result;
                Console.WriteLine("Search videos for query \"SwiftKey\"");

                if (videoResults == null)
                {
                    Console.WriteLine("Didn't see any video result data..");
                }
                else
                {
                    if (videoResults.Value.Count > 0)
                    {
                        var firstVideoResult = videoResults.Value.First();

                        Console.WriteLine($"\r\nVideo result count: {videoResults.Value.Count}");
                        Console.WriteLine($"First video id: {firstVideoResult.VideoId}");
                        Console.WriteLine($"First video name: {firstVideoResult.Name}");
                        Console.WriteLine($"First video url: {firstVideoResult.ContentUrl}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find video results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

            // Include these calls to use queries defined under the following headings.
            //VideoSearchWithFilters(client);
            //VideoDetail(client);
            //VideoTrending(client);


            Console.WriteLine("Any key to exit...");
            Console.ReadKey();

        }

```
## <a name="url-parameters"></a>URL parametreleri

Sorgu metni "Bellevue toplamı" değişmeden, kısa videolar ve 1080 p çözüm arayın.  Sonuç sayısı doğrulayın ve yazdırma kimliği, adı ve url ilk video sonucunun.

```
        public static void VideoSearchWithFilters(VideoSearchAPI client)
        {
            try
            {
                var videoResults = client.Videos.SearchAsync(query: "Bellevue Trailer", pricing: VideoPricing.Free, length: VideoLength.Short, resolution: VideoResolution.HD1080p).Result;
                Console.WriteLine("Search videos for query \"Bellevue Trailer\" that is free, short and 1080p resolution");

                if (videoResults == null)
                {
                    Console.WriteLine("Didn't see any video result data..");
                }
                else
                {
                    if (videoResults.Value.Count > 0)
                    {
                        var firstVideoResult = videoResults.Value.First();

                        Console.WriteLine($"\r\nVideo result count: {videoResults.Value.Count}");
                        Console.WriteLine($"First video id: {firstVideoResult.VideoId}");
                        Console.WriteLine($"First video name: {firstVideoResult.Name}");
                        Console.WriteLine($"First video url: {firstVideoResult.ContentUrl}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find video results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

        }


```
## <a name="trending-videos"></a>Popüler videolar
Oluşturan eğilim video arayın, ardından başlık kutucukları ve kategorileri doğrulayın.
```
        public static void VideoTrending(VideoSearchAPI client)
        {
            try
            {
                var trendingResults = client.Videos.TrendingAsync().Result;
                Console.WriteLine("Search trending videos");

                if (trendingResults == null)
                {
                    Console.WriteLine("Didn't see any trending video data..");
                }
                else
                {
                    // Banner Tiles
                    if (trendingResults.BannerTiles?.Count > 0)
                    {
                        var firstBannerTile = trendingResults.BannerTiles[0];
                        Console.WriteLine($"\r\nBanner tile count: {trendingResults.BannerTiles.Count}");
                        Console.WriteLine($"First banner tile text: {firstBannerTile.Query.Text}");
                        Console.WriteLine($"First banner tile url: {firstBannerTile.Query.WebSearchUrl}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find banner tiles!");
                    }

                    // Categories
                    if (trendingResults.Categories?.Count > 0)
                    {
                        var firstCategory = trendingResults.Categories[0];
                        Console.WriteLine($"Category count: {trendingResults.Categories.Count}");
                        Console.WriteLine($"First category title: {firstCategory.Title}");

                        if (firstCategory.Subcategories?.Count > 0)
                        {
                            var firstSubCategory = firstCategory.Subcategories[0];
                            Console.WriteLine($"SubCategory count: {firstCategory.Subcategories.Count}");
                            Console.WriteLine($"First sub category title: {firstSubCategory.Title}");

                            if (firstSubCategory.Tiles?.Count > 0)
                            {
                                var firstTile = firstSubCategory.Tiles[0];
                                Console.WriteLine($"Tile count: {firstSubCategory.Tiles.Count}");
                                Console.WriteLine($"First tile text: {firstTile.Query.Text}");
                                Console.WriteLine($"First tile url: {firstTile.Query.WebSearchUrl}");
                            }
                            else
                            {
                                Console.WriteLine("Couldn't find tiles!");
                            }
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find subcategories!");
                        }
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find categories!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

        }


```
## <a name="details"></a>Ayrıntılar
Videolar "Bellevue toplamı" için arama ve ilk video ayrıntılı bilgi için arama yapın.
```
        public static void VideoDetail(VideoSearchAPI client)
        {
            try
            {
                var videoResults = client.Videos.SearchAsync(query: "Bellevue Trailer").Result;

                var firstVideo = videoResults?.Value?.FirstOrDefault();

                if (firstVideo != null)
                {
                    var modules = new List<VideoInsightModule?>() { VideoInsightModule.All };
                    var videoDetail = client.Videos.DetailsAsync(query: "Bellevue Trailer", id: firstVideo.VideoId, modules: modules).Result;
                    Console.WriteLine($"Search detail for video id={firstVideo.VideoId}, name={firstVideo.Name}");

                    if (videoDetail != null)
                    {
                        if (videoDetail.VideoResult != null)
                        {
                            Console.WriteLine($"\r\nExpected video id: {videoDetail.VideoResult.VideoId}");
                            Console.WriteLine($"Expected video name: {videoDetail.VideoResult.Name}");
                            Console.WriteLine($"Expected video url: {videoDetail.VideoResult.ContentUrl}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find expected video!");
                        }

                        if (videoDetail?.RelatedVideos?.Value?.Count > 0)
                        {
                            var firstRelatedVideo = videoDetail.RelatedVideos.Value[0];
                            Console.WriteLine($"Related video count: {videoDetail.RelatedVideos.Value.Count}");
                            Console.WriteLine($"First related video id: {firstRelatedVideo.VideoId}");
                            Console.WriteLine($"First related video name: {firstRelatedVideo.Name}");
                            Console.WriteLine($"First related video url: {firstRelatedVideo.ContentUrl}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find any related video!");
                        }
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find detail about the video!");
                    }
                }
                else
                {
                    Console.WriteLine("Couldn't find video results!");
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

[Bilişsel hizmetler .NET SDK'sı örneği](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)