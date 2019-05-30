---
title: "Hızlı Başlangıç: Bing yazım denetimi REST API'si ile yazım denetimi veC#"
titlesuffix: Azure Cognitive Services
description: Bing yazım denetimi REST API'si yazım ve dilbilgisi denetimini kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 04/11/2019
ms.author: aahi
ms.openlocfilehash: e7a1f2572296015aac2d05b36b9b659c85586ff9
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66390256"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-c"></a>Hızlı Başlangıç: Bing yazım denetimi REST API'si ile yazım denetimi veC#

Bu hızlı başlangıçta, Bing yazım denetimi REST API'si, ilk çağrı yapmak için kullanın. Bu basit C# uygulama API'sine bir istek gönderir ve önerilen düzeltmeler listesini döndürür. Bu uygulama C# ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir. Bu uygulama için kaynak kodu kullanılabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingAutosuggestv7.cs).

## <a name="prerequisites"></a>Önkoşullar

* Herhangi bir sürümünü [Visual Studio 2017 veya üstü](https://www.visualstudio.com/downloads/).
* Yüklenecek `Newtonsoft.Json` Visual Studio'da bir NuGet paketi olarak:
    1. İçinde **Çözüm Gezgini**, çözüme sağ tıklayın.
    1. Seçin **çözüm için NuGet paketlerini Yönet**.
    1. Arama `Newtonsoft.Json` paketini ve yükleme.
* Linux/MacOS kullanıyorsanız, bu uygulamanın kullanılarak çalıştırılabilir [Mono](https://www.mono-project.com/).

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. adlı yeni bir konsol çözümü oluşturma `SpellCheckSample` Visual Studio'da. Ardından ana kod dosyasına aşağıdaki ad alanlarını ekleyin.
    
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Newtonsoft.Json;
    ```

2. API uç noktası, abonelik anahtarınız ve yazım iade edilecek metin için değişkenler oluşturun.

    ```csharp
    namespace SpellCheckSample
    {
        class Program
        {
            static string host = "https://api.cognitive.microsoft.com";
            static string path = "/bing/v7.0/spellcheck?";
            static string key = "<ENTER-KEY-HERE>";
            //text to be spell-checked
            static string text = "Hollo, wrld!";
        }
    }
    ```

3. Arama parametrelerinizi için bir değişken oluşturun. Pazar kodunuzu sonra ekleme `mkt=`. Pazar istekten yaptığınız ülke kodudur. Ayrıca, ekleme, yazım denetimi modu sonra `&mode=`. Modu, ya da `proof` (çoğu yazım/gramer hataları yakalar) veya `spell` (çoğu yazım ancak kadar fazla dil bilgisi hataları yakalar).
    
    ```csharp
    static string params_ = "mkt=en-US&mode=proof";
    ```

## <a name="create-and-send-a-spell-check-request"></a>Oluşturma ve yazım onay isteği gönderme

1. Çağrılan zaman uyumsuz bir işlev oluşturma `SpellCheck()` API'sine bir istek gönderebilirsiniz. Oluşturma bir `HttpClient`, abonelik anahtarınızı ekleyin `Ocp-Apim-Subscription-Key` başlığı. Ardından işlev içinde aşağıdaki adımları gerçekleştirin.

    ```csharp
    async static void SpellCheck()
    {
        HttpClient client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

        HttpResponseMessage response = new HttpResponseMessage();
        // add the rest of the code snippets here (except for main())...
    }
    ```

2. İsteğiniz için URI, konak, yol ve parametreleri ekleyerek oluşturun.
    
    ```csharp
    string uri = host + path + params_;
    ```

3. İle bir liste oluşturur bir `KeyValuePair` metni içeren nesne ve oluşturmak için kullanmak bir `FormUrlEncodedContent` nesne. Üst bilgi bilgileri ayarlama ve kullanma `PostAsync()` isteği göndermek için.

    ```csharp
    List<KeyValuePair<string, string>> values = new List<KeyValuePair<string, string>>();
    values.Add(new KeyValuePair<string, string>("text", text));
    
    using (FormUrlEncodedContent content = new FormUrlEncodedContent(values))
    {
        content.Headers.ContentType = new MediaTypeHeaderValue("application/x-www-form-urlencoded");
        response = await client.PostAsync(uri, content);
    }
    ```

## <a name="get-and-print-the-api-response"></a>Alma ve API yanıtı yazdırma

### <a name="get-the-client-id-header"></a>İstemci kimliği üst bilgisi alma

Yanıt içeriyorsa bir `X-MSEdge-ClientID` üstbilgisi değerini alır ve yazdırabilirsiniz.

``` csharp
string client_id;
if (response.Headers.TryGetValues("X-MSEdge-ClientID", out IEnumerable<string> header_values))
{
    client_id = header_values.First();
    Console.WriteLine("Client ID: " + client_id);
}
```

### <a name="get-the-response"></a>Yanıt alın

API yanıt alın. JSON nesnesi seri durumdan ve konsola yazdırır.

```csharp
string contentString = await response.Content.ReadAsStringAsync();

dynamic jsonObj = JsonConvert.DeserializeObject(contentString);
Console.WriteLine(jsonObj);
```

## <a name="call-the-spell-check-function"></a>Yazım denetimi işlevi çağırma

Projenizin ana işlev çağrısında `SpellCheck()`.

```csharp
static void Main(string[] args)
{
    SpellCheck();
    Console.ReadLine();
}
```

## <a name="example-json-response"></a>Örnek JSON yanıtı

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../tutorials/spellcheck.md)

- [Bing yazım denetimi API'si nedir?](../overview.md)
- [Bing Yazım Denetimi API’si v7 Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)
