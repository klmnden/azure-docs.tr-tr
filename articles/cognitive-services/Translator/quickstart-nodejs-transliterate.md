---
title: 'Hızlı Başlangıç: Metin betiğini dönüştürme - Translator Metin Çevirisi, Node.js'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Node.js ile Translator Metin Çevirisi API’sini kullanarak bir dildeki metni bir betikten diğerine dönüştüreceksiniz.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/21/2018
ms.author: nolachar
ms.openlocfilehash: 53ebcd6c37141a783303dd71666d5c8d4805ca49
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46127711"
---
# <a name="quickstart-transliterate-text-with-nodejs"></a>Hızlı Başlangıç: Node.js ile metni başka alfabeye çevirme

Bu hızlı başlangıçta, Translator Metin Çevirisi API'sini kullanarak bir dildeki metni bir betikten diğerine dönüştüreceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu çalıştırmak için [Node.js 6](https://nodejs.org/en/download/) gerekir.

Translator Metin Çevirisi API'sini kullanmak için, ayrıca abonelik anahtarınızın olması gerekir; bkz. [Translator Metin Çevirisi API'sine kaydolma](translator-text-how-to-signup.md).

## <a name="transliterate-request"></a>Karakter Dönüştürme isteği

Aşağıdaki kod, [Karakter Dönüştürme](./reference/v3-0-transliterate.md) yöntemini kullanarak bir dildeki metni bir betikten diğerine dönüştürür.

1. Sık kullandığınız kod düzenleyicisinde yeni bir Node.js projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `subscriptionKey` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```javascript
'use strict';

let fs = require ('fs');
let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'ENTER KEY HERE';

let host = 'api.cognitive.microsofttranslator.com';
let path = '/transliterate?api-version=3.0';

// Transliterate text in Japanese from Japanese script (i.e. Hiragana/Katakana/Kanji) to Latin script.
let params = '&language=ja&fromScript=jpan&toScript=latn';

// Transliterate "good afternoon".
let text = 'こんにちは';

let response_handler = function (response) {
    let body = '';
    response.on ('data', function (d) {
        body += d;
    });
    response.on ('end', function () {
        let json = JSON.stringify(JSON.parse(body), null, 4);
        console.log(json);
    });
    response.on ('error', function (e) {
        console.log ('Error: ' + e.message);
    });
};

let get_guid = function () {
  return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
    var r = Math.random() * 16 | 0, v = c == 'x' ? r : (r & 0x3 | 0x8);
    return v.toString(16);
  });
}

let Transliterate = function (content) {
    let request_params = {
        method : 'POST',
        hostname : host,
        path : path + params,
        headers : {
            'Content-Type' : 'application/json',
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
            'X-ClientTraceId' : get_guid (),
        }
    };

    let req = https.request (request_params, response_handler);
    req.write (content);
    req.end ();
}

let content = JSON.stringify ([{'Text' : text}]);

Transliterate (content);
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
> [GitHub'da Node.js örneklerini keşfedin](https://aka.ms/TranslatorGitHub?type=&language=javascript)
