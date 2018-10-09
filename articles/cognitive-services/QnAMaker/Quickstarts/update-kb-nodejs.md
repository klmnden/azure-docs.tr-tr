---
title: 'Hızlı Başlangıç: Node.js bilgi bankası güncelleştirme - Soru-Cevap Oluşturucu'
titleSuffix: Azure Cognitive Services
description: Soru-Cevap Oluşturma için Node.js’de nasıl bilgi bankası güncelleştirilir?
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 09/12/2018
ms.author: diberry
ms.openlocfilehash: a987993da5202abc9b543aa2dba0f080a622e199
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033628"
---
# <a name="update-a-knowledge-base-in-nodejs"></a>Node.js’de bilgi bankası güncelleştirme

Aşağıdaki kod, [Güncelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600) yöntemini kullanarak mevcut bir bilgi bankasını güncelleştirir.

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-nodejs-repo-note.md)]

Henüz bir bilgi bankanız yoksa, bu hızlı başlangıçta kullanmak için bir örneğini oluşturabilirsiniz: [Yeni bilgi bankası oluşturma](create-new-kb-nodejs.md).

1. Sık kullandığınız IDE’de yeni bir Node.js projesi oluşturun. JavaScript sürüm ECMAScript 6+ kullanın.
1. Aşağıda sağlanan kodu ekleyin.
1. `subscriptionKey` değerini geçerli bir abonelik anahtarı ile değiştirin.
1. `kb` değerini geçerli bir bilgi bankası kimliği ile değiştirin. [Soru-Cevap Oluşturma bilgi bankalarınızdan](https://www.qnamaker.ai/Home/MyServices) birine giderek bu değeri bulun. Güncelleştirmek istediğiniz bilgi bankasını seçin. Söz konusu sayfaya geldiğinizde, aşağıda gösterildiği gibi URL’de 'kdid=' öğesini bulun. Kod örneğiniz için değerini kullanın.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

1. Programı çalıştırın.

```nodejs
'use strict';

let fs = require ('fs');
let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
let subscriptionKey = 'ADD KEY HERE';

// Replace this with a valid knowledge base ID.
let kb = 'ADD ID HERE';

// Represents the various elements used to create HTTP request URIs
// for QnA Maker operations.
let host = 'westus.api.cognitive.microsoft.com';
let service = '/qnamaker/v4.0';
let method = '/knowledgebases/';

// Formats and indents JSON for display.
let pretty_print = function(s) {
    return JSON.stringify(JSON.parse(s), null, 4);
}

// Call 'callback' after we have the entire response.
let response_handler = function (callback, response) {
    let body = '';
    response.on('data', function(d) {
        body += d;
    });
    response.on('end', function() {
    // Calls 'callback' with the status code, headers, and body of the response.
    callback ({ status : response.statusCode, headers : response.headers, body : body });
    });
    response.on('error', function(e) {
        console.log ('Error: ' + e.message);
    });
};

// HTTP response handler calls 'callback' after we have the entire response.
let get_response_handler = function(callback) {
    // Return a function that takes an HTTP response and is closed over the specified callback.
    // This function signature is required by https.request, hence the need for the closure.
    return function(response) {
        response_handler(callback, response);
    }
}

// Calls 'callback' after we have the entire PATCH request response.
let patch = function(path, content, callback) {
    let request_params = {
        method : 'PATCH',
        hostname : host,
        path : path,
        headers : {
            'Content-Type' : 'application/json',
            'Content-Length' : content.length,
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

    // Pass the callback function to the response handler.
    let req = https.request(request_params, get_response_handler(callback));
    req.write(content);
    req.end ();
}

// Calls 'callback' after we have the entire GET request response.
let get = function(path, callback) {
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

    // Pass the callback function to the response handler.
    let req = https.request(request_params, get_response_handler(callback));
    req.end ();
}

// Calls 'callback' after we have the response from the /knowledgebases PATCH method.
let update_kb = function(path, req, callback) {
    console.log('Calling ' + host + path + '.');
    // Send the PATCH request.
    patch(path, req, function (response) {
        // Extract the data we want from the PATCH response and pass it to the callback function.
        callback({ operation : response.headers.location, response : response.body });
    });
}

// Calls 'callback' after we have the response from the GET request to check the status.
let check_status = function(path, callback) {
    console.log('Calling ' + host + path + '.');
    // Send the GET request.
    get(path, function (response) {
        // Extract the data we want from the GET response and pass it to the callback function.
        callback({ wait : response.headers['retry-after'], response : response.body });
    });
}

// Dictionary that holds the knowledge base. Modify knowledge base here.
let req = {
  'add': {
    'qnaList': [
      {
        'id': 1,
        'answer': 'You can change the default message if you use the QnAMakerDialog. See this for details: https://docs.botframework.com/en-us/azure-bot-service/templates/qnamaker/#navtitle',
        'source': 'Custom Editorial',
        'questions': [
          'How can I change the default message from QnA Maker?'
        ],
        'metadata': []
      }
    ],
    'urls': []
  },
  'update' : {
    'name' : 'New KB Name'
  },
  'delete': {
    'ids': [
      0
    ]
  }
};

var path = service + method + kb;
// Convert the request to a string.
let content = JSON.stringify(req);

// Sends the request to update the knowledge base.
update_kb(path, content, function (result) {
    console.log(pretty_print(result.response));
    // Loop until the operation is complete.
    let loop = function() {
        path = service + result.operation;
        // Check the status of the operation.
        check_status(path, function(status) {
            // Write out the status.
            console.log(pretty_print(status.response));
            // Convert the status into an object and get the value of the operationState field.
            var state = (JSON.parse(status.response)).operationState;
            // If the operation isn't complete, wait and query again.
            if (state == 'Running' || state == 'NotStarted') {
                console.log('Waiting ' + status.wait + ' seconds...');
                setTimeout(loop, status.wait * 1000);
            }
        });
    }
    // Begin the loop.
    loop();
});
```

## <a name="understand-what-qna-maker-returns"></a>Soru-Cevap Oluşturma’nın ne döndürdüğünü anlama

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür. Sonuçlarınız biraz farklılık gösterebilir. Son çağrı "Başarılı" durumunu döndürürse, bilgi bankanız başarıyla güncelleştirilmiştir. Sorun gidermek için Soru-Cevap Oluşturma API'sine ait [Bilgi Bankasını Güncelleştir](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600) yanıt kodlarına başvurun.

```json
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-04-13T01:49:48Z",
  "lastActionTimestamp": "2018-04-13T01:49:48Z",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "5156f64e-e31d-4638-ad7c-a2bdd7f41658"
}
...
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-04-13T01:49:48Z",
  "lastActionTimestamp": "2018-04-13T01:49:50Z",
  "resourceLocation": "/knowledgebases/140a46f3-b248-4f1b-9349-614bfd6e5563",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "5156f64e-e31d-4638-ad7c-a2bdd7f41658"
}
Press any key to continue.
```

Bilgi bankanız güncelleştirildikten sonra Soru-Cevap Oluşturma Portalı’nızdaki [Bilgi bankalarım](https://www.qnamaker.ai/Home/MyServices) sayfasından görüntüleyebilirsiniz. Bilgi bankanızda ‘Yeni Bilgi Bankası Adı’ adlı yeni bir ad ve ek bir Soru-Cevap çifti olduğuna dikkat edin.

Bilgi bankanızın diğer öğelerini değiştirmek için Soru-Cevap Oluşturma [JSON şemasına](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600) başvurun ve `req` dizesini değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)