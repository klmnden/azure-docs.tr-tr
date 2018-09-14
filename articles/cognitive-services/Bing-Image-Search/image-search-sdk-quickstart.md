---
title: "Hızlı Başlangıç: Bing görüntü arama SDK'sı ve C# kullanarak görüntüleri arama"
description: Bu hızlı başlangıçta, arama ve Bing resim arama SDK'sı ve C# kullanarak web üzerinde görüntüleri bulmak için kullanın.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: 9e0decb29224b5ad684e1242b8f93e091e08e6b6
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579261"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-c"></a>Hızlı Başlangıç: Bing görüntü arama SDK'sı ve C# ile görüntüleri arama

Bing görüntü arama API'si için bir sarmalayıcı olan ve aynı özellikleri içeren SDK'yı kullanarak ilk görüntü arama yapmak için bu Hızlı Başlangıç'ı kullanın. Bu basit C# uygulama bir resim arama sorgusu gönderir, JSON yanıtı ayrıştırır ve döndürülen ilk görüntünün URL'sini görüntüler.

Bu örnek için kaynak kodu kullanılabilir [github'da](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingImageSearch) ek hata işleme ve ek açıklamalar.

## <a name="prerequisites"></a>Önkoşullar

* Herhangi bir sürümünü [Visual Studio 2017](https://visualstudio.microsoft.com/vs/whatsnew/).
* [Bilişsel görüntü arama NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.ImageSearch/1.2.0).

Visual Studio'da Bing resim arama SDK'sını yüklemek için kullanın `Manage NuGet Packages` Visual Studio'daki Çözüm Gezgini'nden seçeneği.

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Oluşturma ve uygulama başlatma

İlk olarak, Visual Studio'da yeni bir C# konsol uygulaması oluşturun. Ardından aşağıdaki paketler projenize ekleyin.

```csharp
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch.Models;
```

Projenizin ana yöntemde, Bing ve bir arama terimi döndürülecek görüntü sonuçları değişkenleri için geçerli bir abonelik anahtarınızı oluşturun. Ardından anahtarı kullanılarak görüntü arama istemci örneği.

```csharp
//IMPORTANT: replace this variable with your Cognitive Services subscription key
string subscriptionKey = "ENTER YOUR KEY HERE";
//stores the image results returned by Bing
Images imageResults = null;
// the image search term to be used in the query
string searchTerm = "canadian rockies";
//initialize the client
var client = new ImageSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));
```

## <a name="send-a-search-query-using-the-client"></a>İstemcisini kullanarak bir arama sorgusu gönderin

İstemci ile bir sorgu metni aramak için kullanın:

```csharp
// make the search request to the Bing Image API, and get the results"
imageResults = client.Images.SearchAsync(query: searchTerm).Result; //search query
```

## <a name="parse-and-view-the-first-image-result"></a>Ayrıştırma ve ilk görüntü sonucu görüntülemek

Yanıtta döndürülen resim sonuçları ayrıştırılamıyor.
Yanıt arama sonuçları içeriyorsa, ilk sonucu depolar ve bir küçük resim gibi ayrıntılarını yazdırmak URL, toplam sayısını özgün URL'yi döndürülen görüntüler.  

```csharp
if (imageResults != null)
{
    //display the details for the first image result.
    var firstImageResult = imageResults.Value.First();
    Console.WriteLine($"\nTotal number of returned images: {imageResults.Value.Count}\n");
    Console.WriteLine($"Copy the following URLs to view these images on your browser.\n");
    Console.WriteLine($"URL to the first image:\n\n {firstImageResult.ContentUrl}\n");
    Console.WriteLine($"Thumbnail URL for the first image:\n\n {firstImageResult.ThumbnailUrl}");
    Console.ReadKey();
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing resim arama tek sayfalı uygulama Öğreticisi](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing resim arama nedir?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Çevrimiçi bir etkileşimli Tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Ücretsiz bir Bilişsel hizmetler erişim anahtarını alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Azure Bilişsel hizmetler SDK için .NET örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
* [Azure Bilişsel hizmetler belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing resim arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)