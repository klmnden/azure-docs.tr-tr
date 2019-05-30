---
title: "Hızlı Başlangıç: Ruby kullanarak Bing varlık arama REST API'si için bir arama isteği gönder"
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Ruby kullanarak Bing varlık arama REST API'si için bir istek göndermek için kullanın ve bir JSON yanıtı alırsınız.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: b5653ffbfeb22bc59c48dd92b558178fcd89b2de
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66384510"
---
# <a name="quickstart-for-bing-entity-search-api-with-ruby"></a>Hızlı başlangıç: Ruby ile Bing Varlık Arama API'si

Bu hızlı başlangıçta, Bing varlık arama API'si, ilk çağrı yapmak ve JSON yanıtı görüntülemek için kullanın. Bu basit bir Ruby uygulaması API için bir haber arama sorgu gönderir ve yanıtını görüntüler. Bu uygulama için kaynak kodu kullanılabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/ruby/Search/BingEntitySearchv7.rb).

Bu uygulama, Ruby ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

* [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) veya üzeri.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE veya Kod Düzenleyicisi, bir haber Ruby dosya oluşturun ve aşağıdaki paketleri içeri aktarmak.

    ```ruby
    require 'net/https'
    require 'cgi'
    require 'json'
    ```

2. API uç noktanıza, haber arama URL'si, abonelik anahtarınız ve bir arama sorgusu için değişkenler oluşturun.
    
    ```ruby
    host = 'https://api.cognitive.microsoft.com'
    path = '/bing/v7.0/entities'
    
    mkt = 'en-US'
    query = 'italian restaurants near me'
    ```

## <a name="format-and-make-an-api-request"></a>API isteğini biçimlendirme ve API isteğinde bulunma

1. Pazar değişkeninize ekleyerek isteğiniz için parametreleri dize oluşturma `?mkt=` parametresi. Sorgunuzu kodlama ve eklenecek `&q=` parametresi. API ana, yol ve isteğiniz parametrelerini birleştirerek ve bunları bir URI nesnesi cast.

    ```ruby
    params = '?mkt=' + mkt + '&q=' + CGI.escape(query)
    uri = URI (host + path + params)
    ```

2. Son adım değişkenlerinden isteği oluşturmak için kullanın. Abonelik anahtarınızı ekleme `Ocp-Apim-Subscription-Key` başlığı.

    ```ruby
    request = Net::HTTP::Get.new(uri)
    request['Ocp-Apim-Subscription-Key'] = subscriptionKey
    ```

3. İstek göndermek ve yanıtın yazdırma

    ```ruby
    response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
        http.request (request)
    end

    puts JSON::pretty_generate (JSON (response.body))
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
* [Bing varlık arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference)
