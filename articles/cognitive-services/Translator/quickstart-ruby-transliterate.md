---
title: Translator Metin Çevirisi Ruby ile metin betiğini dönüştürme | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Bu hızlı başlangıçta, Bilişsel Hizmetler’de Ruby ile Translator Metin Çevirisi API'sini kullanarak bir dildeki metni bir betikten diğerine dönüştüreceksiniz.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: nolachar
ms.openlocfilehash: 56872ed6b6d7168fb1bf2c45de8104a592af34e3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "43772471"
---
# <a name="quickstart-transliterate-text-with-ruby"></a>Hızlı Başlangıç: Ruby ile metni başka alfabeye çevirme

Bu hızlı başlangıçta, Translator Metin Çevirisi API'sini kullanarak bir dildeki metni bir betikten diğerine dönüştüreceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu çalıştırmak için [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) veya üzeri bir sürümüne ihtiyacınız olacak.

Translator Metin Çevirisi API'sini kullanmak için, ayrıca abonelik anahtarınızın olması gerekir; bkz. [Translator Metin Çevirisi API'sine kaydolma](translator-text-how-to-signup.md).

## <a name="transliterate-request"></a>Karakter Dönüştürme isteği

Aşağıdaki kod, [Karakter Dönüştürme](./reference/v3-0-transliterate.md) yöntemini kullanarak bir dildeki metni bir betikten diğerine dönüştürür.

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
path = '/transliterate?api-version=3.0'

# Transliterate text in Japanese from Japanese script (i.e. Hiragana/Katakana/Kanji) to Latin script.
params = "&language=ja&fromScript=jpan&toScript=latn";

uri = URI (host + path + params)

# Transliterate "good afternoon".
text = 'こんにちは'

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
> [GitHub’da Ruby örneklerini keşfedin](https://aka.ms/TranslatorGitHub?type=&language=ruby)
