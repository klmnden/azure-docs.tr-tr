---
title: "Hızlı Başlangıç: Bing özel arama kullanan uç noktasını çağırmak C# SDK'sı | Microsoft Docs"
titleSuffix: Azure Cognitive Services
description: Özel Arama SDK'sı C# konsol uygulamasını kurma.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 09/06/2018
ms.author: scottwhi
ms.openlocfilehash: 100f1d4ed96f04f8208fd544d410f227561343e6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60955561"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-the-c-sdk"></a>Hızlı Başlangıç: Bing özel arama kullanan uç noktasını çağırmak C# SDK'sı 

Bing özel arama örneğinizin, arama sonuçlarını talep başlamak için bu hızlı başlangıçta kullanmak kullanarak C# SDK. Bing özel arama çoğu programlama dilleri ile uyumlu bir REST API olsa da Bing özel arama SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingCustomWebSearch).

## <a name="prerequisites"></a>Önkoşullar

- Bing özel arama örneği için. Bkz: [hızlı başlangıç: İlk Bing özel arama örneğinizin oluşturma](quick-start.md) daha fazla bilgi için.
- Microsoft [.NET Core](https://www.microsoft.com/net/download/core)
- Herhangi bir sürümünü [Visual Studio 2017](https://www.visualstudio.com/downloads/)
- Linux/MacOS kullanıyorsanız bu uygulama, [Mono](https://www.mono-project.com/) kullanılarak çalıştırılabilir.
- [NuGet Özel Arama](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) paketinin yüklenmiş olması. 
    - Visual Studio'daki Çözüm Gezgini'nde projenize sağ tıklayıp menüden `Manage NuGet Packages` öğesini seçin. `Microsoft.Azure.CognitiveServices.Search.CustomSearch` paketini yükleyin. NuGet Özel Arama paketini yüklediğinizde aşağıdaki derlemeler de yüklenir:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-news-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Visual Studio'da yeni bir C# konsol uygulaması oluşturun. Ardından projenize aşağıdaki paketleri ekleyin.

    ```csharp
    using System;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.CustomSearch;
    ```

2. Uygulamanızın ana yöntemde API anahtarınızı arama istemci örneği oluşturun.

    ```csharp
    var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-SUBSCRIPTION-KEY"));
    ```

## <a name="send-the-search-request-and-receive-a-response"></a>Arama isteği gönderme ve yanıt
    
1. İstemcinizin kullanarak bir arama sorgusu gönderin `SearchAsync()` yöntemi ve yanıtı kaydedin. Değiştirdiğinizden emin olun, `YOUR-CUSTOM-CONFIG-ID` örneğinizin yapılandırma kimliği ile (kimliği bulabilirsiniz [Bing özel arama portalı](https://www.customsearch.ai/)). Bu örnekte, "Xbox" için arama yapar.

    ```csharp
    // This will look up a single query (Xbox).
    var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
    ```

2. `SearchAsync()` metodu bir `WebData` nesnesi döndürür. Nesne herhangi yineleme yapmak için kullanın `WebPages` , bulundu. Bu kod ilk web sayfası sonucunu bulur ve web sayfasının `Name` ile `URL` değerlerini yazdırır.

    ```csharp
    if (webData?.WebPages?.Value?.Count > 0)
    {
        // find the first web page
        var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

        if (firstWebPagesResult != null)
        {
            Console.WriteLine("Number of webpage results {0}", webData.WebPages.Value.Count);
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
    ```csharp

## Next steps

> [!div class="nextstepaction"]
> [Build a Custom Search web app](./tutorials/custom-search-web-page.md)
