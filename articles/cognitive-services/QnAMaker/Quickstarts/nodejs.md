---
title: REST API (V4) - Node.js - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Node.js REST tabanlı bilgi alın ve kod örnekleri, hızlı bir şekilde yardımcı olmak için Microsoft Bilişsel hizmetler, Azure üzerinde Microsoft Translator metin çevirisi API'si ile çalışmaya başlamak.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/13/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: ba4af5d3b6612614dee54db7fdb1287ff7136321
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60913536"
---
# <a name="how-to-use-the-qna-maker-rest-api-with-nodejs"></a>Node.js ile soru-cevap Oluşturucu REST API'si kullanma 
<a name="HOLTop"></a>

Bu makalede nasıl kullanılacağını gösterir [Microsoft soru-cevap Oluşturucu API'si](../Overview/overview.md) aşağıdakileri yapmak için Node.js ile.

- [Yeni bilgi bankası oluşturma.](#Create)
- [Var olan bilgi bankasını güncelleştirme.](#Update)
- [Bilgi bankası oluşturma veya güncelleştirme isteğinin durumunu alma.](#Status)
- [Var olan bilgi bankasını yayımlama.](#Publish)
- [Var olan bilgi bankasının içeriğini değiştirme.](#Replace)
- [Bilgi bankasının içeriğini indirme.](#GetQnA)
- [Bilgi bankasını kullanarak bir soruya yanıt bulma.](#GetAnswers)
- [Bir bilgi bankası hakkında bilgi alma.](#GetKB)
- [Belirli bir kullanıcıya ait olan tüm bilgi bankaları hakkında bilgi alma.](#GetKBsByUser)
- [Bilgi bankasını silme.](#Delete)
- [Geçerli uç nokta anahtarlarını alma.](#GetKeys)
- [Geçerli uç nokta anahtarlarını yeniden oluşturma.](#PutKeys)
- [Büyük küçük harf duyarsız word değişiklikleri geçerli kümesini alın.](#GetAlterations)
- [Büyük küçük harf duyarsız word değişiklikleri geçerli kümesini değiştirin.](#PutAlterations)

[!INCLUDE [Code is available in Azure-Samples GitHub repo](../../../../includes/cognitive-services-qnamaker-nodejs-repo-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu kodu çalıştırmak için [Node.js 6](https://nodejs.org/en/download/) gerekir.

**Microsoft Soru-Cevap Oluşturma API'sine** sahip bir [Bilişsel Hizmetler API hesabınızın](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Azure panonuzdan](https://portal.azure.com/#create/Microsoft.CognitiveServices) ücretli bir abonelik anahtarına ihtiyacınız olacak.

<a name="Create"></a>

## <a name="create-knowledge-base"></a>Bilgi bankası oluşturma

Aşağıdaki kod, [Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff) yöntemini kullanarak yeni bir bilgi bankası oluşturur.

1. Sık kullandığınız IDE’de yeni bir Node.js projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```javascript
'use strict';

let fs = require ('fs');
let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
let subscriptionKey = 'ENTER KEY HERE';

let host = 'westus.api.cognitive.microsoft.com';
let service = '/qnamaker/v4.0';
let method = '/knowledgebases/create';

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
            'Content-Length' : Buffer.byteLength(content),
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.write (content);
    req.end ();
}

// callback is the function to call when we have the entire response from the GET request.
let get = function (path, callback) {
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.end ();
}

// callback is the function to call when we have the response from the /knowledgebases/create POST method.
let create_kb = function (path, req, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the POST request.
    post (path, req, function (response) {
// Extract the data we want from the POST response and pass it to the callback function.
        callback ({ operation : response.headers.location, response : response.body });
    });
}

// callback is the function to call when we have the response from the GET request to check the status.
let check_status = function (path, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the GET request.
    get (path, function (response) {
// Extract the data we want from the GET response and pass it to the callback function.
        callback ({ wait : response.headers['retry-after'], response : response.body });
    });
}

let req = {
  "name": "QnA Maker FAQ",
  "qnaList": [
    {
      "id": 0,
      "answer": "You can use our REST APIs to manage your Knowledge Base. See here for details: https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600",
      "source": "Custom Editorial",
      "questions": [
        "How do I programmatically update my Knowledge Base?"
      ],
      "metadata": [
        {
          "name": "category",
          "value": "api"
        }
      ]
    }
  ],
  "urls": [
    "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
    "https://docs.microsoft.com/bot-framework/resources-bot-framework-faq"
  ],
  "files": []
};

var path = service + method;
// Convert the request to a string.
let content = JSON.stringify(req);
create_kb (path, content, function (result) {
// Write out the response from the /knowledgebases/create method.
    console.log (pretty_print(result.response));
// Loop until the operation is complete.
    let loop = function () {
        path = service + result.operation;
// Check the status of the operation.
        check_status (path, function (status) {
// Write out the status.
            console.log (pretty_print(status.response));
// Convert the status into an object and get the value of the operationState field.
            var state = (JSON.parse(status.response)).operationState;
// If the operation isn't complete, wait and query again.
            if (state == 'Running' || state == 'NotStarted') {
                console.log ('Waiting ' + status.wait + ' seconds...');
                setTimeout(loop, status.wait * 1000);
            }
        });
    }
// Begin the loop.
    loop();
});
```

**Bilgi bankası yanıtı oluşturma**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:30Z",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
...
{
  "operationState": "Running",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:30Z",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
...
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:46Z",
  "resourceLocation": "/knowledgebases/b0288f33-27b9-4258-a304-8b9f63427dad",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
```

[Başa dön](#HOLTop)

<a name="Update"></a>

## <a name="update-knowledge-base"></a>Bilgi bankası güncelleştirme

Aşağıdaki kod, [Güncelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600) yöntemini kullanarak mevcut bir bilgi bankasını güncelleştirir.

1. Sık kullandığınız IDE’de yeni bir Node.js projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```javascript
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

// callback is the function to call when we have the entire response from the PATCH request.
let patch = function (path, content, callback) {
    let request_params = {
        method : 'PATCH',
        hostname : host,
        path : path,
        headers : {
            'Content-Type' : 'application/json',
            'Content-Length' : Buffer.byteLength(content),
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.write (content);
    req.end ();
}

// callback is the function to call when we have the entire response from the GET request.
let get = function (path, callback) {
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.end ();
}

// callback is the function to call when we have the response from the /knowledgebases PATCH method.
let update_kb = function (path, req, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the PATCH request.
    patch (path, req, function (response) {
// Extract the data we want from the PATCH response and pass it to the callback function.
        callback ({ operation : response.headers.location, response : response.body });
    });
}

// callback is the function to call when we have the response from the GET request to check the status.
let check_status = function (path, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the GET request.
    get (path, function (response) {
// Extract the data we want from the GET response and pass it to the callback function.
        callback ({ wait : response.headers['retry-after'], response : response.body });
    });
}

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
    'urls': [
      'https://docs.microsoft.com/azure/cognitive-services/Emotion/FAQ'
    ]
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
update_kb (path, content, function (result) {
    console.log (pretty_print(result.response));
// Loop until the operation is complete.
    let loop = function () {
        path = service + result.operation;
// Check the status of the operation.
        check_status (path, function (status) {
// Write out the status.
            console.log (pretty_print(status.response));
// Convert the status into an object and get the value of the operationState field.
            var state = (JSON.parse(status.response)).operationState;
// If the operation isn't complete, wait and query again.
            if (state == 'Running' || state == 'NotStarted') {
                console.log ('Waiting ' + status.wait + ' seconds...');
                setTimeout(loop, status.wait * 1000);
            }
        });
    }
// Begin the loop.
    loop();
});
```

**Bilgi bankası güncelleştirme yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

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

[Başa dön](#HOLTop)

<a name="Status"></a>

## <a name="get-request-status"></a>İstek alma durumu

Bilgi bankası oluşturma veya güncelleştirme isteğinin durumunu denetlemek için [İşlem](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails) yöntemini çağırabilirsiniz. Bu yöntemin nasıl kullanıldığını görmek için lütfen [Oluşturma](#Create) veya [Güncelleştirme](#Update) yönteminin örnek koduna bakın.

[Başa dön](#HOLTop)

<a name="Publish"></a>

## <a name="publish-knowledge-base"></a>Bilgi bankası yayımlama

Aşağıdaki kod, [Yayımla](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe) yöntemini kullanarak mevcut bir bilgi bankasını yayımlar.

1. Sık kullandığınız IDE’de yeni bir Node.js projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```javascript
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
            'Content-Length' : Buffer.byteLength(content),
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

**Bilgi bankası yanıtını yayımlama**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "result": "Success."
}
```

[Başa dön](#HOLTop)

<a name="Replace"></a>

## <a name="replace-knowledge-base"></a>Bilgi bankasını değiştirme

Aşağıdaki kod [Replace](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/knowledgebases_publish) metodunu kullanarak belirtilen bilgi bankasının içeriğini değiştirir.

1. Sık kullandığınız IDE’de yeni bir Node.js projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```javascript
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

// callback is the function to call when we have the entire response from the PUT request.
let put = function (path, content, callback) {
    let request_params = {
        method : 'PUT',
        hostname : host,
        path : path,
        headers : {
            'Content-Type' : 'application/json',
            'Content-Length' : Buffer.byteLength(content),
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.write (content);
    req.end ();
}

// callback is the function to call when we have the response from the /knowledgebases PUT method.
let replace_kb = function (path, req, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the PUT request.
    put (path, req, function (response) {
// Extract the data we want from the PUT response and pass it to the callback function.
        if (response.status == '204') {
            let result = {'result':'Success'};
            callback (JSON.stringify(result));
        }
        else {
            callback (response.body);
        }
    });
}

let req = {
  'qnaList': [
    {
      'id': 0,
      'answer': 'You can use our REST APIs to manage your Knowledge Base. See here for details: https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600',
      'source': 'Custom Editorial',
      'questions': [
        'How do I programmatically update my Knowledge Base?'
      ],
      'metadata': [
        {
          'name': 'category',
          'value': 'api'
        }
      ]
    }
  ]
};

var path = service + method + kb;
// Convert the request to a string.
let content = JSON.stringify(req);
replace_kb (path, content, function (result) {
    console.log (pretty_print(result));
});
```

**Bilgi bankası yanıtını değiştirme**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
    "result": "Success."
}
```

[Başa dön](#HOLTop)

<a name="GetQnA"></a>

## <a name="download-the-contents-of-a-knowledge-base"></a>Bilgi bankasının içeriğini indirme

Aşağıdaki kod [Download knowledge base](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/knowledgebases_download) metodunu kullanarak belirtilen bilgi bankasının içeriğini indirir.

1. Sık kullandığınız IDE’de yeni bir Node.js projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```javascript
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

// NOTE: Replace this with "test" or "prod".
let env = "test";

let host = 'westus.api.cognitive.microsoft.com';
let service = '/qnamaker/v4.0';
let method = `/knowledgebases/${kb}/${env}/qna/`;

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

// callback is the function to call when we have the entire response from the GET request.
let get = function (path, callback) {
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.end ();
}

// callback is the function to call when we have the response from the /knowledgebases GET method.
let get_qna = function (path, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the GET request.
    get (path, function (response) {
// Extract the data we want from the GET response and pass it to the callback function.
        callback ({ operation : response.headers.location, response : response.body });
    });
}

var path = service + method;
get_qna (path, function (result) {
    console.log (pretty_print(result.response));
});
```

**Bilgi bankası yanıtını indirme**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "qnaDocuments": [
    {
      "id": 1,
      "answer": "You can use our REST APIs to manage your Knowledge Base. See here for details: https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600",
      "source": "Custom Editorial",
      "questions": [
        "How do I programmatically update my Knowledge Base?"
      ],
      "metadata": [
        {
          "name": "category",
          "value": "api"
        }
      ]
    },
    {
      "id": 2,
      "answer": "QnA Maker provides an FAQ data source that you can query from your bot or application. Although developers will find this useful, content owners will especially benefit from this tool. QnA Maker is a completely no-code way of managing the content that powers your bot or application.",
      "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
      "questions": [
        "Who is the target audience for the QnA Maker tool?"
      ],
      "metadata": []
    },
...
  ]
}
```

[Başa dön](#HOLTop)

<a name="GetAnswers"></a>

## <a name="get-answers-to-a-question-using-a-knowledge-base"></a>Bilgi bankasını kullanarak bir soruya yanıt bulma

Aşağıdaki kod **Generate answers** metodunu kullanarak belirli bir bilgi bankasından bir soruya yanıt bulur.

1. Sık kullandığınız IDE’de yeni bir Node.js projesi oluşturun.
1. Aşağıda sağlanan kodu ekleyin.
1. `host` değerinin yerine Soru-Cevap Oluşturma aboneliğinizin web sitesi adını yazın. Daha fazla bilgi için bkz. [Soru-Cevap Oluşturma hizmeti oluşturma](../How-To/set-up-qnamaker-service-azure.md).
1. `endpoint_key` değerini, aboneliğiniz için geçerli olan bir uç nokta anahtarı ile değiştirin. Bu anahtarın abonelik anahtarınızdan farklı olduğunu unutmayın. Uç nokta anahtarlarınızı [Get endpoint keys](#GetKeys) metoduyla alabilirsiniz.
1. `kb` değerini yanıt için sorgulamak istediğiniz bilgi bankasının kimliğiyle değiştirin. Bu bilgi bankasının [Publish](#Publish) metodu kullanılarak önceden yayımlanmış olması gerektiğini unutmayın.
1. Programı çalıştırın.

```javascript
'use strict';

let fs = require ('fs');
let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// NOTE: Replace this with a valid host name.
let host = "ENTER HOST HERE";

// NOTE: Replace this with a valid endpoint key.
// This is not your subscription key.
// To get your endpoint keys, call the GET /endpointkeys method.
let endpoint_key = "ENTER KEY HERE";

// NOTE: Replace this with a valid knowledge base ID.
// Make sure you have published the knowledge base with the
// POST /knowledgebases/{knowledge base ID} method.
let kb = "ENTER KB ID HERE";

let method = "/qnamaker/knowledgebases/" + kb + "/generateAnswer";

let question = {
    'question': 'Is the QnA Maker Service free?',
    'top': 3
};

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
            'Content-Length' : Buffer.byteLength(content),
            'Authorization' : 'EndpointKey ' + endpoint_key,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.write (content);
    req.end ();
}

// callback is the function to call when we have the response from the /knowledgebases POST method.
let get_answers = function (path, req, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the POST request.
    post (path, req, function (response) {
        callback (response.body);
    });
}

// Convert the request to a string.
let content = JSON.stringify(question);
get_answers (method, content, function (result) {
// Write out the response from the /knowledgebases/create method.
    console.log (pretty_print(result));
});
```

**Yanıt alma**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "answers": [
    {
      "questions": [
        "Can I use BitLocker with the Volume Shadow Copy Service?"
      ],
      "answer": "Yes. However, shadow copies made prior to enabling BitLocker will be automatically deleted when BitLocker is enabled on software-encrypted drives. If you are using a hardware encrypted drive, the shadow copies are retained.",
      "score": 17.3,
      "id": 62,
      "source": "https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-frequently-asked-questions",
      "metadata": []
    },
...
  ]
}
```

[Başa dön](#HOLTop)

<a name="GetKB"></a>

## <a name="get-information-about-a-knowledge-base"></a>Bir bilgi bankası hakkında bilgi alma

Aşağıdaki kod [Get knowledge base details](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/knowledgebases_getknowledgebasedetails) metodunu kullanarak belirtilen bilgi bankasıyla ilgili bilgileri alır.

1. Sık kullandığınız IDE’de yeni bir Node.js projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```javascript
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

// callback is the function to call when we have the entire response from the GET request.
let get = function (path, callback) {
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.end ();
}

// callback is the function to call when we have the response from the /knowledgebases GET method.
let get_kb = function (path, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the GET request.
    get (path, function (response) {
// Extract the data we want from the GET response and pass it to the callback function.
        callback ({ operation : response.headers.location, response : response.body });
    });
}

var path = service + method + kb;
get_kb (path, function (result) {
    console.log (pretty_print(result.response));
});
```

**Bilgi bankası ayrıntılarını alma yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "id": "140a46f3-b248-4f1b-9349-614bfd6e5563",
  "hostName": "https://qna-docs.azurewebsites.net",
  "lastAccessedTimestamp": "2018-04-12T22:58:01Z",
  "lastChangedTimestamp": "2018-04-12T22:58:01Z",
  "name": "QnA Maker FAQ",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "urls": [
    "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
    "https://docs.microsoft.com/bot-framework/resources-bot-framework-faq"
  ],
  "sources": [
    "Custom Editorial"
  ]
}
```

[Başa dön](#HOLTop)

<a name="GetKBsByUser"></a>

## <a name="get-all-knowledge-bases-for-a-user"></a>Bir kullanıcının tüm bilgi bankalarını alma

Aşağıdaki kod [Get knowledge bases for user](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/knowledgebases_getknowledgebasesforuser) metodunu kullanarak belirtilen bir kullanıcının tüm bilgi bankalarıyla ilgili bilgileri alır.

1. Sık kullandığınız IDE’de yeni bir Node.js projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```javascript
'use strict';

let fs = require ('fs');
let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
let subscriptionKey = 'ENTER KEY HERE';

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

// callback is the function to call when we have the entire response from the GET request.
let get = function (path, callback) {
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.end ();
}

// callback is the function to call when we have the response from the /knowledgebases GET method.
let get_kbs = function (path, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the GET request.
    get (path, function (response) {
// Extract the data we want from the GET response and pass it to the callback function.
        callback ({ operation : response.headers.location, response : response.body });
    });
}

var path = service + method;
get_kbs (path, function (result) {
    console.log (pretty_print(result.response));
});
```

**Kullanıcının bilgi bankalarını alma yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "knowledgebases": [
    {
      "id": "081c32a7-bd05-4982-9d74-07ac736df0ac",
      "hostName": "https://qna-docs.azurewebsites.net",
      "lastAccessedTimestamp": "2018-04-12T11:51:58Z",
      "lastChangedTimestamp": "2018-04-12T11:51:58Z",
      "name": "QnA Maker FAQ",
      "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
      "urls": [],
      "sources": []
    },
    {
      "id": "140a46f3-b248-4f1b-9349-614bfd6e5563",
      "hostName": "https://qna-docs.azurewebsites.net",
      "lastAccessedTimestamp": "2018-04-12T22:58:01Z",
      "lastChangedTimestamp": "2018-04-12T22:58:01Z",
      "name": "QnA Maker FAQ",
      "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
      "urls": [
        "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
        "https://docs.microsoft.com/bot-framework/resources-bot-framework-faq"
      ],
      "sources": [
        "Custom Editorial"
      ]
    },
...
  ]
}
Press any key to continue.
```

[Başa dön](#HOLTop)

<a name="Delete"></a>

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Aşağıdaki kod [Delete knowledge base](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/knowledgebases_delete) metodunu kullanarak belirtilen bilgi bankasını siler.

1. Sık kullandığınız IDE’de yeni bir Node.js projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```javascript
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

// callback is the function to call when we have the entire response from the DELETE request.
let http_delete = function (path, content, callback) {
    let request_params = {
        method : 'DELETE',
        hostname : host,
        path : path,
        headers : {
            'Content-Type' : 'application/json',
            'Content-Length' : Buffer.byteLength(content),
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.write (content);
    req.end ();
}

// callback is the function to call when we have the response from the /knowledgebases DELETE method.
let delete_kb = function (path, req, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the DELETE request.
    http_delete (path, req, function (response) {
// Extract the data we want from the DELETE response and pass it to the callback function.
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
delete_kb (path, '', function (result) {
    console.log (pretty_print(result));
});
```

**Bilgi bankasını silme yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "result": "Success."
}
```

[Başa dön](#HOLTop)

<a name="GetKeys"></a>

## <a name="get-endpoint-keys"></a>Uç nokta anahtarlarını alma

Aşağıdaki kod [Get endpoint keys](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/endpointkeys_getendpointkeys) metodunu kullanarak geçerli uç nokta anahtarlarını alır.

1. Sık kullandığınız IDE’de yeni bir Node.js projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```javascript
'use strict';

let fs = require ('fs');
let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
let subscriptionKey = 'ENTER KEY HERE';

let host = 'westus.api.cognitive.microsoft.com';
let service = '/qnamaker/v4.0';
let method = '/endpointkeys/';

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

// callback is the function to call when we have the entire response from the GET request.
let get = function (path, callback) {
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.end ();
}

// callback is the function to call when we have the response from the /knowledgebases GET method.
let get_keys = function (path, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the GET request.
    get (path, function (response) {
// Extract the data we want from the GET response and pass it to the callback function.
        callback ({ operation : response.headers.location, response : response.body });
    });
}

var path = service + method;
get_keys (path, function (result) {
    console.log (pretty_print(result.response));
});
```

**Uç nokta anahtarlarını alma yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "primaryEndpointKey": "ac110bdc-34b7-4b1c-b9cd-b05f9a6001f3",
  "secondaryEndpointKey": "1b4ed14e-614f-444a-9f3d-9347f45a9206"
}
```

[Başa dön](#HOLTop)

<a name="PutKeys"></a>

## <a name="refresh-endpoint-keys"></a>Uç nokta anahtarlarını yenileme

Aşağıdaki kod [Refresh endpoint keys](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/endpointkeys_refreshendpointkeys) metodunu kullanarak geçerli uç nokta anahtarlarını yeniden üretir.

1. Sık kullandığınız IDE’de yeni bir Node.js projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```javascript
'use strict';

let fs = require ('fs');
let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
let subscriptionKey = 'ENTER KEY HERE';

// NOTE: Replace this with "PrimaryKey" or "SecondaryKey."
let key_type = "PrimaryKey";

let host = 'westus.api.cognitive.microsoft.com';
let service = '/qnamaker/v4.0';
let method = '/endpointkeys/';

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

// callback is the function to call when we have the entire response from the PATCH request.
let patch = function (path, content, callback) {
    let request_params = {
        method : 'PATCH',
        hostname : host,
        path : path,
        headers : {
            'Content-Type' : 'application/json',
            'Content-Length' : Buffer.byteLength(content),
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.write (content);
    req.end ();
}

// callback is the function to call when we have the response from the /knowledgebases PATCH method.
let refresh_keys = function (path, req, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the PATCH request.
    patch (path, req, function (response) {
// Extract the data we want from the PATCH response and pass it to the callback function.
        if (response.status == '204') {
            let result = {'result':'Success'};
            callback (JSON.stringify(result));
        }
        else {
            callback (response.body);
        }
    });
}

let req = {
  'wordAlterations': [
    {
      'alterations': [
        'botframework',
        'bot frame work'
      ]
    }
  ]
};

var path = service + method + key_type;
// Convert the request to a string.
let content = JSON.stringify(req);
refresh_keys (path, content, function (result) {
    console.log (pretty_print(result));
});
```

**Uç nokta anahtarlarını yenileme yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "primaryEndpointKey": "ac110bdc-34b7-4b1c-b9cd-b05f9a6001f3",
  "secondaryEndpointKey": "1b4ed14e-614f-444a-9f3d-9347f45a9206"
}
```

[Başa dön](#HOLTop)

<a name="GetAlterations"></a>

## <a name="get-word-alterations"></a>Sözcük değişikliklerini alma

Aşağıdaki kod [Download alterations](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fc) metodunu kullanarak geçerli sözcük değişikliklerini alır.

1. Sık kullandığınız IDE’de yeni bir Node.js projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```javascript
'use strict';

let fs = require ('fs');
let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
let subscriptionKey = 'ENTER KEY HERE';

let host = 'westus.api.cognitive.microsoft.com';
let service = '/qnamaker/v4.0';
let method = '/alterations/';

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

// callback is the function to call when we have the entire response from the GET request.
let get = function (path, callback) {
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.end ();
}

// callback is the function to call when we have the response from the /knowledgebases GET method.
let get_alterations = function (path, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the GET request.
    get (path, function (response) {
// Extract the data we want from the GET response and pass it to the callback function.
        callback ({ operation : response.headers.location, response : response.body });
    });
}

var path = service + method;
get_alterations (path, function (result) {
    console.log (pretty_print(result.response));
});
```

**Sözcük değişikliklerini alma yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "wordAlterations": [
    {
      "alterations": [
        "botframework",
        "bot frame work"
      ]
    }
  ]
}
```

[Başa dön](#HOLTop)

<a name="PutAlterations"></a>

## <a name="replace-word-alterations"></a>Sözcük değişikliklerini değiştirme

Aşağıdaki kod [Replace alterations](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fd) metodunu kullanarak geçerli sözcük değişikliklerini değiştirir.

1. Sık kullandığınız IDE’de yeni bir Node.js projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```javascript
'use strict';

let fs = require ('fs');
let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
let subscriptionKey = 'ENTER KEY HERE';

let host = 'westus.api.cognitive.microsoft.com';
let service = '/qnamaker/v4.0';
let method = '/alterations/';

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

// callback is the function to call when we have the entire response from the PUT request.
let put = function (path, content, callback) {
    let request_params = {
        method : 'PUT',
        hostname : host,
        path : path,
        headers : {
            'Content-Type' : 'application/json',
            'Content-Length' : Buffer.byteLength(content),
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

// Pass the callback function to the response handler.
    let req = https.request (request_params, get_response_handler (callback));
    req.write (content);
    req.end ();
}

// callback is the function to call when we have the response from the /knowledgebases PUT method.
let put_alterations = function (path, req, callback) {
    console.log ('Calling ' + host + path + '.');
// Send the PUT request.
    put (path, req, function (response) {
// Extract the data we want from the PUT response and pass it to the callback function.
        if (response.status == '204') {
            let result = {'result':'Success'};
            callback (JSON.stringify(result));
        }
        else {
            callback (response.body);
        }
    });
}

let req = {
  'wordAlterations': [
    {
      'alterations': [
        'botframework',
        'bot frame work'
      ]
    }
  ]
};

var path = service + method;
// Convert the request to a string.
let content = JSON.stringify(req);
put_alterations (path, content, function (result) {
    console.log (pretty_print(result));
});
```

**Sözcük değişikliklerini değiştirme yanıtı**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "result": "Success."
}
```

[Başa dön](#HOLTop)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)

## <a name="see-also"></a>Ayrıca bkz. 

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)
