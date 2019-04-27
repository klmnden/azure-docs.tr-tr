---
title: "Hızlı Başlangıç: İki dilli sözlük ile sözcük araması Ruby - Translator metin çevirisi API'si"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Ruby ile Translator Metin Çevirisi API’sini kullanarak alternatif çeviriler ve bağlam içindeki terim örnekleri bulacaksınız.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
origin.date: 02/08/2019
ms.date: 03/12/2019
ms.author: v-junlch
ms.openlocfilehash: 98d9b7c3bf85e025c9479609737cb8c66bc66287
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60511424"
---
# <a name="quickstart-look-up-words-with-bilingual-dictionary-using-ruby"></a>Hızlı Başlangıç: Ruby kullanarak iki dilli sözlük ile sözcük arayın

Bu hızlı başlangıçta, Translator Metin Çevirisi API'sini kullanarak bir terimin olası alternatif çevirilerini ve ayrıca bu alternatif çevirilerin kullanım örneklerini bulacaksınız.

## <a name="prerequisites"></a>Önkoşullar

Bu kodu çalıştırmak için [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) veya üzeri bir sürümüne ihtiyacınız olacak.

Translator Metin Çevirisi API'sini kullanmak için, ayrıca abonelik anahtarınızın olması gerekir; bkz. [Translator Metin Çevirisi API'sine kaydolma](translator-text-how-to-signup.md).

## <a name="dictionary-lookup-request"></a>Sözlük Arama isteği

Aşağıdaki kod, [Sözlük Arama](./reference/v3-0-dictionary-lookup.md) yöntemini kullanarak bir sözcüğün alternatif çevirilerini alır.

1. Sık kullandığınız kod düzenleyicisinde yeni bir Ruby projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```ruby
require 'net/https'
require 'uri'
require 'cgi'
require 'json'
require 'securerandom'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the key string value with your valid subscription key.
key = 'ENTER KEY HERE'
region = 'your region'
host = 'https://api.translator.azure.cn'
path = '/dictionary/lookup?api-version=3.0'

params = '&from=en&to=fr'

uri = URI (host + path + params)

text = 'great'

content = '[{"Text" : "' + text + '"}]'

request = Net::HTTP::Post.new(uri)
request['Content-type'] = 'application/json'
request['Content-length'] = content.length
request['Ocp-Apim-Subscription-Key'] = key
request['Ocp-Apim-Subscription-Region'] = region
request['X-ClientTraceId'] = SecureRandom.uuid
request.body = content

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

result = response.body.force_encoding("utf-8")

json = JSON.pretty_generate(JSON.parse(result))
puts json
```

## <a name="dictionary-lookup-response"></a>Sözlük Arama yanıtı

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
[
  {
    "normalizedSource": "great",
    "displaySource": "great",
    "translations": [
      {
        "normalizedTarget": "grand",
        "displayTarget": "grand",
        "posTag": "ADJ",
        "confidence": 0.2783,
        "prefixWord": "",
        "backTranslations": [
          {
            "normalizedText": "great",
            "displayText": "great",
            "numExamples": 15,
            "frequencyCount": 34358
          },
          {
            "normalizedText": "big",
            "displayText": "big",
            "numExamples": 15,
            "frequencyCount": 21770
          },
...
        ]
      },
      {
        "normalizedTarget": "super",
        "displayTarget": "super",
        "posTag": "ADJ",
        "confidence": 0.1514,
        "prefixWord": "",
        "backTranslations": [
          {
            "normalizedText": "super",
            "displayText": "super",
            "numExamples": 15,
            "frequencyCount": 12023
          },
          {
            "normalizedText": "great",
            "displayText": "great",
            "numExamples": 15,
            "frequencyCount": 10931
          },
...
        ]
      },
...
    ]
  }
]
```

## <a name="dictionary-examples-request"></a>Sözlük Örnekleri isteği

Aşağıdaki kod, [Sözlük Örnekleri](./reference/v3-0-dictionary-examples.md) yöntemini kullanarak sözlükte terim kullanmayla ilgili bağlamsal örnekler alır.

1. Sık kullandığınız kod düzenleyicisinde yeni bir Ruby projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```ruby
require 'net/https'
require 'uri'
require 'cgi'
require 'json'
require 'securerandom'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the key string value with your valid subscription key.
key = 'ENTER KEY HERE'
region = 'your region'
host = 'https://api.translator.azure.cn'
path = '/dictionary/examples?api-version=3.0'

params = '&from=en&to=fr'

uri = URI (host + path + params)

text = 'great'
translation = 'formidable'

content = '[{"Text" : "' + text + '", "Translation" : "' + translation + '"}]'

request = Net::HTTP::Post.new(uri)
request['Content-type'] = 'application/json'
request['Content-length'] = content.length
request['Ocp-Apim-Subscription-Key'] = key
request['Ocp-Apim-Subscription-Region'] = region
request['X-ClientTraceId'] = SecureRandom.uuid
request.body = content

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

result = response.body.force_encoding("utf-8")

json = JSON.pretty_generate(JSON.parse(result))
puts json
```

## <a name="dictionary-examples-response"></a>Sözlük Örnekleri yanıtı

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
[
  {
    "normalizedSource": "great",
    "normalizedTarget": "formidable",
    "examples": [
      {
        "sourcePrefix": "You have a ",
        "sourceTerm": "great",
        "sourceSuffix": " expression there.",
        "targetPrefix": "Vous avez une expression ",
        "targetTerm": "formidable",
        "targetSuffix": "."
      },
      {
        "sourcePrefix": "You played a ",
        "sourceTerm": "great",
        "sourceSuffix": " game today.",
        "targetPrefix": "Vous avez été ",
        "targetTerm": "formidable",
        "targetSuffix": "."
      },
...
    ]
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Çeviri ve başka alfabeye çevirme gibi bu ve diğer hızlı başlangıçlardaki örnek kodlarla birlikte GitHub’daki diğer örnek Translator Metin Çevirisi projelerini keşfedin.

> [!div class="nextstepaction"]
> [GitHub’da Ruby örneklerini keşfedin](https://aka.ms/TranslatorGitHub?type=&language=ruby)

