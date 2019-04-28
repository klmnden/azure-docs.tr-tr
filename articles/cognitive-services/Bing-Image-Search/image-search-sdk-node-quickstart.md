---
title: "Hızlı Başlangıç: Görüntüleri - Bing resim arama SDK'sı için Node.js arayın"
description: Bu öğreticiyi API için bir sarmalayıcı olan ve aynı özellikleri içeren Bing Resim Arama SDK'sını kullanarak ilk görüntü aramanızı yapmak için kullanın. Bu basit Node.js uygulaması, bir görüntü arama sorgusu gönderir, JSON yanıtını ayrıştırır ve döndürülen ilk görüntünün URL'sini görüntüler.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 03/04/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 641247a0ddc08f15fa70006e280426fda083241f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60917599"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-for-nodejs"></a>Hızlı Başlangıç: Node.js için Bing resim arama SDK'sı ile görüntüleri için arama

Bu öğreticiyi API için bir sarmalayıcı olan ve aynı özellikleri içeren Bing Resim Arama SDK'sını kullanarak ilk görüntü aramanızı yapmak için kullanın. Bu basit JavaScript uygulaması bir görüntü arama sorgusu gönderir, JSON yanıtını ayrıştırır ve döndürülen ilk görüntünün URL'sini görüntüler.

Bu örneğin kaynak kodu, ek hata işleme ve açıklama notları ile [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js)'da bulunabilir.

## <a name="prerequisites"></a>Önkoşullar
**Arama** altından bir [Bilişsel Hizmetler erişim anahtarı](https://azure.microsoft.com/try/cognitive-services/) alın.  Ayrıca bkz: [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).  Ayrıca bkz: [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

* [Node.js için Bilişsel Hizmetler Görüntü Arama SDK'sı](https://www.npmjs.com/package/azure-cognitiveservices-imagesearch)
    * `npm install azure-cognitiveservices-imagesearch` kullanarak yükleme
* [Node.js Azure Rest](https://www.npmjs.com/package/ms-rest-azure) modülü
    * `npm install ms-rest-azure` kullanarak yükleme

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE'de veya düzenleyicide yeni bir JavaScript dosyası oluşturun ve katılık, https ve diğer gereksinimleri ayarlayın.

    ```javascript
    'use strict';
    const Search = require('azure-cognitiveservices-imagesearch');
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    ```

2. Projenizin main yönteminde geçerli abonelik anahtarınız, Bing tarafından döndürülecek görüntü sonuçları ve bir arama terimi için değişkenler oluşturun. Ardından anahtarı kullanılarak görüntü arama istemcisinin bir örneğini oluşturun.

    ```javascript
    //replace this value with your valid subscription key.
    let serviceKey = "ENTER YOUR KEY HERE";

    //the search term for the request
    let searchTerm = "canadian rockies";

    //instantiate the image search client 
    let credentials = new CognitiveServicesCredentials(serviceKey);
    let imageSearchApiClient = new Search.ImageSearchAPIClient(credentials);

    ```

## <a name="create-an-asynchronous-helper-function"></a>Zaman uyumsuz yardımcı işlev oluşturma

1. İstemciyi zaman uyumsuz olarak çağırmak için bir işlev oluşturun ve Bing Görüntü Arama hizmetinin yanıtını döndürün.  
    ```javascript
    //a helper function to perform an async call to the Bing Image Search API
    const sendQuery = async () => {
        return await imageSearchApiClient.imagesOperations.search(searchTerm);
    };
    ```
   ## <a name="send-a-query-and-handle-the-response"></a>Sorgu göndermek ve yanıtı işlemek

1. Yardımcı işlevi çağırın ve yanıtta döndürülen görüntü sonuçlarını ayrıştırmak için `promise` değerini işleyin.

    Yanıt arama sonuçları içeriyorsa, ilk sonucu depolayın ve döndürülen toplam görüntü sayısının yanı sıra bu ilk sonucun küçük resim URL'si, asıl URL gibi ayrıntılarını yazdırın.  
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
> [Bing Resim Arama tek sayfalı uygulama öğreticisi](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Ayrıca bkz.

* [Bing Resim Arama nedir?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Çevrimiçi etkileşimli bir tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Ücretsiz bir Bilişsel Hizmetler erişim anahtarı alın](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)
* [Azure Bilişsel Hizmetler SDK'sı için Node.js örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
* [Azure Bilişsel Hizmetler Belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing Resim Arama API’si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
