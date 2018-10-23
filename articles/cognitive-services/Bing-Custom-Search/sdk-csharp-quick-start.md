---
title: 'Hızlı başlangıç: Özel Arama SDK’sı, C#'
titleSuffix: Azure Cognitive Services
description: Özel Arama SDK'sı C# konsol uygulamasını kurma.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 09/06/2018
ms.author: scottwhi
ms.openlocfilehash: 6dc10bc2dedfe99573b5ad646461e66827c6df9e
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49320178"
---
# <a name="quickstart-using-the-bing-custom-search-sdk-with-c"></a>Hızlı Başlangıç: C# ile Bing Özel Arama SDK'sını kullanma

Bing Özel Arama SDK'sı, Bing Özel Arama REST API'sine kıyasla daha kolay bir programlama modeli sunar. Bu bölümde C# SDK'sını kullanarak ilk Özel Arama çağrılarınızı oluşturma adımları gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Kullanıma hazır özel arama örneği. Bkz. [İlk Bing Özel Arama örneğinizi oluşturma](quick-start.md).  
  
- Abonelik anahtarı. Abonelik anahtarını [ücretsiz denemenizi](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) etkinleştirdikten sonra alabilir veya Azure panonuzdan ücretli abonelik anahtarı (bkz. [Bilişsel Hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)) kullanabilirsiniz.  
  
- Visual Studio 2017 uygulamasının yüklenmiş olması. Yüklü değilse **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirebilirsiniz.  
  
- [NuGet Özel Arama](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) paketinin yüklenmiş olması. Visual Studio'daki Çözüm Gezgini'nde projenize sağ tıklayıp menüden `Manage NuGet Packages` öğesini seçin. `Microsoft.Azure.CognitiveServices.Search.CustomSearch` paketini yükleyin.

NuGet Özel Arama paketini yüklediğinizde aşağıdaki derlemeler de yüklenir:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json



## <a name="run-the-code"></a>Kodu çalıştırma

> [!TIP]
> Güncel kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingCustomWebSearch)'dan Visual Studio çözümü olarak alın.

Bu örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Visual Studio 2017'yi açın.
  
2. **Dosya** menüsünden **Yeni**, **Proje**'ye ve ardından **Visual C# Konsol Uygulaması** şablonuna tıklayın.
  
3. Çözümünüze CustomSearchSolution adını verin ve eklemek istediğiniz klasörü bulun.
  
4. Tamam'a tıklayarak çözümü oluşturun.  
  
4. Çözüm Gezgini'nde projenize sağ tıklayın (çözümle aynı ada sahiptir) ve `Manage NuGet Packages` öğesini seçin. NuGet Paket Yöneticisi'nde **Gözat**'a tıklayın. Arama kutusuna Microsoft.Azure.CognitiveServices.Search.CustomSearch yazıp Enter tuşuna basın. Paketi seçin ve Yükle'ye tıklayın.  
  
4. Aşağıdaki kodu Program.cs dosyasına kopyalayın. **YOUR-SUBSCRIPTION-KEY** ve **YOUR-CUSTOM-CONFIG-ID** yerine abonelik anahtarınızı ve yapılandırma kimliğinizi yazın.  
  
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
  
5. Çözümü derleyin ve çalıştırın (hata ayıklayın). 




## <a name="breaking-it-down"></a>Bileşenlerine ayırma

NuGet Özel Arama paketini yükledikten sonra bir using yönergesi eklemeniz gerekir.

```csharp
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;
```

Ardından arama isteği oluşturmak için kullandığınız özel arama istemcisini başlatabilirsiniz. `YOUR-SUBSCRIPTION-KEY` yerine anahtarınızı yazdığınızdan emin olun.

```csharp
var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));
```

Artık istemciyi kullanarak bir arama isteği gönderebilirsiniz. `YOUR-CUSTOM-CONFIG-ID` yerine örneğinizin yapılandırma kimliğini ([Özel Arama portalından](https://www.customsearch.ai/) alabilirsiniz) yazmayı unutmayın. Bu örnek Xbox araması yapmaktadır. İstediğiniz şekilde güncelleştirebilirsiniz.

```csharp
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
```

`SearchAsync` metodu bir `WebData` nesnesi döndürür. `WebData` kullanarak bulunan `WebPages` ile yineleyebilirsiniz. Bu kod ilk web sayfası sonucunu bulur ve web sayfasının `Name` ile `URL` değerlerini yazdırır.

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

SDK örneklerini inceleyin. Her örnekte ön gereksinimler ve örnekleri yükleme/çalıştırma hakkında ayrıntılı bilgi içeren bir ReadMe dosyası bulunur.

* [.NET örneklerini](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7) kullanmaya başlama 
    * [NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0)
    * Tanımlar ve bağımlılıklar için ayrıca bkz. [.NET kitaplıkları](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingCustomSearch).
* [Node.js örneklerini](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) kullanmaya başlama 
    * Tanımlar ve bağımlılıklar için ayrıca bkz. [Node.js kitaplıkları](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/customSearch).
* [Java örneklerini](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) kullanmaya başlama 
    * Tanımlar ve bağımlılıklar için ayrıca bkz. [Java kitaplıkları](https://github.com/Azure/azure-sdk-for-java/tree/master/cognitiveservices/azure-customsearch).
* [Python örneklerini](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) kullanmaya başlama 
    * Tanımlar ve bağımlılıklar için ayrıca bkz. [Python kitaplıkları](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-customsearch).

