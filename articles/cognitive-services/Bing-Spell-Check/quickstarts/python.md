---
title: "Hızlı başlangıç: Bing Yazım Denetimi API'si, Python"
titlesuffix: Azure Cognitive Services
description: Bing Yazım Denetimi API'sini kısa sürede kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri alın.
services: cognitive-services
author: v-jaswel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: quickstart
ms.date: 09/14/2017
ms.author: v-jaswel
ms.openlocfilehash: 8614e4388f45c3bc9f71697bad44589e2165f6c8
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803299"
---
# <a name="quickstart-for-bing-spell-check-api-with-python"></a>Hızlı başlangıç: Python ile Bing Yazım Denetimi API'si 

Bu makalede Python ile [Bing Yazım Denetimi API'si](https://azure.microsoft.com/services/cognitive-services/spell-check/) kullanma adımları gösterilmektedir. Yazım Denetimi API'si tanınmayan sözcüklere ek olarak değişiklik önerileri döndürür. Genellikle bu API'ye metin gönderip önerilen değişiklikleri metne uygular veya uygulamanızın kullanıcısına göstererek değişikliklerin yapılıp yapılmayacağına karar vermelerini sağlayabilirsiniz. Bu makalede "Hollo, wrld!" metnini içeren bir istek gönderme adımları gösterilmiştir. Önerilen değişiklikler "Hello" ve "world" şeklindedir.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu çalıştırmak için [Python 3.x](https://www.python.org/downloads/) sürümü gereklidir.

**Bing Yazım Denetimi API'si v7** sürümüne sahip bir [Bilişsel Hizmetler API hesabınız](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/#lang) bu hızlı başlangıç için yeterlidir. Ücretsiz denemenizi etkinleştirdiğinizde verilen erişim anahtarınız olması veya Azure panonuzdan ücretli bir abonelik anahtarı kullanmanız gerekir.

## <a name="get-spell-check-results"></a>Yazım Denetimi sonuçlarını alma

1. Sık kullandığınız IDE'de yeni bir Python projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `subscriptionKey` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```python
import http.client, urllib.parse, json

text = 'Hollo, wrld!'

data = {'text': text}

# NOTE: Replace this example key with a valid subscription key.
key = 'ENTER KEY HERE'

host = 'api.cognitive.microsoft.com'
path = '/bing/v7.0/spellcheck?'
params = 'mkt=en-us&mode=proof'

headers = {'Ocp-Apim-Subscription-Key': key,
'Content-Type': 'application/x-www-form-urlencoded'}

# The headers in the following example 
# are optional but should be considered as required:
#
# X-MSEdge-ClientIP: 999.999.999.999  
# X-Search-Location: lat: +90.0000000000000;long: 00.0000000000000;re:100.000000000000
# X-MSEdge-ClientID: <Client ID from Previous Response Goes Here>

conn = http.client.HTTPSConnection(host)
body = urllib.parse.urlencode (data)
conn.request ("POST", path + params, body, headers)
response = conn.getresponse ()
output = json.dumps(json.loads(response.read()), indent=4)
print (output)
```

**Yanıt**

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
> [Bing Yazım Denetimi öğreticisi](../tutorials/spellcheck.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Bing Yazım Denetimi'ne genel bakış](../proof-text.md)
- [Bing Yazım Denetimi API’si v7 Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
