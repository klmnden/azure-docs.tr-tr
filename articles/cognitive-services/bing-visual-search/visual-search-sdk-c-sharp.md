---
title: "Hızlı Başlangıç: Bing Görsel Arama SDK'sı, C# kullanma"
titleSuffix: Azure Cognitive Services
description: Görsel Arama SDK'sı C# konsol uygulamasını kurma.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 05/16/2018
ms.author: v-gedod
ms.openlocfilehash: 25b01de47767e335d614aa0a8cf32c344c7305d8
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52442855"
---
# <a name="quickstart-bing-visual-search-sdk-c"></a>Hızlı Başlangıç: Bing Görsel Arama SDK'sı C#

Bing Görsel Arama SDK'sı, web istekleri ve sonuçları ayrıştırma için REST API işlevlerini kullanır.
[C# Bing Görsel Arama SDK'sı örneklerinin kaynak kodu](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingVisualSearch) Git Hub'dan edinilebilir.

Kod senaryoları aşağıdaki başlıklar altında belgelenmiştir:
* [Görsel Arama İstemcisi](#client)
* [Tam konsol uygulaması](#complete)
* [cropArea ile görüntü ikili gönderisi](#binary-crop)
* [KnowledgeRequest parametresi](#knowledge-req)
* [Etiketler, eylemler ve actionType](#tags-actions)
* [Etiketlerin sayısı, eylemlerin sayısı ve ilk actionType](#num-tags-actions)

## <a name="prerequisites"></a>Önkoşullar

* Visual Studio 2017. Gerekirse ücretsiz topluluk sürümünü şuradan indirebilirsiniz: https://www.visualstudio.com/vs/community/.
* Bu hızlı başlangıçta bir abonelik S9 fiyat katmanı gösterildiği gibi başlatmanız gerekecek [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/search-api/). 

Azure portalında bir abonelik başlatmak için:
1. 'BingSearchV7' ifadesini içeren Azure portalının üst metin kutusuna girin `Search resources, services, and docs`.  
2. Aşağı açılan listesinde Marketi bölümünde seçin `Bing Search v7`.
3. Girin `Name` yeni kaynak için.
4. Seçin `Pay-As-You-Go` abonelik.
5. Seçin `S9` fiyatlandırma katmanı.
6. Tıklayın `Enable` abonelik başlatmak için.

* .NET Core SDK'sını, .NET Core 1.1 uygulamalarını çalıştırma olanağı. CORE, Framework ve Çalışma Zamanı sürümlerini şuradan alabilirsiniz: https://www.microsoft.com/net/download/.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları

Bing Web Araması SDK'sını kullanarak bir konsol uygulaması kurmak için Visual Studio’da Çözüm Gezgini'nde `Manage NuGet Packages` seçeneğine gidin.  `Microsoft.Azure.CognitiveServices.Search.VisualSearch` paketini ekleyin.

[NuGet Web Araması SDK'sı paketini](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.VisualSearch/1.0) yüklemek, aşağıdakiler dahil bağımlılık dosyalarını da yükler:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

<a name="client"></a>

## <a name="visual-search-client"></a>Görsel Arama istemcisi
`VisualSearchAPI` istemcisinin bir örneğini oluşturmak için şu "using" yönergelerini ekleyin:

```csharp
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
```

Ardından, istemciyi örneklendirin:

```csharp
var client = new WebSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
```

İstemciyi görüntü aramak için kullanın:

```csharp
 System.IO.FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open;
 // The knowledgeRequest parameter is not required if an image binary is passed in the request body
 var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: (string)null).Result;
```

Önceki sorgunun sonuçlarını ayrıştırın:

```csharp
// Visual Search results
if (visualSearchResults.Image?.ImageInsightsToken != null)
{
    Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
}
else
{
    Console.WriteLine("Couldn't find image insights token!");
}

// List of tags
if (visualSearchResults.Tags.Count > 0)
{
    var firstTagResult = visualSearchResults.Tags.First();
    Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

    // List of actions in first tag
    if (firstTagResult.Actions.Count > 0)
    {
        var firstActionResult = firstTagResult.Actions.First();
        Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
        Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
    }
    else
    {
        Console.WriteLine("Couldn't find tag actions!");
    }
```

<a name="complete"></a> 

## <a name="complete-console-application"></a>Tam konsol uygulaması

Aşağıdaki konsol uygulaması, önceden tanımlanmış sorguyu yürütür ve sonuçları ayrıştırır:

```csharp
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
using System;
using System.IO;
using System.Linq;

namespace VisualSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {
            String subKey = "YOUR-SUBSCRIPTION-KEY";

            VisualSearchImageBinary(subKey);
            //VisualSearchImageBinaryWithCropArea(subKey);
            //VisualSearchUrlWithFilters(subKey);
            //VisualSearchInsightsTokenWithCropArea(subKey);
            //VisualSearchUrlWithJson(subKey);

            Console.WriteLine("\r\nAny key to quit...");
            Console.ReadKey();
        }


        // This will send an image binary in the body of the post request and print out the imageInsightsToken, 
        //  the number of tags, the number of actions, and the first actionType.

        public static void VisualSearchImageBinary(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                using (System.IO.FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open))
                {
                    // The knowledgeRequest parameter is not required if an image binary is passed in the request body
                    var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: (string)null).Result;
                    Console.WriteLine("\r\nVisual search request with binary of image");

                    if (visualSearchResults == null)
                    {
                        Console.WriteLine("No visual search result data.");
                    }
                    else
                    {
                        // Visual Search results
                        if (visualSearchResults.Image?.ImageInsightsToken != null)
                        {
                            Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image insights token!");
                        }

                        // List of tags
                        if (visualSearchResults.Tags.Count > 0)
                        {
                            var firstTagResult = visualSearchResults.Tags.First();
                            Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                            // List of actions in first tag
                            if (firstTagResult.Actions.Count > 0)
                            {
                                var firstActionResult = firstTagResult.Actions.First();
                                Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                                Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                            }
                            else
                            {
                                Console.WriteLine("Couldn't find tag actions!");
                            }

                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image tags!");
                        }
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

Bing arama örnekleri SDK'nın çeşitli özelliklerini gösterir.  Aşağıdaki işlevleri önceden tanımlanmış `VisualSrchSDK` sınıfına ekleyin.

<a name="binary-crop"></a>

## <a name="image-binary-post-with-croparea"></a>cropArea ile görüntü ikili gönderisi

Aşağıdaki kod, gönderi isteğinin gövdesinde cropArea nesnesinin yanı sıra görüntünün ikili verilerini de gönderir.  Ardından imageInsightsToken değerini, etiket sayısını, eylem sayısını ve ilk actionType değerini yazdırır.

```csharp
public static void VisualSearchImageBinaryWithCropArea(string subscriptionKey)
{
    var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

    try
    {
        using (FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open))
        {
            // The ImageInfo struct contains a crop area specifying a region to crop in the uploaded image
            CropArea CropArea = new CropArea(top: (float)0.1, bottom: (float)0.5, left: (float)0.1, right: (float)0.9);
            ImageInfo ImageInfo = new ImageInfo(cropArea: CropArea);
            VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo);

            // The knowledgeRequest here holds additional information about the image, which is passed in in binary form
            var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: VisualSearchRequest).Result;
            Console.WriteLine("\r\nVisual search request with binary of image with knowledgeRequest of CropArea");

            if (visualSearchResults == null)
            {
                Console.WriteLine("No visual search result data.");
            }
            else
            {
                // Visual Search results
                if (visualSearchResults.Image?.ImageInsightsToken != null)
                {
                    Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                }
                else
                {
                    Console.WriteLine("Couldn't find image insights token!");
                }

                // List of tags
                if (visualSearchResults.Tags.Count > 0)
                {
                    var firstTagResult = visualSearchResults.Tags.First();
                    Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                    // List of actions in first tag
                    if (firstTagResult.Actions.Count > 0)
                    {
                        var firstActionResult = firstTagResult.Actions.First();
                        Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                        Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find tag actions!");
                    }

                }
                else
                {
                    Console.WriteLine("Couldn't find image tags!");
                }
            }
        }
    }

    catch (Exception ex)
    {
        Console.WriteLine("Encountered exception. " + ex.Message);
    }
}
```

<a name="knowledge-req"></a>

## <a name="knowledgerequest-parameter"></a>KnowledgeRequest parametresi

Aşağıdaki kod, `knowledgeRequest` parametresinde bir \"site:pinterest.com\" filtresi ile birlikte bir resim URL'si gönderir.  Ardından `imageInsightsToken` değerini, etiket sayısını, eylem sayısını ve ilk actionType değerini yazdırır.

```csharp
public static void VisualSearchUrlWithFilters(string subscriptionKey)
{
    var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

    try
    {
        // The image can be specified via URL, in the ImageInfo object
        var ImageUrl = "https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80";
        ImageInfo ImageInfo = new ImageInfo(url: ImageUrl);

        // Optional filters inside the knowledgeRequest will restrict similar products and images to certain domains
        Filters Filters = new Filters(site: "pinterest.com");
        KnowledgeRequest KnowledgeRequest = new KnowledgeRequest(filters: Filters);

        // An image binary is not necessary here, as the image is specified via URL
        VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo, knowledgeRequest: KnowledgeRequest);
        var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
        Console.WriteLine("\r\nVisual search request with url of image and knowledgeRequest based on filters");

        if (visualSearchResults == null)
        {
            Console.WriteLine("No visual search result data.");
        }
        else
        {
            // Visual Search results
            if (visualSearchResults.Image?.ImageInsightsToken != null)
            {
                Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
            }
            else
            {
                Console.WriteLine("Couldn't find image insights token!");
            }

            // List of tags
            if (visualSearchResults.Tags.Count > 0)
            {
                var firstTagResult = visualSearchResults.Tags.First();
                Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                // List of actions in first tag
                if (firstTagResult.Actions.Count > 0)
                {
                    var firstActionResult = firstTagResult.Actions.First();
                    Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                    Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                }
                else
                {
                    Console.WriteLine("Couldn't find tag actions!");
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
```

<a name="tags-actions"></a>

## <a name="tags-actions-and-actiontype"></a>Etiketler, eylemler ve actionType

Aşağıdaki kod, knowledgeRequest parametresinde cropArea nesnesi ile birlikte bir görüntü içgörüleri belirteci gönderir.  Ardından imageInsightsToken değerini, etiket sayısını, eylem sayısını ve ilk actionType değerini yazdırır.

```csharp
public static void VisualSearchInsightsTokenWithCropArea(string subscriptionKey)
{
    var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

    try
    {
        // The image can be specified via an insights token, in the ImageInfo object
        var ImageInsightsToken = "bcid_CA6BDBEA28D57D52E0B9D4B254F1DF0D*ccid_6J+8V1zi*thid_R.CA6BDBEA28D57D52E0B9D4B254F1DF0D";

        // An optional crop area can be passed in to define a region of interest in the image
        CropArea CropArea = new CropArea(top: (float)0.1, bottom: (float)0.5, left: (float)0.1, right: (float)0.9);
        ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: ImageInsightsToken, cropArea: CropArea);

        // An image binary is not necessary here, as the image is specified via insights token
        VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo);
        var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
        Console.WriteLine("\r\nVisual search request with ImageInsightsToken and knowledgeRequest based on imageInfo of cropArea");

        if (visualSearchResults == null)
        {
            Console.WriteLine("No visual search result data.");
        }
        else
        {
            // Visual Search results
            if (visualSearchResults.Image?.ImageInsightsToken != null)
            {
                Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
            }
            else
            {
                Console.WriteLine("Couldn't find image insights token!");
            }

            // List of tags
            if (visualSearchResults.Tags.Count > 0)
            {
                var firstTagResult = visualSearchResults.Tags.First();
                Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                // List of actions in first tag
                if (firstTagResult.Actions.Count > 0)
                {
                    var firstActionResult = firstTagResult.Actions.First();
                    Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                    Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                }
                else
                {
                    Console.WriteLine("Couldn't find tag actions!");
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
```

<a name="num-tags-actions"></a>

## <a name="number-of-tags-number-of-actions-and-first-actiontype"></a>Etiketlerin sayısı, eylemlerin sayısı ve ilk actionType

Aşağıdaki kod, knowledgeRequest parametresinde bir resim URL'si ve bir kırpma alanı gönderir.  Ardından imageInsightsToken değerini, etiket sayısını, eylem sayısını ve ilk actionType değerini yazdırır.

```csharp
public static void VisualSearchUrlWithJson(string subscriptionKey)
{
    var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

    try
    {

        //The visual search request can be passed in as a JSON string
        //The image is specified via URL in the ImageInfo object, along with a crop area as shown below:
        
        //     "imageInfo": {
        //         "url": "https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80",
        //     "cropArea": {
        //           "top": 0.1,
        //           "bottom": 0.5,
        //           "left": 0.1,
        //           "right": 0.9
        //         }
        //     },
        //     "knowledgeRequest": {
        //        "filters": {
        //            "site": "pinterest.com"
        //        }              
        //     }

        var VisualSearchRequestJSON = "{\"imageInfo\":{\"url\":\"https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80\",\"cropArea\":{\"top\":0.1,\"bottom\":0.5,\"left\":0.1,\"right\":0.9}},\"knowledgeRequest\":{\"filters\":{\"site\":\"pinterest.com\"}}}";

        // An image binary is not necessary here, as the image is specified by URL in JSON text
        var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequestJSON).Result;
        Console.WriteLine("\r\nVisual search request with image url specified by JSON text");

        if (visualSearchResults == null)
        {
            Console.WriteLine("No visual search result data.");
        }
        else
        {
            // Visual Search results
            if (visualSearchResults.Image?.ImageInsightsToken != null)
            {
                Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
            }
            else
            {
                Console.WriteLine("Couldn't find image insights token!");
            }

            // List of tags
            if (visualSearchResults.Tags.Count > 0)
            {
                var firstTagResult = visualSearchResults.Tags.First();
                Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                // List of actions in first tag
                if (firstTagResult.Actions.Count > 0)
                {
                    var firstActionResult = firstTagResult.Actions.First();
                    Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                    Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                }
                else
                {
                    Console.WriteLine("Couldn't find tag actions!");
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
```

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel Hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7).
