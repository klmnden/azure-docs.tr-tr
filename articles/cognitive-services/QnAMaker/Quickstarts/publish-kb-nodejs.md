---
title: 'Hızlı Başlangıç: Node.js Bilgi Bankası yayımlama - Soru-Cevap Oluşturucu'
titleSuffix: Azure Cognitive Services
description: Soru-Cevap Oluşturma için Node.js’de nasıl bilgi bankası yayımlanılır?
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.technology: qna-maker
ms.topic: quickstart
ms.date: 09/12/2018
ms.author: diberry
ms.openlocfilehash: 00642661995e16bda9ad995e69545b28468779c5
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47040947"
---
# <a name="publish-a-knowledge-base-in-nodejs"></a>Node.js’de bilgi bankası yayımlama

Aşağıdaki kod, [Yayımla](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe) yöntemini kullanarak mevcut bir bilgi bankasını yayımlar.

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-nodejs-repo-note.md)]

Henüz bir bilgi bankanız yoksa, bu hızlı başlangıçta kullanmak için bir örneğini oluşturabilirsiniz: [Yeni bilgi bankası oluşturma](create-new-kb-nodejs.md).

1. Sık kullandığınız IDE'de yeni bir Node projesi oluşturun.
1. Aşağıda sağlanan kodu ekleyin.
1. `subscriptionKey` değerini geçerli bir abonelik anahtarı ile değiştirin.
1. `kb` değerini geçerli bir bilgi bankası kimliği ile değiştirin. [Soru-Cevap Oluşturma bilgi bankalarınızdan](https://www.qnamaker.ai/Home/MyServices) birine giderek bu değeri bulun. Yayımlamak istediğiniz bilgi bankasını seçin. Söz konusu sayfaya geldiğinizde, aşağıda gösterildiği gibi URL’de 'kdid=' öğesini bulun. Kod örneğiniz için değerini kullanın.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)
1. Programı çalıştırın.

``` Node.js
'use strict';

let fs = require ('fs');
let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
let subscriptionKey = 'ENTER KEY HERE';

// NOTE: Replace this with a valid knowledge base ID.
let kb = 'ENTER ID HERE';

let host = 'westus.api.cognitive.microsoft.com';
let service = '/qnamaker/v4.0';
let method = '/knowledgebases/';

let pretty_print = function (s) {
    return JSON.stringify(JSON.parse(s), null, 4);
}

// callback is the function to call when we have the entire response.
let response_handler = function (callback, response) {
    let body = '';
    response.on ('data', function (d) {
        body += d;
    });
    response.on ('end', function () {
// Call the callback function with the status code, headers, and body of the response.
        callback ({ status : response.statusCode, headers : response.headers, body : body });
    });
    response.on ('error', function (e) {
        console.log ('Error: ' + e.message);
    });
};

// Get an HTTP response handler that calls the specified callback function when we have the entire response.
let get_response_handler = function (callback) {
// Return a function that takes an HTTP response, and is closed over the specified callback.
// This function signature is required by https.request, hence the need for the closure.
    return function (response) {
        response_handler (callback, response);
    }
}

// callback is the function to call when we have the entire response from the POST request.
let post = function (path, content, callback) {
    let request_params = {
        method : 'POST',
        hostname : host,
        path : path,
        headers : {
            'Content-Type' : 'application/json',
            'Content-Length' : content.length,
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.write (content);
    req.end ();
}

// callback is the function to call when we have the response from the /knowledgebases POST method.
let publish_kb = function (path, req, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the POST request.
    post (path, req, function (response) {
// Extract the data we want from the POST response and pass it to the callback function.
        if (response.status == '204') {
            let result = {'result':'Success'};
            callback (JSON.stringify(result));
        }
        else {
            callback (response.body);
        }
    });
}

var path = service + method + kb;
publish_kb (path, '', function (result) {
    console.log (pretty_print(result));
});
```

## <a name="understand-what-qna-maker-returns"></a>Soru-Cevap Oluşturma’nın ne döndürdüğünü anlama

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
{
  "result": "Success."
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)