---
title: "Hızlı Başlangıç: Resimler - Bing resim arama REST API'si ve Node.js için arama yapın"
titleSuffix: Azure Cognitive Services
description: JSON yanıtlar almasına ve JavaScript kullanarak Bing resim arama REST API'si için görüntü arama istekleri göndermek için bu Hızlı Başlangıç'ı kullanın.
services: cognitive-services
documentationcenter: ''
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 02/06/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: f3b174fa00c6f91c4e4840f2abcb14f95451d7f5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60914202"
---
# <a name="quickstart-search-for-images-using-the-bing-image-search-rest-api-and-nodejs"></a>Hızlı Başlangıç: Node.js ve Bing resim arama REST API'si kullanarak resimler için arama yapın

Bu hızlı başlangıçta, Bing resim arama API'si için arama istekleri gönderirken başlatmak için kullanın. JavaScript uygulaması bu API için bir arama sorgusu gönderir ve ilk görüntünün URL'sini sonuçları görüntüler. Bu uygulamanın, JavaScript'te yazılmış olsa da çoğu programlama dilleri ile uyumlu bir RESTful web hizmeti API'dir.

Bu örneğin kaynak kodu, ek hata işleme ve açıklama notları ile [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingImageSearchv7Quickstart.js)’da bulunabilir.

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/download/)’in en son sürümü.

* [JavaScript isteği kitaplığı](https://github.com/request/request)  
  [!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

Ayrıca bkz: [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE’de veya düzenleyicide yeni bir JavaScript dosyası oluşturun ve katılık ve https gereksinimlerini ayarlayın.

    ```javascript
    'use strict';
    let https = require('https');
    ```

2. API uç noktası, görüntü API’si arama yolu, abonelik anahtarınız ve arama teriminiz için değişkenler oluşturun.
    ```javascript
    let subscriptionKey = 'enter key here';
    let host = 'api.cognitive.microsoft.com';
    let path = '/bing/v7.0/images/search';
    let term = 'tropical ocean';
    ```

## <a name="construct-the-search-request-and-query"></a>Arama isteği ve sorgu oluşturun.

1. API isteğine yönelik bir arama URL’sini biçimlendirmek için son adımdaki değişkenleri kullanın. Arama API'sine gönderilmeden önce URL kodlamalı olmalıdır.

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

2. API’ye sorgunuzu göndermek için istek kitaplığını kullanın. `response_handler`, sonraki bölümde tanımlanır.
    ```javascript
    let req = https.request(request_params, response_handler);
    req.end();
    ```

## <a name="handle-and-parse-the-response"></a>Yanıtı işleme ve ayrıştırma

1. HTTP çağrısı (`response`) alan `response_handler` adlı bir işlevi parametre olarak tanımlayın. Bu işlev içinde aşağıdaki adımları uygulayın:

    1. JSON yanıtının gövdesini içerecek bir değişken tanımlayın.  
        ```javascript
        let response_handler = function (response) {
            let body = '';
        };
        ```

    2. **Veri** işareti çağrıldığında yanıtın gövdesini depolama
        ```javascript
        response.on('data', function (d) {
            body += d;
        });
        ```

    3. Olduğunda bir **son** bayrağı sinyal, JSON yanıtı ilk sonucu alın. Döndürülen görüntüleri toplam sayısı ile birlikte ilk görüntünün URL'sini yazdırın.

        ```javascript
        response.on('end', function () {
            let firstImageResult = imageResults.value[0];
            console.log(`Image result count: ${imageResults.value.length}`);
            console.log(`First image thumbnail url: ${firstImageResult.thumbnailUrl}`);
            console.log(`First image web search url: ${firstImageResult.webSearchUrl}`);
         });
        ```

## <a name="example-json-response"></a>Örnek JSON yanıtı

Bing Resim Arama API'sinden yanıtlar JSON olarak döndürülür. Bu örnek yanıt, tek bir sonuç göstermek için kısaltıldı.

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
    }]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir tek sayfalı uygulama oluşturma](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Bing Resim Arama nedir?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Çevrimiçi etkileşimli bir tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) 
* [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/) Bing arama API'leri. 
* [Ücretsiz bir Bilişsel Hizmetler erişim anahtarı alın](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Azure Bilişsel Hizmetler Belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing Resim Arama API’si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
