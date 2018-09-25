---
title: Özel arama SDK C# hızlı başlangıç | Microsoft Docs
titleSuffix: Cognitive Services
description: Özel arama SDK C# konsol uygulaması ayarlayın.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/06/2018
ms.author: scottwhi
ms.openlocfilehash: 6c9917e3a63515f36b386e444edcc53de07799fc
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949936"
---
# <a name="c-sdk-quickstart"></a>C# SDK'sını hızlı başlangıç

Bing özel arama SDK'sı, Bing özel arama REST API'si daha bir daha kolay bir programlama modeli sağlar. Bu bölüm C# SDK'sını kullanarak ilk özel arama çağrılarınızı yapmadan aracılığıyla size yol gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Bir kullanıma hazır özel arama örneği. Bkz: [ilk Bing özel arama örneğinizin oluşturma](quick-start.md).  
  
- Bir abonelik anahtarı. Etkinleştirme sırasında bir abonelik anahtarı edinirler, [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search), ya da Ücretli abonelik anahtarı, Azure panosundan kullanabilirsiniz (bkz [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).  
  
- Visual Studio 2017 yüklü. Zaten sahip değilseniz, indirebileceğiniz **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).  
  
- [NuGet özel arama](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) yüklü paket. Visual Studio'da Çözüm Gezgini'nden, projeyi sağ tıklatın `Manage NuGet Packages` menüsünde. `Microsoft.Azure.CognitiveServices.Search.CustomSearch` paketini yükleyin.

Özel arama NuGet paketi yüklerken aşağıdaki derlemelerini yükler:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json



## <a name="run-the-code"></a>Kodu çalıştırma

Bu örneği çalıştırmak için aşağıdaki adımları izleyin:

1. Visual Studio 2017'yi açın.
  
2. Tıklayın **dosya** menüsünde tıklatın **yeni**, **proje**ve ardından **Visual C# konsol uygulaması** şablonu.
  
3. Çözümünüzü CustomSearchSolution adlandırın ve talimatı klasöre göz atın.
  
4. Çözümü oluşturmak için Tamam'a tıklayın.  
  
4. Çözüm Gezgini'nden projenize (Çözüm aynı ada sahiptir) sağ tıklayıp `Manage NuGet Packages`. Tıklayın **Gözat** NuGet Paket Yöneticisi'nde. Microsoft.Azure.CognitiveServices.Search.CustomSearch arama kutusuna girin ve enter tuşuna basın. Paketi seçin ve ardından Yükle'ye tıklayın.  
  
4. Aşağıdaki kodu, Program.cs dosyasına kopyalayın. Değiştirin **YOUR-SUBSCRIPTION-KEY** ve **YOUR-özel-CONFIG-ID** abonelik anahtarınızı ve yapılandırma kimliği  
  
    ```csharp
    using System;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

    namespace CustomSrchSDK
    {
        class Program
        {
            static void Main(string[] args)
            {

                var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-SUBSCRIPTION-KEY"));

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
                }
                catch (Exception ex)
                {
                    Console.WriteLine("Encountered exception. " + ex.Message);
                }

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

        }
    }
    ```  
  
5. Derleme ve çalıştırma (hata ayıklama) çözümü. 




## <a name="breaking-it-down"></a>Bozucu

Kullanarak bir eklemenize gerek özel arama NuGet paketini yükledikten sonra bunu yönergesi.

```csharp
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;
```

Ardından, arama istekleri yapma özel arama istemci örneği oluşturun. Değiştirdiğinizden emin olun `YOUR-SUBSCRIPTION-KEY` anahtarınızı.

```csharp
var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));
```

Artık bir arama talebi göndermek için istemciyi kullanabilirsiniz. Değiştirdiğinizden emin olun, `YOUR-CUSTOM-CONFIG-ID` örneğinizin yapılandırma kimliği ile (kimliği bulabilirsiniz [özel arama portalı](https://www.customsearch.ai/)). Bu örnek için Xbox arar. Gerektiği şekilde güncelleştirme.

```csharp
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
```

`SearchAsync` Yöntemi döndürür bir `WebData` nesne. Kullanım `WebData` herhangi yinelemek için `WebPages` , bulundu. Bu kod ilk Web sayfası sonucu bulur ve Web sayfasının yazdırır `Name` ve `URL`.

```csharp
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


## <a name="next-steps"></a>Sonraki adımlar

SDK'sı örneklerini gözden denetleyin. Her örnek, Önkoşullar ve yükleme/çalışan örnekleri ayrıntılarını içeren bir benioku dosyası içerir.

* Kullanmaya başlama [.NET örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7) 
    * [NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0)
    * Ayrıca bkz: [.NET kitaplıkları](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingCustomSearch) tanımlar ve bağımlılıkları için.
* Kullanmaya başlama [NodeJS örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * Ayrıca bkz: [NodeJS kitaplıkları](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/customSearch) tanımlar ve bağımlılıkları için.
* Kullanmaya başlama [Java örnekleri](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * Ayrıca bkz: [Java kitaplıkları](https://github.com/Azure/azure-sdk-for-java/tree/master/cognitiveservices/azure-customsearch) tanımlar ve bağımlılıkları için.
* Kullanmaya başlama [Python örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * Ayrıca bkz: [Python kitaplıkları](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-customsearch) tanımlar ve bağımlılıkları için.

