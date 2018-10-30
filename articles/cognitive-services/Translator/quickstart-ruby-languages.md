---
title: 'Hızlı Başlangıç: Desteklenen dilleri alma, Ruby - Translator Metin Çevirisi API’si'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Ruby ile Translator Metin Çevirisi API’sini kullanarak çeviri, başka alfabeye çevirme ve sözlük arama için desteklenen dillerin ve örneklerin bir listesini alacaksınız.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: erhopf
ms.openlocfilehash: 02ebeae83464aa9770854fd06468b3731d6b8549
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645278"
---
# <a name="quickstart-get-supported-languages-with-the-translator-text-rest-api-ruby"></a>Hızlı Başlangıç: Translator Metin Çevirisi REST API'si (Ruby) ile desteklenen dilleri alma

Bu hızlı başlangıçta, Translator Metin Çevirisi API’sini kullanarak çeviri, başka alfabeye çevirme ve sözlük arama için desteklenen dillerin ve örneklerin bir listesini alacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu çalıştırmak için [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) veya üzeri bir sürümüne ihtiyacınız olacak.

Translator Metin Çevirisi API'sini kullanmak için, ayrıca abonelik anahtarınızın olması gerekir; bkz. [Translator Metin Çevirisi API'sine kaydolma](translator-text-how-to-signup.md).

## <a name="languages-request"></a>Diller isteği

Aşağıdaki kod, [Diller](./reference/v3-0-languages.md) yöntemini kullanarak çeviri, başka alfabeye çevirme ve sözlük arama için desteklenen dillerin ve örneklerin listesini alır.

1. Sık kullandığınız kod düzenleyicisinde yeni bir Ruby projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```ruby
require 'net/https'
require 'uri'
require 'cgi'
require 'json'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the key string value with your valid subscription key.
key = 'ENTER KEY HERE'

host = 'https://api.cognitive.microsofttranslator.com'
path = '/languages?api-version=3.0'

uri = URI (host + path)

request = Net::HTTP::Get.new(uri)
request['Ocp-Apim-Subscription-Key'] = key

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

result = response.body.force_encoding("utf-8")

json = JSON.pretty_generate(JSON.parse(result))

output_path = 'output.txt'

File.open(output_path, 'w' ) do |output|
    output.print json
end
```

## <a name="languages-response"></a>Diller yanıtı

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    },
...
  },
  "transliteration": {
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "scripts": [
        {
          "code": "Arab",
          "name": "Arabic",
          "nativeName": "العربية",
          "dir": "rtl",
          "toScripts": [
            {
              "code": "Latn",
              "name": "Latin",
              "nativeName": "اللاتينية",
              "dir": "ltr"
            }
          ]
        },
        {
          "code": "Latn",
          "name": "Latin",
          "nativeName": "اللاتينية",
          "dir": "ltr",
          "toScripts": [
            {
              "code": "Arab",
              "name": "Arabic",
              "nativeName": "العربية",
              "dir": "rtl"
            }
          ]
        }
      ]
    },
...
  },
  "dictionary": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
...
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Çeviri ve başka alfabeye çevirme gibi bu ve diğer hızlı başlangıçlardaki örnek kodlarla birlikte GitHub’daki diğer örnek Translator Metin Çevirisi projelerini keşfedin.

> [!div class="nextstepaction"]
> [GitHub’da Ruby örneklerini keşfedin](https://aka.ms/TranslatorGitHub?type=&language=ruby)
