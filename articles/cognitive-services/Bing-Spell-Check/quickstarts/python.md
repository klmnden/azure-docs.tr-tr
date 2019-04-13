---
title: "Hızlı Başlangıç: Bing yazım denetimi REST API'si ve Python ile yazım denetimi"
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
ms.openlocfilehash: 1cf46fd5ec55f0b240f6bb4adbe49c1344a4663b
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59547688"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-python"></a>Hızlı Başlangıç: Bing yazım denetimi REST API'si ve Python ile yazım denetimi

Bu hızlı başlangıçta, Bing yazım denetimi REST API'si, ilk çağrı yapmak için kullanın. Bu basit bir Python uygulaması API'sine bir istek gönderir ve önerilen düzeltmeler listesini döndürür. Bu uygulama Python ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir. Bu uygulama için kaynak kodu kullanılabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingEntitySearchv7.py)

## <a name="prerequisites"></a>Önkoşullar

* Python [3.x](https://www.python.org)

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="initialize-the-application"></a>Uygulamayı Başlat

1. Sık kullandığınız IDE veya düzenleyici yeni bir Python dosyası oluşturun ve aşağıdaki içeri aktarma deyimini ekleyin.

   ```python
   import requests
   import json
   ```

2. Onay, abonelik anahtarınız ve Bing yazım denetimi uç noktanızı yazım istediğiniz metni için değişkenler oluşturun.

    ```python
    api_key = "<ENTER-KEY-HERE>"
    example_text = "Hollo, wrld" # the text to be spell-checked
    endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck"
    ```

## <a name="create-the-parameters-for-the-request"></a>İstek için parametreleri oluşturma

1. Yeni bir sözlük ile oluşturma `text` anahtar ve değer olarak, metin olarak.

    ```python
    data = {'text': example_text}
    ```

2. İsteğiniz parametrelerini ekleyin. Pazar kodunuzu sonra ekleme `mkt=`. Pazar istekten yaptığınız ülke kodudur. Ayrıca, ekleme, yazım denetimi modu sonra `&mode=`. Modu, ya da `proof` (çoğu yazım/gramer hataları yakalar) veya `spell` (çoğu yazım ancak kadar fazla dil bilgisi hataları yakalar).

    ```python
    params = {
        'mkt':'en-us',
        'mode':'proof'
        }
    ```

3. Ekleme bir `Content-Type` başlığı ve abonelik anahtarınızı `Ocp-Apim-Subscription-Key` başlığı.

    ```python
    headers = {
        'Content-Type': 'application/x-www-form-urlencoded',
        'Ocp-Apim-Subscription-Key': api_key,
        }
    ```

## <a name="send-the-request-and-read-the-response"></a>İstek göndermek ve yanıtın okuma

1. İstekleri kitaplığını kullanarak bir POST isteği gönderin.

    ```python
    response = requests.post(endpoint, headers=headers, params=params, data=data)
    ```

2. JSON yanıtını alma ve yazdırabilirsiniz.

    ```python
    json_response = response.json()
    print(json.dumps(json_response, indent=4))
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
- [Bing Yazım Denetimi API’si v7 Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
