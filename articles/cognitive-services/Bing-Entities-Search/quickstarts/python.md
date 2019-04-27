---
title: "Hızlı Başlangıç: Python kullanarak Bing varlık arama REST API'si için bir arama isteği gönder"
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Python kullanarak Bing varlık arama REST API'si için bir istek göndermek için kullanın ve bir JSON yanıtı alırsınız.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: b35fa32776fa449bf4f46479345a94e63fe28e68
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60737015"
---
# <a name="quickstart-send-a-search-request-to-the-bing-entity-search-rest-api-using-python"></a>Hızlı Başlangıç: Python kullanarak Bing varlık arama REST API'si için bir arama isteği gönder

Bu hızlı başlangıçta, Bing varlık arama API'si, ilk çağrı yapmak ve JSON yanıtı görüntülemek için kullanın. Bu basit bir Python uygulaması API için bir haber arama sorgu gönderir ve yanıtını görüntüler. Bu örneğin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingEntitySearchv7.py)’da mevcuttur.

Bu uygulama Python ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

* [Python](https://www.python.org/downloads/) 2.x veya 3.x

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE veya düzenleyici yeni bir Python dosyası oluşturun ve aşağıdaki içeri aktarmaları ekleyin. Abonelik anahtarınız, uç nokta, Pazar ve bir arama sorgusu için değişkenler oluşturun. Uç noktanız Azure panosunda bulabilirsiniz.

    ```python
    import http.client, urllib.parse
    import json
    
    subscriptionKey = 'ENTER YOUR KEY HERE'
    host = 'api.cognitive.microsoft.com'
    path = '/bing/v7.0/entities'
    mkt = 'en-US'
    query = 'italian restaurants near me'
    ```

2. Bir isteği oluşturma Pazar değişkeninize ekleyerek url `?mkt=` parametresi. URL kodlaması, sorgu ve ona ekleme `&q=` parametresi. 
    
    ```python
    params = '?mkt=' + mkt + '&q=' + urllib.parse.quote (query)
    ```

## <a name="send-a-request-and-get-a-response"></a>Bir istek gönderir ve bir yanıt alın

1. Çağrılan bir işlev oluşturma `get_suggestions()`. Ardından aşağıdaki adımları gerçekleştirin.
   1. Abonelik anahtarınızı içeren bir sözlük ekleyin `Ocp-Apim-Subscription-Key` bir anahtar olarak.
   2. Kullanım `http.client.HTTPSConnection()` HTTPS istemci nesnesi oluşturmak için. Gönderme bir `GET` kullanarak istek `request()` , yol ve parametreleri ve üst bilgi bilgileri.
   3. Yanıtla Store `getresponse()`ve dönüş `response.read()`.

      ```python
      def get_suggestions ():
       headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
       conn = http.client.HTTPSConnection (host)
       conn.request ("GET", path + params, None, headers)
       response = conn.getresponse ()
       return response.read()
      ```

2. Çağrı `get_suggestions()`ve json yanıtı yazdırın.

    ```python
    result = get_suggestions ()
    print (json.dumps(json.loads(result), indent=4))
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

* [Bing varlık arama API'si nedir](../search-the-web.md)
* [Bing varlık arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)
