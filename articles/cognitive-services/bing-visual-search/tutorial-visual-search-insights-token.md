---
title: Bing Visual arama SDK ImageInsightsToken Öğreticisi | Microsoft Docs
description: Bing Visual arama SDK ImageInsightsToken tarafından belirtilen görüntüleri URL'lerini almak için nasıl kullanılacağını.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 06/21/2018
ms.author: rosh
ms.openlocfilehash: 578fa90f504920030b488d2b8fa3a2d0232cccce
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753679"
---
# <a name="tutorial-bing-visual-search-sdk-imageinsightstoken-and-results"></a>Öğretici: Bing Visual arama SDK ImageInsightsToken ve sonuçları
Görsel arama SDK'sı görüntülerden çevrimiçi döndüren bir önceki arama bulmak için bir seçenek içerir bir `ImageInsightsToken`.  Bu örnek alır bir `ImageInsightsToken` ve bir sonraki aramada belirteci kullanır.  Kod gönderir `ImageInsightsToken` Bing arama ve çevrimiçi bulunan benzer görüntülerinin URL'leri dahil sonuçlarına Bing ve döndürür.

## <a name="prerequisites"></a>Önkoşullar
Visual Studio 2017. Gerekirse, ücretsiz topluluk sürümünü buradan indirebilirsiniz varsa: https://www.visualstudio.com/vs/community/.
Bilişsel hizmetler API anahtarı SDK çağrıları kimlik doğrulaması için gereklidir. Ücretsiz bir deneme anahtarı için kaydolun. Deneme anahtarı saniyede bir çağrıyla yedi gün için uygundur. Üretim senaryoları için erişim anahtarı satın alın. Ayrıca fiyatlandırma bilgileri bakın.
.NET core SDK, .net core 1.1 uygulamalarını çalıştırma özelliği. Buradan çekirdek, Framework ve çalışma zamanı elde edebilirsiniz: https://www.microsoft.com/net/download/.

##<a name="application-dependencies"></a>Uygulama bağımlılıkları
Bing Web arama SDK'yı kullanarak bir konsol uygulaması ayarlamak için Visual Studio'daki Çözüm Gezgini'nden NuGet paketlerini Yönet seçeneğini göz atın. Ekleyin:
* Microsoft.Azure.CognitiveServices.Search.VisualSearch
* Microsoft.Azure.CognitiveServices.Search.ImageSearchpackage paketler.

Web ara SDK'sı NuGet paketi yükleniyor dahil olmak üzere bağımlılıkları yükler:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="get-the-imageinsightstoken-from-image-search"></a>Görüntü aramadan ImageInsightsToken Al
Bu örnekte bir `ImageInsightsToken` aşağıdaki yöntemi tarafından alınan.  Bu çağrı hakkında daha fazla bilgi için bkz: [görüntü arama SDK C# quickstart](https://docs.microsoft.com/en-us/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart).

Kod 'İçin Kanada Rockies' sorgu sonuçlarına arar ve bir ImageInsightsToken alır. İlk resmin Öngörüler belirteci, küçük resim URL'si ve resim içerik URL'si yazdırır.  Yöntem `ImageInsightsToken`Visual arama Taleplerde kullanmak için.

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

## <a name="specify-the-imageinsightstoken-for-visual-search-request"></a>Görsel arama isteği ImageInsightsToken belirtin
Bu örnek önceki yöntemi tarafından döndürülen Öngörüler belirtecini kullanır. Aşağıdaki kod oluşturur bir `ImageInfo` nesnesinin `ImageInsightsToken` ve ImageInfo nesnesine yükleyen bir `VisualSearchRequest`. Belirtin `ImageInsightsToken` içinde bir `ImageInfo` için `VisualSearchRequest`

```
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```
## <a name="use-visual-search-to-find-images-from-an-imageinsightstoken"></a>Bir ImageInsightsToken görüntüleri bulmak için görsel arama
`VisualSearchRequest` İçinde aramak için görüntü ile ilgili bilgileri içeren `ImageInfo` nesnesi.  `VisualSearchMethodAsync` Yöntemi sonuçları alır.
```
// An image binary is not necessary here, as the image is specified by insights token.
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
Console.WriteLine("\r\nVisual search request with knowledgeRequest");

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>ImageModuleAction URL Veri Al
Görsel arama sonuçları `ImageTag` nesneleri.  Her etiket bir listesini içeren `ImageAction` nesneleri.  Her `ImageAction` içeren bir `Data` eylem türüne bağlı olan bir değer listesi alanı:

Aşağıdaki kod ile çeşitli türleri alabilirsiniz:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
Tam uygulama döndürür:

* ActionType: MoreSizes WebSearchUrl ->:
* ActionType: VisualSearch WebSearchUrl ->:
* ActionType: ImageById WebSearchUrl ->:
* ActionType: RelatedSearches WebSearchUrl ->:
* ActionType: DocumentLevelSuggestions WebSearchUrl ->:
* ActionType: TopicResults WebSearchUrl ->: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fcanadian%2brocky&p=DevEx, 5823.1
* ActionType: ImageResults WebSearchUrl ->: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&p=DevEx, 5831.1

Yukarıdaki listede gösterildiği gibi `TopicResults` ve `ImageResults` türlerini içeren ilgili görüntüleri için sorgular. Bing arama sonuçları listesinde bağlantısını URL'lerinde.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>PagesIncluding ActionType URL'leri görüntüsü Visual arama sonucu bulunamadı

Gerçek görüntü URL'leri alma okuyan bir cast gerektiren bir `ActionType` olarak `ImageModuleAction`, içeren bir `Data` değerleri listesi öğesiyle.  Her değer bir görüntü URL'dir.  Aşağıdaki atamalar `PagesIncluding` eylem türü `ImageModuleAction` ve değerleri okur.
```
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```
Bu veri türleri hakkında daha fazla bilgi için bkz: [görüntüleri - Visual arama](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch).
## <a name="complete-code"></a>Tam kod

Aşağıdaki kod, önceki örneklerin çalışır. Gönderir `ImageInsightsToken` bir post isteği. Bing yazdırır sonra URL'leri her ActionType için arama yapın. ActionType ise `PagesIncluding`, kodu alır `ImageObject` öğeler `Data`.  `Data` Web sayfalarındaki görüntülerin URL'leri değerler listesini içerir.  Kopyalayın ve sonuçta elde edilen Visual arama sonuçları göstermek için URL'leri tarayıcıya yapıştırın. Görüntüleri göstermek için tarayıcı ContentUrl öğeleri kopyalayıp yeniden açın.

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
[Görsel arama yanıtı](https://review.docs.microsoft.com/en-us/azure/cognitive-services/bing-visual-search/overview?branch=pr-en-us-44614#the-response)