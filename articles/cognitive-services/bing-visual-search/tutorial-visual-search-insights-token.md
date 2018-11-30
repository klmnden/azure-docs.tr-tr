---
title: 'Öğretici: ImageInsightsToken - Bing Görsel Arama'
titlesuffix: Azure Cognitive Services
description: ImageInsightsToken tarafından belirtilen görüntülerin URL’lerini almak için Bing Görsel Arama SDK’sını kullanma.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: tutorial
ms.date: 06/21/2018
ms.author: rosh
ms.openlocfilehash: 62780500d29c891182d3869bf0ba3ccdc5e2f715
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441070"
---
# <a name="tutorial-bing-visual-search-sdk-imageinsightstoken-and-results"></a>Öğretici: Bing Görsel Arama SDK’sı ImageInsightsToken ve sonuçlar
Görsel Arama SDK’sı, `ImageInsightsToken` döndüren önceki bir aramadan çevrimiçi görüntüleri bulma seçeneğini içerir.  Bu örnek, bir `ImageInsightsToken` alır ve sonraki bir aramada belirteci kullanır.  Kod, Bing’e `ImageInsightsToken` gönderir ve Bing Arama URL’lerini ve çevrimiçi bulunan benzer görüntülerin URL’lerini içeren sonuçları döndürür.

## <a name="prerequisites"></a>Önkoşullar
Visual Studio 2017. Gerekirse ücretsiz topluluk sürümünü şuradan indirebilirsiniz: https://www.visualstudio.com/vs/community/.
SDK çağrılarının kimliğini doğrulamak için bir bilişsel hizmetler API anahtarı gerekir. Ücretsiz deneme anahtarına kaydolun. Deneme anahtarı yedi gün boyunca saniyede bir çağrı için kullanılabilir. Üretim senaryoları için bir erişim anahtarı satın alın. Ayrıca fiyatlandırma bilgilerine de bakın.
.NET Core SDK'sını, .NET Core 1.1 uygulamalarını çalıştırma olanağı. CORE, Framework ve Çalışma Zamanı sürümlerini şuradan alabilirsiniz: https://www.microsoft.com/net/download/.

Bu öğreticide, bir abonelik S9 fiyat katmanı gösterildiği gibi başlatmanız gerekecek [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/search-api/). 

Azure Portalı'nda bir abonelik başlatmak için:
1. 'BingSearchV7' ifadesini içeren Azure Portal'ın üstünde metin kutusuna girin `Search resources, services, and docs`.  
2. Aşağı açılan listesinde Marketi bölümünde seçin `Bing Search v7`.
3. Girin `Name` yeni kaynak için.
4. Seçin `Pay-As-You-Go` abonelik.
5. Seçin `S9` fiyatlandırma katmanı.
6. Tıklayın `Enable` abonelik başlatmak için.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları
Bing Web Araması SDK’sını kullanarak bir konsol uygulaması kurmak için Visual Studio’da Çözüm Gezgini’nde NuGet Paketlerini Yönet seçeneğine gidin. Ekle:
* Microsoft.Azure.CognitiveServices.Search.VisualSearch
* Microsoft.Azure.CognitiveServices.Search.ImageSearchpackage paketleri.

NuGet Web Araması SDK’sı paketini yüklemek aşağıdakilerin dahil olduğu bağımlılıkları da yükler:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="get-the-imageinsightstoken-from-image-search"></a>Görüntü Arama’dan ImageInsightsToken Al

Bu örnekte, aşağıdaki yöntem tarafından alınan bir `ImageInsightsToken` kullanılır.  Bu çağrı hakkında daha fazla bilgi için bkz. [Görüntü Arama SDK’sı C# hızlı başlangıcı](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart).

Kod, bir 'Kanada Rocky Dağları' sorgusunda sonuçları arar ve bir ImageInsightsToken alır. İlk görüntünün içgörü belirtecini, küçük resim URL’sini ve görüntü içeriği URL’sini yazdırır.  Yöntem, sonraki bir Görsel Arama isteğinde kullanmak üzere `ImageInsightsToken` döndürür.

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

## <a name="specify-the-imageinsightstoken-for-visual-search-request"></a>Görsel Arama isteği için ImageInsightsToken belirtin

Bu örnek, önceki yöntem tarafından döndürülen içgörü belirtecini kullanır. Aşağıdaki kod, `ImageInsightsToken` öğesinden bir `ImageInfo` nesnesi oluşturur ve ImageInfo nesnesini bir `VisualSearchRequest` öğesine yükler. `VisualSearchRequest` için `ImageInfo` içinde `ImageInsightsToken` belirtin

```
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```

## <a name="use-visual-search-to-find-images-from-an-imageinsightstoken"></a>ImageInsightsToken öğesinden görüntüleri bulmak için Görsel Arama’yı kullanma

`VisualSearchRequest`, `ImageInfo` nesnesinde aranacak görüntü ile ilgili bilgileri içerir.  `VisualSearchMethodAsync` yöntemi sonuçları alır.
```
// An image binary is not necessary here, as the image is specified by insights token.
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
Console.WriteLine("\r\nVisual search request with knowledgeRequest");

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>ImageModuleAction öğesinden URL verilerini alma
Görsel Arama sonuçları `ImageTag` nesneleridir.  Her etiket bir `ImageAction` nesneleri listesi içerir.  Her `ImageAction`, eylem türüne bağlı değerlerin bir listesi olan `Data` alanını içerir:

Aşağıdaki kod ile çeşitli türleri alabilirsiniz:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
Tam uygulama şunları döndürür:

* ActionType: MoreSizes -> WebSearchUrl:
* ActionType: VisualSearch -> WebSearchUrl:
* ActionType: ImageById -> WebSearchUrl:
* ActionType: RelatedSearches -> WebSearchUrl:
* ActionType: DocumentLevelSuggestions -> WebSearchUrl:
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fcanadian%2brocky&p=DevEx,5823.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&p=DevEx,5831.1

Yukarıdaki listede gösterildiği gibi `TopicResults` ve `ImageResults` türleri, ilgili görüntüler için sorguları içerir. Listedeki URL’ler, Bing arama sonuçlarına yönlendirir.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>Görsel Arama tarafından bulunan görüntülerin PagesIncluding ActionType URL’leri

Gerçek görüntü URL’lerini almak için, bir `ActionType` türünü `ImageModuleAction` olarak okuyan bir tür dönüştürme gerekir. Bu tür ise değerler listesine sahip bir `Data` öğesi içerir.  Her değer, bir görüntünün URL’sidir.  Aşağıdaki, `PagesIncluding` eylem türünü `ImageModuleAction` türüne dönüştürür ve değerleri okur.
```
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```
Bu veri türleri hakkında daha fazla bilgi için bkz. [Görüntüler - Görsel Arama](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch).

## <a name="complete-code"></a>Tam kod

Aşağıdaki kod, önceki örnekleri çalıştırır. Bir post isteğinde `ImageInsightsToken` gönderir. Daha sonra her ActionType için Bing arama URL’lerini yazdırır. ActionType `PagesIncluding` olursa kod, `Data` içinde `ImageObject` öğelerini alır.  `Data`, Web sayfalarındaki görüntülerin URL’leri olan değerlerin bir listesini içerir.  Sonuçları göstermek için elde edilen Görsel Arama URL’lerini kopyalayıp tarayıcıya yapıştırın. Görüntüleri göstermek için ContentUrl öğelerini kopyalayıp tarayıcıya yapıştırın.

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
[Görsel Arama yanıtı](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview#the-response)
