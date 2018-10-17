---
title: 'Hızlı başlangıç: URL Önizleme Projesi, Node.js'
titlesuffix: Azure Cognitive Services
description: Azure'da Microsoft Bilişsel Hizmetler kapsamındaki URL Önizleme özelliğini kullanmaya başlayın.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: project-url-preview
ms.topic: quickstart
ms.date: 03/16/2018
ms.author: rosh
ms.openlocfilehash: b8410127ed9a0444ab400eb3242868a85bd5b2c9
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48883576"
---
# <a name="quickstart-url-preview-with-nodejs"></a>Hızlı başlangıç: Node.js içe URL Önizleme 

Aşağıdaki Node örneği SwiftKey Web sitesi için bir URL Önizlemesi oluşturur: https://swiftkey.com/en.

## <a name="prerequisites"></a>Ön koşullar

[Bilişsel Hizmetler Laboratuvarları](https://aka.ms/answersearchsubscription) ücretsiz denemesi için erişim anahtarı alın

## <a name="code-scenario"></a>Kod senaryosu 

Aşağıdaki kod URL Önizleme verilerini alır.
Aşağıdaki adımları izler:
1. Ana bilgisayar ve yol ile uç noktayı belirtmek için değişkenleri bildirme.
2. Önizleme için sorgu URL'sini belirtme ve sorgu parametresini ekleme.  
3. Yanıt için bir işleyici işlevi oluşturma.
4. İsteği oluşturan ve *Ocp-Apim-Subscription-Key* üst bilgisini ekleyen Search işlevini tanımlama.
5. Search işlevini çalıştırma. 

Bu tanıtımda kullanılan kodun tamamı aşağıda verilmiştir:

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
- [C# örnek kodu](csharp.md)
- [Java hızlı başlangıcı](java-quickstart.md)
- [JavaScript hızlı başlangıcı](javascript.md)
- [Python hızlı başlangıcı](python-quickstart.md)