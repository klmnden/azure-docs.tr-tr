---
title: 'Hızlı Başlangıç: Metin betiğini dönüştürme - Translator Metin Çevirisi, Python'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Python ile Translator Metin Çevirisi API’sini kullanarak bir dildeki metni bir betikten diğerine dönüştüreceksiniz.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/21/2018
ms.author: nolachar
ms.openlocfilehash: 24fe0dc23703bdca4ce74ba1a527990a70ac2ffc
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46122120"
---
# <a name="quickstart-transliterate-text-with-python"></a>Hızlı Başlangıç: Python ile metni başka alfabeye çevirme

Bu hızlı başlangıçta, Translator Metin Çevirisi API'sini kullanarak bir dildeki metni bir betikten diğerine dönüştüreceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu çalıştırmak için [Python 3.x](https://www.python.org/downloads/) sürümüne ihtiyacınız olacak.

Translator Metin Çevirisi API'sini kullanmak için, ayrıca abonelik anahtarınızın olması gerekir; bkz. [Translator Metin Çevirisi API'sine kaydolma](translator-text-how-to-signup.md).

## <a name="transliterate-request"></a>Karakter Dönüştürme isteği

Aşağıdaki kod, [Karakter Dönüştürme](./reference/v3-0-transliterate.md) yöntemini kullanarak bir dildeki metni bir betikten diğerine dönüştürür.

1. Sık kullandığınız kod düzenleyicisinde yeni bir Python projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `subscriptionKey` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```python
# -*- coding: utf-8 -*-

import http.client, urllib.parse, uuid, json

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'ENTER KEY HERE'

host = 'api.cognitive.microsofttranslator.com'
path = '/transliterate?api-version=3.0'

# Transliterate text in Japanese from Japanese script (i.e. Hiragana/Katakana/Kanji) to Latin script.
params = '&language=ja&fromScript=jpan&toScript=latn';

# Transliterate "good afternoon".
text = 'こんにちは'

def transliterate (content):

    headers = {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-type': 'application/json',
        'X-ClientTraceId': str(uuid.uuid4())
    }

    conn = http.client.HTTPSConnection(host)
    conn.request ("POST", path + params, content, headers)
    response = conn.getresponse ()
    return response.read ()

requestBody = [{
    'Text' : text,
}]
content = json.dumps(requestBody, ensure_ascii=False).encode('utf-8')
result = transliterate (content)

# Note: We convert result, which is JSON, to and from an object so we can pretty-print it.
# We want to avoid escaping any Unicode characters that result contains. See:
# https://stackoverflow.com/questions/18337407/saving-utf-8-texts-in-json-dumps-as-utf8-not-as-u-escape-sequence
output = json.dumps(json.loads(result), indent=4, ensure_ascii=False)

print (output)
```

## <a name="transliterate-response"></a>Karakter Dönüştürme yanıtı

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
[
  {
    "text": "konnnichiha",
    "script": "latn"
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Çeviri ve dil tanımlayıcı gibi bu ve diğer hızlı başlangıçlardaki örnek kodlarla birlikte GitHub’daki diğer örnek Translator Metin Çevirisi projelerini keşfedin.

> [!div class="nextstepaction"]
> [GitHub’da Python örneklerini keşfedin](https://aka.ms/TranslatorGitHub?type=&language=python)
