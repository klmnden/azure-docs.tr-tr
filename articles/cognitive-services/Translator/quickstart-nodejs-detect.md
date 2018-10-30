---
title: 'Hızlı Başlangıç: Metinden dili tanımlama, Node.js - Translator Metin Çevirisi API’si'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Node.js ile Translator Metin Çevirisi API’sini kullanarak kaynak metnin dilini tanımlayacaksınız.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/21/2018
ms.author: erhopf
ms.openlocfilehash: 15c8b8077caf7c1235d0eff0429f7ada11e533ff
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49644683"
---
# <a name="quickstart-identify-language-from-text-with-the-translator-text-rest-api-nodejs"></a>Hızlı Başlangıç: Translator Metin Çevirisi REST API’si (Node.js) ile metin dili tanımlama

Bu hızlı başlangıçta, Translator Metin Çevirisi API'sini kullanarak kaynak metnin dilini tanımlayacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu çalıştırmak için [Node.js 6](https://nodejs.org/en/download/) gerekir.

Translator Metin Çevirisi API'sini kullanmak için, ayrıca abonelik anahtarınızın olması gerekir; bkz. [Translator Metin Çevirisi API'sine kaydolma](translator-text-how-to-signup.md).

## <a name="detect-request"></a>Algılama isteği

Aşağıdaki kod, [Algılama](./reference/v3-0-detect.md) yöntemini kullanarak kaynak metnin dilini tanımlar.

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
let path = '/detect?api-version=3.0';

let params = '';

let text = 'Salve, mondo!';

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

let Detect = function (content) {
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

Detect (content);
```

## <a name="detect-response"></a>Algılama yanıtı

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
[
  {
    "language": "it",
    "score": 1.0,
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "alternatives": [
      {
        "language": "pt",
        "score": 1.0,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      },
      {
        "language": "en",
        "score": 1.0,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      }
    ]
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Çeviri ve başka alfabeye çevirme gibi bu ve diğer hızlı başlangıçlardaki örnek kodlarla birlikte GitHub’daki diğer örnek Translator Metin Çevirisi projelerini keşfedin.

> [!div class="nextstepaction"]
> [GitHub'da Node.js örneklerini keşfedin](https://aka.ms/TranslatorGitHub?type=&language=javascript)
