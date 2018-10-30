---
title: 'Hızlı Başlangıç: Cümle uzunluklarını alma, Ruby - Translator Metin Çevirisi API’si'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Ruby ile Translator Metin Çevirisi API’sini kullanarak metindeki cümlelerin uzunluklarını bulacaksınız.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: erhopf
ms.openlocfilehash: 18ef4e20c28b783738d2be929e063e6dada412d4
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49644717"
---
# <a name="quickstart-get-sentence-lengths-with-the-translator-text-rest-api-ruby"></a>Hızlı Başlangıç: Translator Metin Çevirisi REST API'si (Ruby) ile cümle uzunluklarını alma

Bu hızlı başlangıçta, Translator Metin Çevirisi API'sini kullanarak metindeki cümlelerin uzunluklarını bulacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu çalıştırmak için [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) veya üzeri bir sürümüne ihtiyacınız olacak.

Translator Metin Çevirisi API'sini kullanmak için, ayrıca abonelik anahtarınızın olması gerekir; bkz. [Translator Metin Çevirisi API'sine kaydolma](translator-text-how-to-signup.md).

## <a name="breaksentence-request"></a>BreakSentence isteği

Aşağıdaki kod, [BreakSentence](./reference/v3-0-break-sentence.md) yöntemini kullanarak kaynak metni cümlelere böler.

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
path = '/breaksentence?api-version=3.0'

uri = URI (host + path)

text = 'How are you? I am fine. What did you do today?'

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

## <a name="breaksentence-response"></a>BreakSentence yanıtı

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "sentLen": [
      13,
      11,
      22
    ]
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Çeviri ve başka alfabeye çevirme gibi bu ve diğer hızlı başlangıçlardaki örnek kodlarla birlikte GitHub’daki diğer örnek Translator Metin Çevirisi projelerini keşfedin.

> [!div class="nextstepaction"]
> [GitHub’da Ruby örneklerini keşfedin](https://aka.ms/TranslatorGitHub?type=&language=ruby)
