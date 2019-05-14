---
title: "Hızlı Başlangıç: Bing varlık arama REST API'si kullanarak bir arama isteği gönderC#"
titlesuffix: Azure Cognitive Services
description: Bing varlık arama REST API'si kullanarak bir istek göndermek için bu hızlı başlangıçta kullanmak C#ve bir JSON yanıtı alırsınız.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 03/12/2019
ms.author: aahi
ms.openlocfilehash: 211f33d5b217714b26dc39ad63f9d1427950589a
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65595766"
---
# <a name="quickstart-send-a-search-request-to-the-bing-entity-search-rest-api-using-c"></a>Hızlı Başlangıç: Bing varlık arama REST API'si kullanarak bir arama isteği gönderC#

Bu hızlı başlangıçta, Bing varlık arama API'si, ilk çağrı yapmak ve JSON yanıtı görüntülemek için kullanın. Bu basit C# uygulama API için bir haber arama sorgu gönderir ve yanıtını görüntüler. Bu uygulama için kaynak kodu kullanılabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingEntitySearchv7.cs).

Bu uygulama C# ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir.


## <a name="prerequisites"></a>Önkoşullar

- Herhangi bir sürümünü [Visual Studio 2017 veya üstü](https://www.visualstudio.com/downloads/).

- NuGet paketi olarak kullanılabilen [Json.NET](https://www.newtonsoft.com/json) çerçevesi. Visual Studio'da NuGet paketini yüklemek için:

   1. Projenize sağ tıklayın **Çözüm Gezgini**.
   2. Seçin **NuGet paketlerini Yönet**.
   3. Arama *Newtonsoft.Json* paketini ve yükleme.

- Linux/MacOS kullanıyorsanız, bu uygulamayı kullanarak çalıştırılabilir [Mono](https://www.mono-project.com/).


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Yeni bir C# çözümünü Visual Studio'da konsol. Ardından ana kod dosyasına aşağıdaki ad alanlarını ekleyin.
    
    ```csharp
    using Newtonsoft.Json;
    using System;
    using System.Net.Http;
    using System.Text;
    ```

2. Yeni bir sınıf oluşturun ve API uç noktası, abonelik anahtarı ve arama yapmak istediğiniz sorgu değişkenleri ekleyin.

    ```csharp
    namespace EntitySearchSample
    {
        class Program
        {
            static string host = "https://api.cognitive.microsoft.com";
            static string path = "/bing/v7.0/entities";
    
            static string market = "en-US";
    
            // NOTE: Replace this example key with a valid subscription key.
            static string key = "ENTER YOUR KEY HERE";
    
            static string query = "italian restaurant near me";
        //...
        }
    }
    ```

## <a name="send-a-request-and-get-the-api-response"></a>Bir istek gönderir ve API yanıtı alın

1. Bir sınıf içinde çağrılan bir işlev oluşturun `Search()`. Yeni bir `HttpClient` nesne ve abonelik anahtarınızı ekleme `Ocp-Apim-Subscription-Key` başlığı.

   1. Konak ve yol birleştirerek isteğiniz URI'sini oluşturur. Ardından, Pazar ve URL kodlaması sorgunuzu ekleyin.
   2. Await `client.GetAsync()` bir HTTP yanıtı almanız ve ardından json yanıtı bekleyen tarafından depolamak `ReadAsStringAsync()`.
   3. Biçim ile JSON dizesi `JsonConvert.DeserializeObject()` ve konsola yazdırır.

      ```csharp
      async static void Search()
      {
       //...
       HttpClient client = new HttpClient();
       client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

       string uri = host + path + "?mkt=" + market + "&q=" + System.Net.WebUtility.UrlEncode(query);

       HttpResponseMessage response = await client.GetAsync(uri);

       string contentString = await response.Content.ReadAsStringAsync();
       dynamic parsedJson = JsonConvert.DeserializeObject(contentString);
       Console.WriteLine(parsedJson);
      }
      ```

2. Uygulamanızın ana yöntemi çağırın `Search()` işlevi.
    
    ```csharp
    static void Main(string[] args)
    {
        Search();
        Console.ReadLine();
    }
    ```


## <a name="example-json-response"></a>Örnek JSON yanıtı

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "https://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfa uygulaması oluşturma](../tutorial-bing-entities-search-single-page-app.md)

* [Bing varlık arama API'si nedir?](../overview.md )
* [Bing varlık arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)
