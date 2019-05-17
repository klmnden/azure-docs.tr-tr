---
title: "Hızlı Başlangıç: Haber arama - Bing haber arama SDK'sı için Node.js gerçekleştirin"
titleSuffix: Azure Cognitive Services
description: Yanıta işlemek ve bu hızlı başlangıçta Node.js için Bing haber arama SDK'sını kullanarak haber arama için kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 01/10/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: cf13fe5279be9606197abda624c33dbc4f233b34
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65798804"
---
# <a name="quickstart-perform-a-news-search-with-the-bing-news-search-sdk-for-nodejs"></a>Hızlı Başlangıç: Node.js için Bing haber arama SDK'sı ile haber araması

Bu hızlı başlangıçta, Haberler Bing haber arama SDK'sı için Node.js için aramaya başlamak için kullanın. Bing haber arama çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js).

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/)

Bing haber arama SDK'sını kullanarak bir konsol uygulaması ayarlamak için:
1. Çalıştırma `npm install ms-rest-azure` geliştirme ortamınızda.
2. Çalıştırma `npm install azure-cognitiveservices-newssearch` geliştirme ortamınızda.


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. `CognitiveServicesCredentials` nesnesinin bir örneğini oluşturun. Abonelik anahtarınız ve bir arama terimi için değişkenler oluşturun.

    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let search_term = 'Winter Olympics'
    ```

2. İstemci örneği:
    
    ```javascript
    const NewsSearchAPIClient = require('azure-cognitiveservices-newssearch');
    let client = new NewsSearchAPIClient(credentials);
    ```

## <a name="send-a-search-query"></a>Bir arama sorgusu gönderin

1. İstemci, bu durumda "Kış Olimpiyatları" bir sorgu terimine bulmak için kullanın:
    
    ```javascript
    client.newsOperations.search(search_term).then((result) => {
        console.log(result.value);
    }).catch((err) => {
        throw err;
    });
    ```

Kod konsola `result.value` öğelerini yazdırır ve metin ayrıştırması gerçekleştirmez. Varsa sonuçlar kategorilere ayrılmış şekilde şunları içerir:

- `_type: 'NewsArticle'`
- `_type: 'WebPage'`
- `_type: 'VideoObject'`
- `_type: 'ImageObject'`

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](tutorial-bing-news-search-single-page-app.md)
