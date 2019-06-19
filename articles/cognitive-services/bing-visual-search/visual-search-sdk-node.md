---
title: "Hızlı Başlangıç: Node.js için Bing görsel arama SDK'sını kullanarak görüntü Öngörüler elde edin"
titleSuffix: Azure Cognitive Services
description: Bing görsel arama SDK'sını kullanarak bir görüntüyü karşıya yükleme ve ilgili Öngörüler elde edin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 06/18/2018
ms.author: aahi
ms.openlocfilehash: 6f7740ebd09a1fde564613c38ab12513ca51fece
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204907"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-sdk-for-nodejs"></a>Hızlı Başlangıç: Node.js için Bing görsel arama SDK'sını kullanarak görüntü Öngörüler elde edin

Bu hızlı başlangıçta, görüntü ınsights Node.js SDK'sını kullanarak Bing görsel arama hizmetinden alma başlamak için kullanın. Bing görsel arama çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/visualSearch.js). 

## <a name="prerequisites"></a>Önkoşullar
* [Node.js](https://www.nodejs.org/)
* Node.js için Bing görsel arama SDK'sı
    * Bing görsel arama SDK'sını kullanarak bir konsol uygulaması ayarlamak için aşağıdaki komutları çalıştırın:
        1. `npm install ms-rest-azure`
        2. `npm install azure-cognitiveservices-search-visualSearch`.


[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

<a name="client"></a>

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE veya düzenleyici yeni bir JavaScript dosyası oluşturun ve aşağıdaki koşulları ekleyin. Daha sonra abonelik anahtarınızı, özel yapılandırma kimliği ve dosya yolu karşıya yüklemek istediğiniz görüntüye değişkenleri oluşturun. 

    ```javascript
    const os = require("os");
    const async = require('async');
    const fs = require('fs');
    const Search = require('azure-cognitiveservices-visualsearch');
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    
    let keyVar = 'YOUR-VISUAL-SEARCH-ACCESS-KEY';
    let credentials = new CognitiveServicesCredentials(keyVar);
    let filePath = "../Data/image.jpg";
    ```

2. İstemci örneği oluşturun.

    ```javascript
    let visualSearchClient = new Search.VisualSearchClient(credentials);
    ```

## <a name="search-for-images"></a>Resimler için arama

1. Kullanım `fs.createReadStream()` görüntü dosyanızda okuyup arama isteği ve sonuçlar için değişkenler oluşturun. Daha sonra istemci görüntülerini aramak için kullanın.

    ```javascript
    let fileStream = fs.createReadStream(filePath);
    let visualSearchRequest = JSON.stringify({});
    let visualSearchResults;
    try {
        visualSearchResults = await visualSearchClient.images.visualSearch({
            image: fileStream,
            knowledgeRequest: visualSearchRequest
        });
        console.log("Search visual search request with binary of image");
    } catch (err) {
        console.log("Encountered exception. " + err.message);
    }
    ```

2. Önceki sorgunun sonuçlarını ayrıştırın:

    ```javascript
    // Visual Search results
    if (visualSearchResults.image.imageInsightsToken) {
        console.log(`Uploaded image insights token: ${visualSearchResults.image.imageInsightsToken}`);
    }
    else {
        console.log("Couldn't find image insights token!");
    }
    
    // List of tags
    if (visualSearchResults.tags.length > 0) {
        let firstTagResult = visualSearchResults.tags[0];
        console.log(`Visual search tag count: ${visualSearchResults.tags.length}`);
    
        // List of actions in first tag
        if (firstTagResult.actions.length > 0) {
            let firstActionResult = firstTagResult.actions[0];
            console.log(`First tag action count: ${firstTagResult.actions.length}`);
            console.log(`First tag action type: ${firstActionResult.actionType}`);
        }
        else {
            console.log("Couldn't find tag actions!");
        }
    
    }
    else {
        console.log("Couldn't find image tags!");
    }
    
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfa uygulaması oluşturma](tutorial-bing-visual-search-single-page-app.md)
