---
title: Önceki arama ImageInsightsToken - Bing görsel arama kullanarak benzer görüntülerden Bul
titleSuffix: Azure Cognitive Services
description: Bing görsel arama SDK ImageInsightsToken tarafından belirtilen görüntülerin URL'lerini almak için kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: article
ms.date: 06/18/2019
ms.author: rosh
ms.openlocfilehash: 78584c2c0419bb27fb58c07eb97b1aa38501951f
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204075"
---
# <a name="find-similar-images-from-previous-searches-using-imageinsightstoken"></a>Önceki arama ImageInsightsToken kullanarak benzer görüntülerden Bul

Visual Search SDK'sı döndüren önceki aramaları çevrimiçi resimleri bulmak sağlayan bir `ImageInsightsToken`. Bu uygulamayı alır bir `ImageInsightsToken` ve bir sonraki arama belirtecini kullanır. Ardından gönderir `ImageInsightsToken` Bing arama URL'leri ve çevrimiçi bulunan benzer görüntülerin URL'lerini dahil sonuçlarına Bing ve döndürür.

Bu öğretici için tam kaynak kodunu ek hata işleme ve ek açıklamalar ile bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchInsightsTokens.cs).

## <a name="prerequisites"></a>Önkoşullar

* Herhangi bir sürümünü [Visual Studio 2019](https://www.visualstudio.com/downloads/).
* Linux/MacOS kullanıyorsanız, bu uygulamayı kullanarak çalıştırabilirsiniz [Mono](https://www.mono-project.com/).
* NuGet görsel arama ve resim arama paketler.
    - Visual Studio'da Çözüm Gezgini'nden, projeyi sağ tıklatın **NuGet paketlerini Yönet** menüsünde. Yükleme `Microsoft.Azure.CognitiveServices.Search.CustomSearch` paketi ve `Microsoft.Azure.CognitiveServices.Search.ImageSearch` paket. NuGet paketlerini yükleme Ayrıca aşağıdakileri yükler:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="get-the-imageinsightstoken-from-the-bing-image-search-sdk"></a>Bing resim arama SDK ImageInsightsToken Al

Bu uygulamayı kullanan bir `ImageInsightsToken` edinilen [Bing resim arama SDK](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart). Yeni C# konsol uygulaması, bir istemci kullanarak API çağırmak için oluşturma `ImageSearchClient()`. Ardından `SearchAsync()` sorgunuzla:

```csharp
var client = new ImageSearchClient(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey));
var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
Console.WriteLine("Search images for query \"canadian rockies\"");
```

İlk arama sonucu kullanarak Store `imageResults.Value.First()`ve resim InSight'ın depolayın `ImageInsightsToken`.

```csharp
String insightTok = "None";
if (imageResults.Value.Count > 0)
{
    var firstImageResult = imageResults.Value.First();
    insightTok = firstImageResult.ImageInsightsToken;
}
else
{
    insightTok = "None found";
    Console.WriteLine("Couldn't find image results!");
}
```

Bu `ImageInsightsToken` Bing görsel arama, bir istek gönderilir.

## <a name="add-the-imageinsightstoken-to-a-visual-search-request"></a>Görsel arama ImageInsightsToken ekleyin

Belirtin `ImageInsightsToken` oluşturarak bir görsel arama isteği için bir `ImageInfo` nesnesinden `ImageInsightsToken` Bing görsel arama alınan yanıtları yer alan.

```csharp
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```

## <a name="use-bing-visual-search-to-find-images-from-an-imageinsightstoken"></a>Bir ImageInsightsToken görüntüleri bulmak için Bing görsel arama'yı kullanın

`VisualSearchRequest` Nesne görüntüde hakkında bilgiler içerir `ImageInfo` aranacak. `VisualSearchMethodAsync()` yöntemi sonuçları alır. Görüntü belirteci tarafından temsil edilen bir görüntü ikili sağlamanıza gerek yoktur.

```csharp
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;

```

## <a name="iterate-through-the-visual-search-results"></a>Görsel arama sonuçlarını yinelemek

Görsel Arama sonuçları `ImageTag` nesneleridir. Her etiket bir `ImageAction` nesneleri listesi içerir. Her `ImageAction` içeren bir `Data` eylem türüne bağlı olan değerler listesini alanı. Üzerinden yinelemek `ImageTag` nesneler `visualSearchResults.Tags`örneği ve get için `ImageAction` içindeki etiketi. Aşağıdaki örnekte ayrıntılarını yazdırır `PagesIncluding` eylemler:

```csharp
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
```

### <a name="pagesincluding-actiontypes"></a>PagesIncluding ActionTypes

Eylem türlerinden gerçek resim URL'leri alma gerektirir okuyan bir cast bir `ActionType` olarak `ImageModuleAction`, içeren bir `Data` değerleri bir liste öğesi. Her değer, bir görüntünün URL’sidir.  Aşağıdaki yayınları `PagesIncluding` eylem türü `ImageModuleAction` ve değerlerini okur:

```csharp
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

Bu veri türleri hakkında daha fazla bilgi için bkz. [Görüntüler - Görsel Arama](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch).

## <a name="returned-urls"></a>Döndürülen URL'leri

Uygulamanın, aşağıdaki URL'ler döndürür:

|EylemTürü  |URL'si  | |
|---------|---------|---------|
|MoreSizes -> WebSearchUrl     |         |
|VisualSearch -> WebSearchUrl     |         |
|ImageById -> WebSearchUrl    |         |
|RelatedSearches WebSearchUrl ->:    |         |
|DocumentLevelSuggestions WebSearchUrl ->:     |         |
|TopicResults -> WebSearchUrl    | [https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&amp;CID=1BA795A21EAF6A63175699B71FC36B7C&amp;rd=1&amp;h=BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&amp;v=1&amp;r=https%3a%2f%2fwww.bing.com%2fdiscover%2fcanadian%2brocky&amp;p=DevEx,5823.1](https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fcanadian%2brocky&p=DevEx,5823.1)       |
|ImageResults -> WebSearchUrl    |  [https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&amp;CID=1BA795A21EAF6A63175699B71FC36B7C&amp;rd=1&amp;h=PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&amp;v=1&amp;r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&amp;p=DevEx,5831.1](https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&p=DevEx,5831.1)       |

Yukarıda gösterildiği gibi `TopicResults` ve `ImageResults` sorguları ilgili görüntüleri için türler bulunur. Bing arama sonuçları URL bağlantısı.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görsel arama tek sayfa web uygulaması oluşturma](tutorial-bing-visual-search-single-page-app.md)
