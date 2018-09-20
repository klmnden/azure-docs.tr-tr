---
title: Özel arama SDK C# hızlı başlangıç | Microsoft Docs
titleSuffix: Cognitive Services
description: Özel arama SDK C# konsol uygulaması ayarlayın.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 01/31/2018
ms.author: rosh
ms.openlocfilehash: 6b41dfbde0c2af776ee2c35220f731e40de334a0
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46367857"
---
# <a name="custom-search-sdk-c-quickstart"></a>Özel Arama SDK’sı C# hızlı başlangıcı

Bing özel arama SDK'sı, varlık arama ve sonuçlar ayrıştırma için REST API işlevselliğini içerir.

Bu örnek için kaynak kod un kullanılabilir [Github](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingCustomWebSearch).
## <a name="application-dependencies"></a>Uygulama bağımlılıkları

Bing özel arama SDK'sını kullanarak bir konsol uygulaması oluşturma için göz atın `Manage NuGet Packages` Visual Studio'daki Çözüm Gezgini'nden seçeneği. `Microsoft.Azure.CognitiveServices.Search.CustomSearch` paketini ekleyin.

Yükleme [NuGet özel arama](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) paket bağımlılıkları, aşağıdaki derlemeler de dahil olmak üzere da yükler:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="entity-search-client"></a>Varlık arama istemci

CustomSearchAPI istemcisi örneği oluşturmak için using yönergelerini ekleyin:
```
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

```

Özel arama istemci örneği: değiştirin `YOUR-CUSTOM-SEARCH-KEY` ve `YOUR-CUSTOM-CONFIG-ID` erişim anahtarına ve Kimliğine atanmış, API uç nokta Yapılandırması [My örnekleri](https://www.customsearch.ai/).
```
var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));

```
İstemci ile bir sorgu metni aramak için kullanın:
```
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;

```
## <a name="parse-the-results"></a>Sonuçları ayrıştırılamıyor

`SearchAsync` Yöntemi döndürür bir `WebData` içeren nesne `WebPages` herhangi bir sorgu için bulunması durumunda. Bu kod ilk sonuç bulur ve alır, `Name` ve `URL`.
```
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
 
Console.WriteLine("Searched for Query# \" Xbox \"");

 //WebPages
if (webData?.WebPages?.Value?.Count > 0)
{
    // find the first web page
    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

    if (firstWebPagesResult != null)
    {
        Console.WriteLine("Webpage Results#{0}", webData.WebPages.Value.Count);
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

```
## <a name="complete-console-application"></a>Tam konsol uygulaması

Aşağıdaki kod, arama sorgusu "Xbox" ve yazdırır `Name` ve `URL` ilk web sonucu.
```
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

namespace CustomSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {

            var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));

            try
            {
                // This will look up a single query (Xbox).
                var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
                Console.WriteLine("Searched for Query# \" Xbox \"");

                //WebPages
                if (webData?.WebPages?.Value?.Count > 0)
                {
                    // find the first web page
                    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

                    if (firstWebPagesResult != null)
                    {
                        Console.WriteLine("Webpage Results#{0}", webData.WebPages.Value.Count);
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
            }
            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

            Console.WriteLine("Any key to exit...");
            Console.ReadKey();
        }

    }
}


```

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel Hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
