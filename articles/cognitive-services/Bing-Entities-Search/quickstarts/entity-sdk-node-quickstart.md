---
title: "Hızlı Başlangıç: Node.js için Bing varlık arama SDK'sı ile arama isteği gönder"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Node.js için Bing varlık arama SDK'sı ile varlıkları aramak için kullanın
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: v-gedod
ms.openlocfilehash: 015a2d344b066bd7b65c3228a2795c3395793f2b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60737049"
---
# <a name="quickstart-send-a-search-request-with-the-bing-entity-search-sdk-for-nodejs"></a>Hızlı Başlangıç: Node.js için Bing varlık arama SDK'sı ile arama isteği gönder

Bu hızlı başlangıçta, Node.js için Bing varlık arama SDK'sı ile varlıkları için aramaya başlamak için kullanın. Bing varlık arama çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js).

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/download/)’in en son sürümü.

* [Bing varlık arama SDK'sı için Node.js](https://www.npmjs.com/package/azure-cognitiveservices-entitysearch)

Bing varlık arama SDK'sını yüklemek için:

1. Çalıştırma `npm install ms-rest-azure` geliştirme ortamınızda.
2. Çalıştırma `npm install azure-cognitiveservices-entitysearch` geliştirme ortamınızda.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE veya düzenleyici yeni bir JavaScript dosyası oluşturun ve aşağıdaki koşulları ekleyin. 
    
    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const EntitySearchAPIClient = require('azure-cognitiveservices-entitysearch');
    ```

2. Bir örneğini oluşturmak `CognitiveServicesCredentials` abonelik anahtarınızı kullanarak. Ardından arama istemcisi örneği ile oluşturun.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let entitySearchApiClient = new EntitySearchAPIClient(credentials);
    ```

## <a name="send-a-request-and-receive-a-response"></a>Bir istek gönderir ve yanıt

1. İle bir varlıkları arama isteği Gönder `entitiesOperations.search()`. Bir yanıt aldıktan sonra yazdırabilirsiniz `queryContext`, sayı, döndürülen sonuçlar ve ilk sonucu açıklaması.
      
    ```javascript
    entitySearchApiClient.entitiesOperations.search('seahawks').then((result) => {
        console.log(result.queryContext);
        console.log(result.entities.value);
        console.log(result.entities.value[0].description);
    }).catch((err) => {
        throw err;
    });
    ```

<!-- Removing until we can replace with a sanitized version.
![Entity results](media/entity-search-sdk-node-quickstart-results.png)
-->

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../tutorial-bing-entities-search-single-page-app.md)

* [Bing varlık arama API'si nedir?](../overview.md )