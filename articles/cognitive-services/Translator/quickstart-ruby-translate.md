---
title: "Hızlı Başlangıç: Metin çevirme, Ruby - Translator Metin Çevirisi API'si"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Ruby ile Translator Metin Çevirisi API’sini kullanarak metni bir dilden diğerine çevireceksiniz.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: erhopf
ms.openlocfilehash: a36f873c08fee57ddb73f94b4a77dc4514422cb1
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645618"
---
# <a name="quickstart-translate-text-with-the-translator-text-rest-api-ruby"></a>Hızlı Başlangıç: Translator Metin Çevirisi REST API’si (Ruby) ile metin çevirme

Bu hızlı başlangıçta, Translator Metin Çevirisi API'sini kullanarak metni bir dilden diğerine çevireceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu çalıştırmak için [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) veya üzeri bir sürümüne ihtiyacınız olacak.

Translator Metin Çevirisi API'sini kullanmak için, ayrıca abonelik anahtarınızın olması gerekir; bkz. [Translator Metin Çevirisi API'sine kaydolma](translator-text-how-to-signup.md).

## <a name="translate-request"></a>Çeviri isteği

Aşağıdaki kod, [Çeviri](./reference/v3-0-translate.md) yöntemini kullanarak kaynak metni bir dilden diğerine çevirir.

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

host = 'https://api.cognitive.microsofttranslator.com'
path = '/translate?api-version=3.0'

# Translate to German and Italian.
params = '&to=de&to=it'

uri = URI (host + path + params)

text = 'Hello, world!'

content = '[{"Text" : "' + text + '"}]'

request = Net::HTTP::Post.new(uri)
request['Content-type'] = 'application/json'
request['Content-length'] = content.length
request['Ocp-Apim-Subscription-Key'] = key
request['X-ClientTraceId'] = SecureRandom.uuid
request.body = content

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

result = response.body.force_encoding("utf-8")

json = JSON.pretty_generate(JSON.parse(result))
puts json
```

## <a name="translate-response"></a>Çeviri yanıtı

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "translations": [
      {
        "text": "Hallo Welt!",
        "to": "de"
      },
      {
        "text": "Salve, mondo!",
        "to": "it"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Başka alfabeye çevirme ve dil tanımlayıcı gibi bu ve diğer hızlı başlangıçlardaki örnek kodlarla birlikte GitHub’daki diğer örnek Translator Metin Çevirisi projelerini keşfedin.

> [!div class="nextstepaction"]
> [GitHub’da Ruby örneklerini keşfedin](https://aka.ms/TranslatorGitHub?type=&language=ruby)
