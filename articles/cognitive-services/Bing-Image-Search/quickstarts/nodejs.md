---
title: "Hızlı Başlangıç: Node.js kullanarak Bing resim arama API'si arama sorgularıyla Gönder"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, arama ve Bing Web araması API'si kullanarak web üzerinde görüntüleri bulmak için kullanın.
services: cognitive-services
documentationcenter: ''
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 8/20/2018
ms.author: aahi
ms.openlocfilehash: 43f0cfec6aa2d4263b6a627736c2a432b2943145
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576655"
---
# <a name="quickstart-send-search-queries-using-the-bing-image-search-rest-api-and-nodejs"></a>Hızlı Başlangıç: Node.js ve Bing resim arama REST API'si kullanarak gönderme arama sorguları

Bu hızlı başlangıçta, bir JSON yanıtı alırsınız ve Bing resim arama API'si, ilk çağrı yapmak için kullanın. Bu basit bir JavaScript uygulama API için bir arama sorgusu gönderir ve ham sonuçları görüntüler.

Bu uygulamanın, JavaScript'te yazılmış ve Node.js API çalışır bir RESTful Web çoğu programlama dilinden uyumlu hizmet.

Bu örnek için kaynak kodu kullanılabilir [github'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingImageSearchv7Quickstart.js) ek hata işleme ve kod ek açıklamaları.

## <a name="prerequisites"></a>Önkoşullar

* En son sürümünü [Node.js](https://nodejs.org/en/download/).

* [JavaScript isteği kitaplığı](https://github.com/request/request)
[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Oluşturma ve uygulama başlatma

1. Sık kullandığınız IDE veya düzenleyici yeni bir JavaScript dosyası oluşturun ve katılık ve https gereksinimlerini ayarlayın.

    ```javascript
    'use strict';
    let https = require('https');
    ```

2. Değişkenleri API uç noktası için görüntü API'si arama yolu, abonelik anahtarınızı oluşturma ve arama terimi.
    ```javascript
    let subscriptionKey = 'enter key here';
    let host = 'api.cognitive.microsoft.com';
    let path = '/bing/v7.0/images/search';
    let term = 'tropical ocean';
    ```

## <a name="construct-the-search-request-and-query"></a>Sorgu ve arama isteği oluşturun.

1. Son adım değişkenlerinden API isteği için bir arama URL'si biçimlendirmek için kullanın. Arama teriminizi API'ye gönderilen önce URL olarak kodlanmış olması gerektiğini unutmayın.

    ```javascript
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path + '?q=' + encodeURIComponent(search),
        headers : {
        'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };
    ```

2. API için sorgu göndermek için isteği Kitaplığı'nı kullanın. `response_handler` sonraki bölümde tanımlanır.
    ```javascript
    let req = https.request(request_params, response_handler);
    req.end();
    ```

## <a name="handle-and-parse-the-response"></a>İşlemek ve yanıtı ayrıştırılamadı

1. adlı bir fonksiyon tanımlayın `response_handler` HTTP çağrısı, alan `response`, parametre olarak. Bu işlevin içinde aşağıdaki adımları gerçekleştirin:
    
    1. JSON yanıt gövdesini içeren bir değişkeni tanımlar.  
        ```javascript
        let response_handler = function (response) {
            let body = '';
        };
        ```

    2. Yanıt gövdesinin Store olduğunda **veri** bayrağı çağrılır 
        ```javascript
        response.on('data', function (d) {
            body += d;
        });
        ```

    3. Olduğunda bir **son** bayrak işareti, JSON işlenebilir ve resmin URL'si yazdırılabilir döndürülen görüntüleri yanı sıra toplam sayısı.
    
        ```javascript
        response.on('end', function () {
            let firstImageResult = imageResults.value[0];
            console.log(`Image result count: ${imageResults.value.length}`);
            console.log(`First image thumbnail url: ${firstImageResult.thumbnailUrl}`);
            console.log(`First image web search url: ${firstImageResult.webSearchUrl}`);
         });
        ```

## <a name="json-response"></a>JSON yanıtı

Bing resim arama API'si alınan yanıtları JSON olarak döndürülür. Bu örnek yanıt, tek bir sonuç göstermek için kısaltıldı.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing resim arama tek sayfalı uygulama Öğreticisi](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing resim arama nedir?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Çevrimiçi bir etkileşimli Tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Ücretsiz bir Bilişsel hizmetler erişim anahtarını alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Azure Bilişsel hizmetler belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing resim arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)