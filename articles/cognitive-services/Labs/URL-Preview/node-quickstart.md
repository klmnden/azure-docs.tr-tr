---
title: Proje URL'si Önizleme - Microsoft Bilişsel hizmetler için node.js hızlı başlangıç | Microsoft Docs
description: URL önizleme Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki kullanmaya başlayın.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/16/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 195033d2740b11873baae095cec028dc8d19ce49
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354011"
---
# <a name="url-preview-node-quickstart"></a>URL önizleme düğümü hızlı başlangıç

Aşağıdaki düğümü örnek SwiftKey Web sitesi için Url Önizleme oluşturur: https://swiftkey.com/en.

## <a name="prerequisites"></a>Önkoşullar

Ücretsiz deneme sürümü için bir erişim anahtarı alma [Bilişsel hizmetler Laboratuvarları](https://aka.ms/answersearchsubscription)

## <a name="code-scenario"></a>Kod senaryosu 

Aşağıdaki kod URL önizleme verilerini alır.
Aşağıdaki adımlarda uygulanır:
1. Bitiş noktası tarafından konak ve yol belirtmek için değişkenleri bildirin.
2. Önizleme için sorgu URL'sini belirtin ve sorgu parametresi ekleyin.  
3. Yanıt için bir işleyici işlevi oluşturun.
4. İsteği oluşturur ve ekleyen arama işlevini tanımlayan *Apim abonelik anahtar Ocp* üstbilgi.
5. Arama işlevini çalıştırın. 

Bu Tanıtım için tam kod aşağıdaki gibidir:

````
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'YOUR-ACCESS-KEY'; 
let host = 'api.labs.cognitive.microsoft.com';
let path = '/urlpreview/v7.0/search';

let mkt = 'en-US';
let q = 'https://swiftkey.com/';

let params = '?q=' + encodeURI(q);

let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        let body_ = JSON.parse(body);
        let body__ = JSON.stringify(body_, null, '  ');
        console.log(body__);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};

let Search = function () {
    let request_params = {
        method: 'GET',
        hostname: host,
        path: path + params,
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey,
        }
    };

    let req = https.request(request_params, response_handler);
    req.end();
}

Search();

````

## <a name="next-steps"></a>Sonraki adımlar
- [C# kod örneği](csharp.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [JavaScript hızlı başlangıç](javascript.md)
- [Python hızlı başlangıç](python-quickstart.md)