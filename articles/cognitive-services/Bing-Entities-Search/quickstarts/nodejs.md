---
title: "Hızlı Başlangıç: Node.js kullanarak Bing varlık arama REST API'si için bir arama isteği gönder"
titlesuffix: Azure Cognitive Services
description: Bing varlık arama REST API'si kullanarak bir istek göndermek için bu hızlı başlangıçta kullanmak C#ve bir JSON yanıtı alırsınız.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 8007d576a6b896f12423087cfd4a483d9171abc5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60737134"
---
# <a name="quickstart-send-a-search-request-to-the-bing-entity-search-rest-api-using-nodejs"></a>Hızlı Başlangıç: Node.js kullanarak Bing varlık arama REST API'si için bir arama isteği gönder

Bu hızlı başlangıçta, Bing varlık arama API'si, ilk çağrı yapmak ve JSON yanıtı görüntülemek için kullanın. Bu basit bir JavaScript uygulama API için bir haber arama sorgu gönderir ve yanıtını görüntüler. Bu örneğin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingEntitySearchv7.js)’da mevcuttur.

Bu uygulamanın, JavaScript'te yazılmış olsa da çoğu programlama dilleri ile uyumlu bir RESTful Web hizmeti API'dir.

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/download/)’in en son sürümü.

* [JavaScript isteği kitaplığı](https://github.com/request/request)

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE’de veya düzenleyicide yeni bir JavaScript dosyası oluşturun ve katılık ve https gereksinimlerini ayarlayın.

    ```javaScript
    'use strict';
    let https = require ('https');
    ```

2. API uç noktası, abonelik anahtarı ve arama sorgusu için değişkenler oluşturun.

    ```javascript
    let subscriptionKey = 'ENTER YOUR KEY HERE';
    let host = 'api.cognitive.microsoft.com';
    let path = '/bing/v7.0/entities';
    
    let mkt = 'en-US';
    let q = 'italian restaurant near me';
    ```

3. Adlı bir dizeye pazara çıkma sürelerini ve sorgu parametrelerinizin ekleme `query`. URL unutmayın-sorgunuzu ile kodlama `encodeURI()`.
    ```javascript 
    let query = '?mkt=' + mkt + '&q=' + encodeURI(q);
    ```

## <a name="handle-and-parse-the-response"></a>Yanıtı işleme ve ayrıştırma

1. adlı bir fonksiyon tanımlayın `response_handler` HTTP çağrısı, alan `response`, parametre olarak. Bu işlevin içinde aşağıdaki adımları gerçekleştirin:

    1. JSON yanıtının gövdesini içerecek bir değişken tanımlayın.  
        ```javascript
        let response_handler = function (response) {
            let body = '';
        };
        ```

    2. **Veri** işareti çağrıldığında yanıtın gövdesini depolama
        ```javascript
        response.on('data', function (d) {
            body += d;
        });
        ```

    3. Olduğunda bir **son** bayrağı sinyal, JSON ayrıştırmak ve yazdırabilirsiniz.

        ```javascript
        response.on ('end', function () {
        let json = JSON.stringify(JSON.parse(body), null, '  ');
        console.log (json);
        });
        ```

## <a name="send-a-request"></a>İstek gönderme

1. Çağrılan bir işlev oluşturma `Search` arama isteği göndermek için. İçinde aşağıdaki adımları gerçekleştirin.

   1. İstek parametrelerinizi içeren bir JSON nesnesi oluşturun: kullanın `Get` yöntemi ve konak ve yol bilgilerinizi ekleyin. Abonelik anahtarınızı ekleme `Ocp-Apim-Subscription-Key` başlığı. 
   2. Kullanım `https.request()` daha önce oluşturduğunuz yanıt işleyicisi ve arama parametrelerinizi isteği göndermek için.
    
      ```javascript
      let Search = function () {
       let request_params = {
           method : 'GET',
           hostname : host,
           path : path + query,
           headers : {
               'Ocp-Apim-Subscription-Key' : subscriptionKey,
           }
       };
    
       let req = https.request (request_params, response_handler);
       req.end ();
      }
      ```

2. Çağrı `Search()` işlevi.

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
