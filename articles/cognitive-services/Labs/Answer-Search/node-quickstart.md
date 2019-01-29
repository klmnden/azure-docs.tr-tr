---
title: 'Hızlı Başlangıç: Proje yanıt arama, düğüm'
description: Node ile Yanıt Arama Projesini kullanmaya başlayın.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: answer-search
ms.topic: quickstart
ms.date: 04/13/2018
ms.author: rosh
ms.openlocfilehash: 3fd10bd08aa86458173dd1d88e2767f6f9ca4191
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104460"
---
# <a name="quickstart-project-answer-search-with-node"></a>Hızlı Başlangıç: Proje düğümü ile yanıt arama

Aşağıdaki Node örneği Yosemite Ulusal Parkı hakkındaki bilgileri almak için bir sorgu oluşturur.

## <a name="prerequisites"></a>Önkoşullar

[Bilişsel Hizmetler Laboratuvarları](https://aka.ms/answersearchsubscription) ücretsiz denemesi için erişim anahtarı alın

Bu örnekte Node v8.9.4 kullanılmıştır

## <a name="code-scenario"></a>Kod senaryosu 

Aşağıdaki kod, yanıtları alır.
Aşağıdaki adımları izler:
1. Ana bilgisayar ve yol ile uç noktayı belirtmek için değişkenleri bildirme.
2. Önizleme için sorgu URL'sini belirtme ve sorgu parametresini ekleme.  
3. Yanıt için bir işleyici işlevi oluşturma.
4. İsteği oluşturan ve *Ocp-Apim-Subscription-Key* üst bilgisini ekleyen Search işlevini tanımlama.
5. Search işlevini çalıştırma. 

Bu tanıtımda kullanılan kodun tamamı aşağıda verilmiştir:

```
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

```

## <a name="next-steps"></a>Sonraki adımlar
- [C# örnek kodu](c-sharp-quickstart.md)
- [Java hızlı başlangıcı](java-quickstart.md)
- [Python hızlı başlangıcı](python-quickstart.md)