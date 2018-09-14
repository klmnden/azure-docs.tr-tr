---
title: "Hızlı Başlangıç: Gönderme arama, Bing resim arama API'si ile sorgular ve"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, arama ve Bing Web araması API'si kullanarak web üzerinde görüntüleri bulmak için kullanın.
services: cognitive-services
documentationcenter: ''
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 9/07/2018
ms.author: aahi
ms.openlocfilehash: 22eb54f6adec0335dd89a5ae906117542bb11717
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45580421"
---
# <a name="quickstart-send-search-queries-using-the-bing-image-search-api-and-c"></a>Hızlı Başlangıç: Bing resim arama API'si ve C# kullanarak gönderme arama sorguları

Bu hızlı başlangıçta, Bing resim arama API'si, ilk çağrı yapmak ve bir JSON yanıtı Arama sonuçlarından görüntülemek için kullanın. Bu basit C# uygulaması, API için bir HTTP görüntü arama sorgusu gönderir ve döndürülen ilk görüntünün URL'sini görüntüler.

Bu uygulama C# dilinde yazılmıştır, ancak çoğu programlama dilleri ile uyumlu bir RESTful Web hizmeti API'dir.

Bu örnek için kaynak kodu kullanılabilir [github'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingImageSearchv7Quickstart.cs) ek hata işleme ve kod ek açıklamaları.

## <a name="prerequisites"></a>Önkoşullar

* Herhangi bir sürümünü [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* [Json.NET](https://www.newtonsoft.com/json) framework, bir NuGet paketi olarak kullanılabilir. 
* Linux/MacOS kullanıyorsanız, bu uygulamanın kullanılarak çalıştırılabilir [Mono](http://www.mono-project.com/).

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Oluşturun ve bir proje başlatın

1. adlı yeni bir konsol çözümü oluşturma `BingSearchApisQuickStart` Visual Studio'da. Ardından şu ad alanlarından ana kod dosyasına ekleyin.

    ```csharp
    using System;
    using System.Net;
    using System.IO;
    using System.Collections.Generic;
    using Newtonsoft.Json.Linq;
    ```

2. Abonelik anahtarınızı API uç noktası için değişkenler oluşturun ve arama terimi.

    ```csharp
    //...
    namespace BingSearchApisQuickstart
    {
        class Program
        {
        // Replace the this string with your valid access key.
        const string subscriptionKey = "enter your key here";
        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/search";
        const string searchTerm = "tropical ocean";
    //...
    ```

## <a name="create-a-struct-to-format-the-bing-image-search-response"></a>Bing resim arama yanıt biçimlendirmek için bir yapı oluşturma

Tanımlayan bir `SearchResult` yapı resim araması sonuçları ve JSON üst bilgileri içerir.
    
```csharp
    namespace BingSearchApisQuickstart
    {
        class Program
        {
        //...
            struct SearchResult
            {
                public String jsonResult;
                public Dictionary<String, String> relevantHeaders;
            }
//...
```

## <a name="create-a-method-to-send-search-requests"></a>Arama istekleri göndermek için bir yöntem oluşturma

Adlı bir yöntem oluşturma `BingImageSearch` API çağrısı yapmak ve dönüş türü kümesine `SearchResult` daha önce oluşturduğunuz yapısı.

```csharp
//...
namespace BingSearchApisQuickstart
{
    //...
    class Program
    {
        //...
        static SearchResult BingImageSearch(string searchTerm)
        {
        }
//...
```

## <a name="create-and-handle-an-image-search-request"></a>Oluşturma ve bir görüntü arama isteği işleme
 
İçinde `BingImageSearch` yöntemi, aşağıdaki adımları gerçekleştirin.

1. Arama isteği için URI oluşturur. Arama terimi unutmayın `toSearch` dizesine eklenen önce biçimlendirilmiş olması gerekir. 

    ```csharp
    static SearchResult BingImageSearch(string toSearch){

        var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(toSearch);
    //...
    ```

2. Web isteği gerçekleştirmek ve bir JSON dizesi olarak yanıt alın.

    ```csharp
    WebRequest request = WebRequest.Create(uriQuery);
    request.Headers["Ocp-Apim-Subscription-Key"] = subscriptionKey;
    HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
    string json = new StreamReader(response.GetResponseStream()).ReadToEnd();
    ```

3. Arama sonuç nesnesi oluşturun ve Bing HTTP üstbilgileri ayıklayın. Ardından dönün `searchResult`.

    ```csharp
    // Create the result object for return
    var searchResult = new SearchResult()
    {
        jsonResult = json,
        relevantHeaders = new Dictionary<String, String>()
    };

    // Extract Bing HTTP headers
    foreach (String header in response.Headers)
    {
        if (header.StartsWith("BingAPIs-") || header.StartsWith("X-MSEdge-"))
            searchResult.relevantHeaders[header] = response.Headers[header];
    }
    return searchResult;
    ```

## <a name="process-and-view-the-response"></a>İşlem ve görünümü yanıta

1. Ana yöntemde çağrı `BingImageSearch()` ve döndürülen yanıtı depolayın. Ardından JSON bir nesnede seri durumdan çıkarır.

    ```csharp
    SearchResult result = BingImageSearch(searchTerm);
    //deserialize the JSON response from the Bing Image Search API
    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result.jsonResult);
    ```

2. İlk döndürülen görüntüyü Al `jsonObj`ve yazdırma başlığı ve bir görüntünün URL'si. 
    ```csharp
    var firstJsonObj = jsonObj["value"][0];
    Console.WriteLine("Title for the first image result: " + firstJsonObj["name"]+"\n");
    //After running the application, copy the output URL into a browser to see the image. 
    Console.WriteLine("URL for the first image result: " + firstJsonObj["webSearchUrl"]+"\n");
    ```  
       
3. Abonelik anahtarınızı uygulamanın koddan kaldırdığınızdan emin olun.

## <a name="json-response"></a>JSON yanıtı

Bing resim arama API'si alınan yanıtları JSON olarak döndürülür. Bu örnek yanıt, tek bir sonuç göstermek için kısaltıldı.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing resim arama tek sayfalı uygulama Öğreticisi](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing resim arama nedir?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Çevrimiçi bir etkileşimli Tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Ücretsiz bir Bilişsel hizmetler erişim anahtarını alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Azure Bilişsel hizmetler belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing resim arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)