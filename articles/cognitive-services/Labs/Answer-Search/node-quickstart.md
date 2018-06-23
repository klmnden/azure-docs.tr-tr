---
title: Microsoft Bilişsel hizmetler, proje yanıt arama için düğüm hızlı başlangıç | Microsoft Docs
description: Proje yanıt arama, Microsoft Azure'da Bilişsel hizmetler kullanmaya başlayın.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 36b2709d39230aae7929164ba4c9306f57043b43
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353974"
---
# <a name="project-answer-search-node-quickstart"></a>Proje yanıt arama düğümü hızlı başlangıç

Aşağıdaki düğümü örnek Yosemite National Park hakkında bilgi için bir sorgu oluşturur.

## <a name="prerequisites"></a>Önkoşullar

Ücretsiz deneme sürümü için bir erişim anahtarı alma [Bilişsel hizmetler Laboratuvarları](https://aka.ms/answersearchsubscription)

Bu örnek, düğüm v8.9.4 kullanır.

## <a name="code-scenario"></a>Kod senaryosu 

Aşağıdaki kod yanıtları alır.
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
let subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'; 

let host = 'api.labs.cognitive.microsoft.com';
let path = '/answerSearch/v7.0/search';

let mkt = 'en-us';
let q = 'Yosemite National Park';

let params = '?q=' + encodeURI(q) + '&mkt=en-us';

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
- [C# kod örneği](c-sharp-quickstart.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [Python hızlı başlangıç](python-quickstart.md)