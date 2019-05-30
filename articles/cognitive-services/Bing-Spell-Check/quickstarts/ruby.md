---
title: "Hızlı Başlangıç: Ruby ve Bing yazım denetimi REST API'si ile yazım denetimi"
titlesuffix: Azure Cognitive Services
description: Bing yazım denetimi REST API'si yazım ve dilbilgisi denetimini kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: a3e65f9ad8e8a9c6876d1588ecaa94531206c6f4
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389700"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-ruby"></a>Hızlı Başlangıç: Ruby ve Bing yazım denetimi REST API'si ile yazım denetimi

Bu hızlı başlangıçta, Bing yazım denetimi REST API'si Ruby kullanarak ilk çağrı yapmak için kullanın. Bu basit uygulama API'sine bir istek gönderir ve bir kelimelerin yaramadı tanımlamanıza, önerilen düzeltmeler tarafından izlenen bir liste döndürür. Bu uygulama, Ruby ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir. Bu uygulama için kaynak kodu kullanılabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/ruby/Search/BingSpellCheckv7.rb)

## <a name="prerequisites"></a>Önkoşullar

* [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) veya üzeri.

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Tercih ettiğiniz düzenleyiciyi veya IDE içinde yeni bir Ruby dosyası oluşturun ve aşağıdaki koşulları ekleyin. 

    ```javascript
    require 'net/http'
    require 'uri'
    require 'json'
    ```

2. Abonelik anahtarı, uç nokta URI'si ve yol değişkenleri oluşturun. İstek parametrelerinizi ekleyerek oluşturma `mkt=` parametresi, pazarlama ve `&mode` için `proof` sağlama modu.

    ```ruby
    key = 'ENTER YOUR KEY HERE'
    uri = 'https://api.cognitive.microsoft.com'
    path = '/bing/v7.0/spellcheck?'
    params = 'mkt=en-us&mode=proof'
    ```

## <a name="send-a-spell-check-request"></a>Yazım denetimi İsteği Gönder

1. Bir URI, konak URI yolu ve parametreleri dizeden oluşturun. Sorgu metni içeren için yazım denetimi yapmak istediğiniz kullanıcının kümesi.

   ```ruby
   uri = URI(uri + path + params)
   uri.query = URI.encode_www_form({
      # Request parameters
   'text' => 'Hollo, wrld!'
   })
   ```

2. Yukarıda oluşturulan URI'yi kullanarak bir istek oluşturun. Anahtarınıza ekleme `Ocp-Apim-Subscription-Key` başlığı.

    ```ruby
    request = Net::HTTP::Post.new(uri)
    request['Content-Type'] = "application/x-www-form-urlencoded"
    request['Ocp-Apim-Subscription-Key'] = key
    ```

3. İsteği gönderin.

    ```ruby
    response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
        http.request(request)
    end
    ```

4. JSON yanıtını alma ve konsola yazdırır. 

    ```ruby
    result = JSON.pretty_generate(JSON.parse(response.body))
    puts result
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
> [Bir tek sayfalı web uygulaması oluşturma](../tutorials/spellcheck.md)

- [Bing yazım denetimi API'si nedir?](../overview.md)
- [Bing Yazım Denetimi API’si v7 Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)
