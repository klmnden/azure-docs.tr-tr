---
title: 'Hızlı Başlangıç: Bing özel arama kullanan uç noktasını çağırmak C# | Microsoft Docs'
titlesuffix: Azure Cognitive Services
description: Bing özel arama örneğinizin gelen arama sonuçlarını talep başlamak için bu hızlı başlangıçta kullanmak C#.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: maheshb
ms.openlocfilehash: a775c1c864a8a5513be546195da5c0891f8bb1f6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61066181"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-c"></a>Hızlı Başlangıç: Bing özel arama kullanan uç noktasını çağırmaC# 

Bing özel arama örneğinizin arama sonuçlarını talep başlamak için bu Hızlı Başlangıç'ı kullanın. Bu uygulamanın yazıldığı sırada C#, Bing özel arama API'si bir RESTful web çoğu programlama dilleri ile uyumlu bir hizmettir. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingCustomSearchv7.cs).

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

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Visual Studio'da yeni bir C# konsol uygulaması oluşturun. Ardından projenize aşağıdaki paketleri ekleyin.

    ```csharp
    using System;
    using System.Net.Http;
    using System.Web;
    using Newtonsoft.Json;
    ```

2. Bing özel arama API'si tarafından döndürülen arama sonuçlarını depolamak için aşağıdaki sınıflar oluşturun.

    ```csharp
    public class BingCustomSearchResponse {        
        public string _type{ get; set; }            
        public WebPages webPages { get; set; }
    }

    public class WebPages {
        public string webSearchUrl { get; set; }
        public int totalEstimatedMatches { get; set; }
        public WebPage[] value { get; set; }        
    }

    public class WebPage {
        public string name { get; set; }
        public string url { get; set; }
        public string displayUrl { get; set; }
        public string snippet { get; set; }
        public DateTime dateLastCrawled { get; set; }
        public string cachedPageUrl { get; set; }
    }
    ```

3. Projenizin ana yöntemde, Bing özel arama API'si abonelik anahtarınız, arama örneğinizin özel yapılandırma kimliği ve bir arama terimi için değişkenler oluşturun.

    ```csharp
    var subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
    var customConfigId = "YOUR-CUSTOM-CONFIG-ID";
    var searchTerm = args.Length > 0 ? args[0]:"microsoft";
    ```

4. Arama teriminizi ekleyerek istek URL'si oluşturmak `q=` sorgu parametresi ve yapılandırma kimliği özel arama örneğinizin `customconfig=`. parametrelerle ayrı bir `&` karakter. 

    ```csharp
    var url = "https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?" +
                "q=" + searchTerm + "&" +
                "customconfig=" + customConfigId;
    ```

## <a name="send-and-receive-a-search-request"></a>Arama isteği gönderip 

1. Bir istek istemci oluşturmanız ve abonelik anahtarınızı ekleme `Ocp-Apim-Subscription-Key` başlığı.

    ```csharp
    var client = new HttpClient();
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
    ```

2. Arama isteği gerçekleştirmek ve bir JSON nesnesi olarak yanıt alın.

    ```csharp
    var httpResponseMessage = client.GetAsync(url).Result;
    var responseContent = httpResponseMessage.Content.ReadAsStringAsync().Result;
    BingCustomSearchResponse response = JsonConvert.DeserializeObject<BingCustomSearchResponse>(responseContent);
    ```
   ## <a name="process-and-view-the-results"></a>Sonuçları işleme ve görüntüleme

1. Adı, url ve tarihi dahil olmak üzere, her bir arama sonucu ile ilgili Web sayfasında son gezinilen bilgileri görüntülemek için yanıt nesnesi yineleme yapma.

    ```csharp
    for(int i = 0; i < response.webPages.value.Length; i++) {                
        var webPage = response.webPages.value[i];
        
        Console.WriteLine("name: " + webPage.name);
        Console.WriteLine("url: " + webPage.url);                
        Console.WriteLine("displayUrl: " + webPage.displayUrl);
        Console.WriteLine("snippet: " + webPage.snippet);
        Console.WriteLine("dateLastCrawled: " + webPage.dateLastCrawled);
        Console.WriteLine();
    }
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir özel arama web uygulaması derleme](./tutorials/custom-search-web-page.md)
