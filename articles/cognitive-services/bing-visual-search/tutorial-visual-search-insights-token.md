---
title: Bing görsel arama SDK ImageInsightsToken Öğreticisi | Microsoft Docs
description: Bing görsel arama SDK ImageInsightsToken tarafından belirtilen görüntülerin URL'lerini almaya kullanma
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 06/21/2018
ms.author: rosh
ms.openlocfilehash: 8f6e7f7e88ae78fe7e8a9be4adefd689dd26d0f9
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "41987473"
---
# <a name="tutorial-bing-visual-search-sdk-imageinsightstoken-and-results"></a>Öğretici: Bing görsel arama SDK ImageInsightsToken ve sonuçları
Döndüren bir önceki arama çevrimiçi resimleri bulmak için bir seçenek Visual Search SDK'sını içeren bir `ImageInsightsToken`.  Bu örnekte bir `ImageInsightsToken` ve bir sonraki arama belirtecini kullanır.  Kodu gönderir `ImageInsightsToken` Bing arama URL'leri ve çevrimiçi bulunan benzer görüntülerin URL'lerini dahil sonuçlarına Bing ve döndürür.

## <a name="prerequisites"></a>Önkoşullar
Visual Studio 2017. Gerekirse, ücretsiz bir topluluk sürümünü buradan indirebilirsiniz varsa: https://www.visualstudio.com/vs/community/.
Bilişsel hizmetler API anahtarı kimlik doğrulaması SDK çağrıları için gereklidir. Ücretsiz bir deneme anahtarı için kaydolun. Deneme anahtarı ile saniye başına çağrı yedi gün için uygundur. Üretim senaryoları için bir erişim anahtarı satın alın. Ayrıca fiyatlandırma bilgileri görürsünüz.
.NET core SDK'sı, .net core 1.1 uygulamaları çalıştırma olanağı. Çekirdek çerçevesi ve çalışma zamanı buradan edinebilirsiniz: https://www.microsoft.com/net/download/.

##<a name="application-dependencies"></a>Uygulama bağımlılıkları
Bing Web araması SDK'sını kullanarak bir konsol uygulaması ayarlamak için NuGet paketlerini Yönet seçeneğini Visual Studio'da Çözüm Gezgini'nden göz atın. Ekleyin:
* Microsoft.Azure.CognitiveServices.Search.VisualSearch
* Microsoft.Azure.CognitiveServices.Search.ImageSearchpackage paketler.

Web arama SDK'sı NuGet paketi yükleniyor dahil olmak üzere, bağımlılıkları yükler:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="get-the-imageinsightstoken-from-image-search"></a>Görüntü arama ImageInsightsToken Al
Bu örnekte bir `ImageInsightsToken` aşağıdaki yöntemi tarafından alınan.  Bu çağrı hakkında daha fazla bilgi için bkz: [görüntü arama SDK C# hızlı](https://docs.microsoft.com/en-us/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart).

Kod 'Kanada Rockies' için bir sorgu sonuçlarına arar ve bir ImageInsightsToken alır. Bunu, ilk görüntünün ınsights belirteci, küçük resim URL'si ve resim içerik URL'si yazdırır.  Yöntem döndürür `ImageInsightsToken`görsel arama Taleplerde kullanmak için.

```
        private static String ImageResults(String subKey)
        {
            String insightTok = "None";
            try
            {
                var client = new ImageSearchAPI(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey)); //
                var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
                Console.WriteLine("Search images for query \"canadian rockies\"");

                if (imageResults == null)
                {
                    Console.WriteLine("No image result data.");
                }
                else
                {
                    // Image results
                    if (imageResults.Value.Count > 0)
                    {
                        var firstImageResult = imageResults.Value.First();

                        Console.WriteLine($"\r\nImage result count: {imageResults.Value.Count}");
                        insightTok = firstImageResult.ImageInsightsToken;
                        Console.WriteLine($"First image insights token: {firstImageResult.ImageInsightsToken}");  
                        Console.WriteLine($"First image thumbnail url: {firstImageResult.ThumbnailUrl}");
                        Console.WriteLine($"First image content url: {firstImageResult.ContentUrl}");
                    }
                    else
                    {
                        insightTok = "None found";
                        Console.WriteLine("Couldn't find image results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("\r\nEncountered exception. " + ex.Message);
            }

            return insightTok;
        }
```

## <a name="specify-the-imageinsightstoken-for-visual-search-request"></a>Görsel arama isteği için ImageInsightsToken belirtin
Bu örnek, önceki yöntem tarafından döndürülen ınsights belirtecini kullanır. Aşağıdaki kod oluşturur bir `ImageInfo` nesnesinden `ImageInsightsToken` ve ImageInfo nesnesine yükler bir `VisualSearchRequest`. Belirtin `ImageInsightsToken` içinde bir `ImageInfo` için `VisualSearchRequest`

```
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```
## <a name="use-visual-search-to-find-images-from-an-imageinsightstoken"></a>Bir ImageInsightsToken görüntüleri bulmak için görsel arama
`VisualSearchRequest` Resmi için arama hakkında bilgi içeren `ImageInfo` nesne.  `VisualSearchMethodAsync` Yöntemi sonuçlarını alır.
```
// An image binary is not necessary here, as the image is specified by insights token.
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
Console.WriteLine("\r\nVisual search request with knowledgeRequest");

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>ImageModuleAction URL verilerini al
Görsel arama sonuçları `ImageTag` nesneleri.  Her etiket bir listesini içeren `ImageAction` nesneleri.  Her `ImageAction` içeren bir `Data` eylem türüne bağlı olan değerler listesini alanı:

Aşağıdaki kod ile çeşitli türleri alabilirsiniz:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
Uygulamanın döndürür:

* ActionType: MoreSizes WebSearchUrl ->:
* ActionType: VisualSearch WebSearchUrl ->:
* ActionType: ImageById WebSearchUrl ->:
* ActionType: RelatedSearches WebSearchUrl ->:
* ActionType: DocumentLevelSuggestions WebSearchUrl ->:
* ActionType: TopicResults WebSearchUrl ->: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fcanadian%2brocky&p=DevEx, 5823.1
* ActionType: ImageResults WebSearchUrl ->: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&p=DevEx, 5831.1

Önceki listede gösterildiği `TopicResults` ve `ImageResults` sorguları ilgili görüntüleri için türler bulunur. Bing arama sonuçları listesinde bağlantısını URL'lerinde.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>PagesIncluding ActionType URL'leri görüntülerin görsel arama sonucu bulundu

Gerçek görüntü URL'leri alma gerektirir okuyan bir cast bir `ActionType` olarak `ImageModuleAction`, içeren bir `Data` değerleri bir liste öğesi.  Her bir görüntünün URL'sini değerdir.  Aşağıdaki yayınları `PagesIncluding` eylem türü `ImageModuleAction` ve değerlerini okur.
```
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```
Bu veri türleri hakkında daha fazla bilgi için bkz. [görüntüleri - görsel arama](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch).
## <a name="complete-code"></a>Tam kod

Aşağıdaki kod, önceki örneklerde çalışır. Gönderdiği `ImageInsightsToken` bir post isteğinde. Bing yazdırır sonra URL'leri her ActionType için arama yapın. ActionType ise `PagesIncluding`, kod `ImageObject` öğeler `Data`.  `Data` Web sayfalarındaki görüntülerin URL'lerini olan değerlerin bir listesini içerir.  Sonuçta elde edilen görsel arama sonuçları göstermek için URL'leri tarayıcıya kopyalayıp yeniden açın. Görüntüleri göstermek için tarayıcı ContentUrl öğeleri kopyalayıp yeniden açın.

```
using System;
using System.IO;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;

namespace VisualSearchFeatures
{
    class Program
    {
        static void Main(string[] args)
        {
            String subscriptionKey = "YOUR-ACCESS-KEY";

            insightsToken = ImageResults(subscriptionKey);

            VisualSearchInsightsToken(subscriptionKey, insightsToken);

            Console.WriteLine("Any key to quit...");
            Console.ReadKey();

        }

        // Searches for results on query (Canadian Rockies) and gets an ImageInsightsToken.
        // Also prints first image insights token, thumbnail url, and image content url.
        private static String ImageResults(String subKey)
        {
            String insightTok = "None";
            try
            {
                var client = new ImageSearchAPI(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey)); //
                var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
                Console.WriteLine("Search images for query \"canadian rockies\"");

                if (imageResults == null)
                {
                    Console.WriteLine("No image result data.");
                }
                else
                {
                    // Image results
                    if (imageResults.Value.Count > 0)
                    {
                        var firstImageResult = imageResults.Value.First();

                        Console.WriteLine($"\r\nImage result count: {imageResults.Value.Count}");
                        insightTok = firstImageResult.ImageInsightsToken;
                        Console.WriteLine($"First image insights token: {firstImageResult.ImageInsightsToken}");  
                        Console.WriteLine($"First image thumbnail url: {firstImageResult.ThumbnailUrl}");
                        Console.WriteLine($"First image content url: {firstImageResult.ContentUrl}");
                    }
                    else
                    {
                        insightTok = "None found";
                        Console.WriteLine("Couldn't find image results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("\r\nEncountered exception. " + ex.Message);
            }

            return insightTok;
        }


        // This method will get Visual Search results from an imageInsightsToken obtained from the return value of the ImageResults method.
        // The method includes imageInsightsToken the in a knowledgeRequest parameter, along with a cropArea object. 
        // It prints out the imageInsightsToken uploaded in the request.
        // Finally the example prints URLs of images found using the imageInsightsToken.

        public static void VisualSearchInsightsToken(string subscriptionKey, string insightsTok)
        {
            var client = new VisualSearchAPI(new Microsoft.Azure.CognitiveServices.Search.VisualSearch.ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                // The image can be specified via an insights token, in the ImageInfo object.
                ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);

                // An image binary is not necessary here, as the image is specified by insights token.
                VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
                Console.WriteLine("\r\nVisual search request with imageInsightsToken and knowledgeRequest");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // Visual Search results
                    if (visualSearchResults.Image?.ImageInsightsToken != null)
                    {
                        Console.WriteLine($"Uploaded image insights token: {insightsTok}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image insights token!");
                    }

                    if (visualSearchResults.Tags.Count > 0)
                    {
                        // List of tags
                        foreach (ImageTag t in visualSearchResults.Tags)
                        {
                            foreach (ImageAction i in t.Actions)
                            {
                                Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " WebSearchURL: " + i.WebSearchUrl);

                                if (i.ActionType == "PagesIncluding")
                                {
                                    foreach (ImageObject o in (i as ImageModuleAction).Data.Value)
                                    {
                                        Console.WriteLine("ContentURL: " + o.ContentUrl);
                                    }
                                }
                            }
                        }
                    }

                    else
                    {
                        Console.WriteLine("Couldn't find image tags!");
                    }

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
## <a name="next-steps"></a>Sonraki adımlar
[Görsel arama yanıt](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview#the-response)
