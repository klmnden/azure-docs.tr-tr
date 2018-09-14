---
title: "Hızlı Başlangıç: Node.js ve Bing resim arama SDK'sını kullanarak görüntüleri arayın"
description: Bu hızlı başlangıçta, istek ve Node.js kullanarak Bing resim arama tarafından döndürülen görüntüleri Filtrele.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: cagronlund
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: ebe6629344c076119c0bfdaee76a69df6274ca28
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45572592"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-nodejs"></a>Hızlı Başlangıç: Node.js ve Bing resim arama SDK'sı ile görüntüleri arayın

Bing görüntü arama API'si için bir sarmalayıcı olan ve aynı özellikleri içeren SDK'yı kullanarak ilk görüntü arama yapmak için bu Hızlı Başlangıç'ı kullanın. Bu basit bir JavaScript uygulama bir resim arama sorgusu gönderir, JSON yanıtı ayrıştırır ve döndürülen ilk görüntünün URL'sini görüntüler.

Bu örnek için kaynak kodu kullanılabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js) ek hata işleme ve ek açıklamalar.

## <a name="prerequisites"></a>Önkoşullar

* [Bilişsel hizmetler görüntü Node.js SDK'sı arayın](https://www.npmjs.com/package/azure-cognitiveservices-imagesearch)
    * Kullanarak yükleme `npm install azure-cognitiveservices-imagesearch`
* [Node.js Azure Rest](https://www.npmjs.com/package/ms-rest-azure) Modülü
    * Kullanarak yükleme `npm install ms-rest-azure`

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Oluşturma ve uygulama başlatma

1. Sık kullandığınız IDE veya düzenleyici yeni bir JavaScript dosyası oluşturun ve katılık, https ve diğer gereksinimler ayarlayın.

    ```javascript
    'use strict';
    https = require('https');
    const Search = require('azure-cognitiveservices-imagesearch');
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    ```

2. Projenizin ana yöntemde, Bing ve bir arama terimi döndürülecek görüntü sonuçları değişkenleri için geçerli bir abonelik anahtarınızı oluşturun. Ardından anahtarı kullanılarak görüntü arama istemci örneği.

    ```javascript
    //replace this value with your valid subscription key.
    let serviceKey = "ENTER YOUR KEY HERE";

    //the search term for the request 
    let searchTerm = "canadian rockies";

    //instantiate the the image search client 
    let credentials = new CognitiveServicesCredentials(serviceKey);
    let imageSearchApiClient = new Search.ImageSearchAPIClient(credentials);

    ```

## <a name="create-an-asynchronous-helper-function"></a>Zaman uyumsuz yardımcı işlev oluşturma

1. İstemci zaman uyumsuz olarak çağırmak için fonksiyon oluşturun ve Bing resim arama hizmetinden bir yanıt döndürür.  
    ```javascript
    //a helper function to perform an async call to the Bing Image Search API
    const sendQuery = async () => {
        return await imageSearchApiClient.imagesOperations.search(searchTerm);
    };
    ```
## <a name="send-a-query-and-handle-the-response"></a>Bir sorgu göndermek ve yanıtın işlemek 

1. Tanıtıcı ve yardımcı işlevini çağırın, `promise` yanıtta döndürülen resim sonuçları ayrıştırılamıyor.

    Yanıt arama sonuçları içeriyorsa, ilk sonucu depolar ve bir küçük resim gibi ayrıntılarını yazdırmak URL, toplam sayısını özgün URL'yi döndürülen görüntüler.  
    ```javascript
    sendQuery().then(imageResults => {
        if (imageResults == null) {
        console.log("No image results were found.");
        }
        else {
            console.log(`Total number of images returned: ${imageResults.value.length}`);
            let firstImageResult = imageResults.value[0];
            //display the details for the first image result. After running the application,
            //you can copy the resulting URLs from the console into your browser to view the image. 
            console.log(`Total number of images found: ${imageResults.value.length}`);
            console.log(`Copy these URLs to view the first image returned:`);
            console.log(`First image thumbnail url: ${firstImageResult.thumbnailUrl}`);
            console.log(`First image content url: ${firstImageResult.contentUrl}`); 
        }
      })
      .catch(err => console.error(err))
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing resim arama tek sayfalı uygulama Öğreticisi](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing resim arama nedir?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Çevrimiçi bir etkileşimli Tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Ücretsiz bir Bilişsel hizmetler erişim anahtarını alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api) 
* [Azure Bilişsel hizmetler SDK'sı için node.js Örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
* [Azure Bilişsel hizmetler belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing resim arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)