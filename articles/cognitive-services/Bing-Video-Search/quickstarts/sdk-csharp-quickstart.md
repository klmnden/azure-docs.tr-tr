---
title: "Hızlı Başlangıç: Bing Video arama için SDK'sı kullanarak video arayınC#"
titleSuffix: Azure Cognitive Services
description: Bing Video arama için SDK'sı kullanarak video arama istekleri göndermek için bu hızlı başlangıçta kullanmak C#.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 06/26/2019
ms.author: aahi
ms.openlocfilehash: 3673f18ff38b2ae98180f470b9f76f1fc57ee8b6
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442529"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-c"></a>Hızlı Başlangıç: Bing Video arama için SDK ile video aramasıC#

Bing Video arama için SDK'sı ile haberler için aramaya başlamak için bu hızlı başlangıçta kullanmak C#. Bing Video arama çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingVideoSearch) ilave ek açıklamaların ve özelliklerine sahip.

## <a name="prerequisites"></a>Önkoşullar

* Herhangi bir sürümünü [Visual Studio 2017 veya üstü](https://visualstudio.microsoft.com/downloads/).
* Kullanılabilir Json.NET framework [bir NuGet paketi olarak](https://www.nuget.org/packages/Newtonsoft.Json/).

Bing Video arama SDK'sını projenize eklemek için seçin **NuGet paketlerini Yönet** gelen **Çözüm Gezgini** Visual Studio'da. `Microsoft.Azure.CognitiveServices.Search.VideoSearch` paketini ekleyin.

Yükleme [[videoyu Search SDK'sı NuGet paketi]](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.VideoSearch/1.2.0) de aşağıdaki bağımlılıkları yükler:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]


## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Yeni bir C# çözümünü Visual Studio'da konsol. Ardından ana kod dosyasına aşağıdakileri ekleyin.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.VideoSearch;
    using Microsoft.Azure.CognitiveServices.Search.VideoSearch.Models;
    ```

2. İstemci yeni bir örneğini `ApiKeyServiceClientCredentials` nesne abonelik anahtarınız ve yapılandırıcının çağrılmasıyla.

    ```csharp
    var client = new VideoSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```

## <a name="send-a-search-request-and-process-the-results"></a>Arama isteği göndermek ve sonuçları işlemek

1. İstemci, bir arama isteği göndermek üzere kullanın. "SwiftKey" için arama sorgusu kullanın.

    ```csharp
    var videoResults = client.Videos.SearchAsync(query: "SwiftKey").Result;
    ```

2. Hiçbir sonuç döndürülmedi, birinci ile alma `videoResults.Value[0]`. Ardından, videonun kimliği, başlık ve url yazdırın.

    ```csharp
    if (videoResults.Value.Count > 0)
    {
        var firstVideoResult = videoResults.Value[0];

        Console.WriteLine($"\r\nVideo result count: {videoResults.Value.Count}");
        Console.WriteLine($"First video id: {firstVideoResult.VideoId}");
        Console.WriteLine($"First video name: {firstVideoResult.Name}");
        Console.WriteLine($"First video url: {firstVideoResult.ContentUrl}");
    }
    else
    {
        Console.WriteLine("Couldn't find video results!");
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı bir web uygulaması oluşturma](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing Video arama API'si nedir?](../overview.md)
* [Bilişsel Hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
