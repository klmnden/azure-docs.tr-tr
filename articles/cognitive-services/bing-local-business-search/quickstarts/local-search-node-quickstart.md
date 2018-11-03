---
title: Hızlı Başlangıç - sorgu Bing yerel iş arama kullanarak Node.js API'si için bir gönderme | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Düğümünde Bing yerel iş arama API'sini kullanmaya başlayın.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-local-business
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: ca05941a499d6bc3e183919de7b407cfba53038d
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958262"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-using-nodejs"></a>Hızlı Başlangıç: Bing yerel iş arama Node.js kullanarak API için bir sorgu gönderin.

Bu hızlı başlangıçta, Azure Bilişsel hizmet olduğu Bing yerel iş arama API'si için istekleri göndermeye başlamak için kullanın. Bu basit uygulama, node.js'de yazılmış olsa da API'si bir RESTful Web tüm programlama dillerini HTTP isteğinde bulunan ve JSON ayrıştırma özelliğine sahip uyumlu hizmetidir.
 
Bu örnek uygulama, arama sorgusu için API'sinden yerel yanıt verilerini alır. `hotel in Bellevue`.

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/download/)’in en son sürümü.

* [JavaScript isteği kitaplığı](https://github.com/request/request)

Olmalıdır bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) Bing API'leri ile. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) bu hızlı başlangıç için yeterlidir. Ücretsiz deneme tarafından sağlanan erişim anahtarı kullanın.

##<a name="code-scenario"></a>Kod senaryosu
Aşağıdaki kod, tanımlar alır ve isteği gönderir. Aşağıdaki adımları izler:

1. Ana bilgisayar ve yol ile uç noktayı belirtmek için değişkenleri bildirme.
2. Sorgu belirtin ve sorgu parametresi ekleyin. 
3. Yanıt için bir işleyici işlevi oluşturma.
4. İsteği oluşturur ve Ocp-Apim-Subscription-Key üstbilgisi ekler arama işlevini tanımlar.
5. Search işlevini çalıştırma. 

Bu tanıtımda kullanılan kodun tamamı aşağıda verilmiştir:

````
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'your-access-key';

let host = 'api.cognitive.microsoft.com/bing';
let path = '/v7.0/localbusinesses/search';

let mkt = 'en-US';
let q = 'hotel in Bellevue';

let params = '?q=' + encodeURI(q) + "&appid=" + accessKey + "&traffictype=Internal_monitor&mkt=" + mkt;

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
- [Yerel iş arama hızlı başlangıç](local-quickstart.md)
- [Yerel iş arama Java hızlı başlangıç](local-search-java-quickstart.md)
- [Yerel iş arama Python hızlı başlangıç](local-search-python-quickstart.md)
