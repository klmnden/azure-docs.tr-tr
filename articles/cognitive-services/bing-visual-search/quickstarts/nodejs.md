---
title: "Hızlı Başlangıç: Bing görsel arama REST API'si ve Node.js kullanarak görüntü Öngörüler elde edin"
titleSuffix: Azure Cognitive Services
description: Bing görsel arama API'sine bir görüntüyü karşıya yükleme ve ilgili Öngörüler alma hakkında bilgi edinin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 4/02/2019
ms.author: scottwhi
ms.openlocfilehash: 9414bac220d928618b403aa2f7df7748772e0e9a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60832617"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>Hızlı Başlangıç: Bing görsel arama REST API'si ve Node.js kullanarak görüntü Öngörüler elde edin

Bu hızlı başlangıçta, ilk Bing görsel arama API'sine çağrı yapmak ve arama sonuçlarını görüntülemek için kullanın. Bu basit bir JavaScript uygulama API için bir görüntüyü karşıya yükler ve bu konuda döndürülen bilgileri görüntüler. Bu uygulamanın, JavaScript'te yazılmış olsa da çoğu programlama dilleri ile uyumlu bir RESTful Web hizmeti API'dir.

Form verileri bir yerel görüntü karşıya yüklenirken içermelidir `Content-Disposition` başlığı. Ayarlamanız gerekir, `name` "image" parametresini ve `filename` herhangi bir dize parametresi ayarlanabilir. Form içeriğini görüntünün ikili verileri içerir. Karşıya yükleyebileceğiniz maksimum görüntü boyutu 1 MB’tır.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/download/)
* JavaScript için istek modülü. Kullanabileceğiniz `npm install request` modülü yüklemek için komutu.
* Form verileri modülü. Kullanabileceğiniz `npm install form-data` modülü yüklemek için komutu. 

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Uygulamayı Başlat

1. Sık kullandığınız IDE veya düzenleyici bir JavaScript dosyası oluşturun ve aşağıdaki gereksinimleri ayarlayın:

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. API uç noktanız abonelik anahtarı ve görüntü yolu için değişkenler oluşturun:

    ```javascript
    var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. Adlı bir işlev oluşturma `requestCallback()` API'den yanıt'ı yazdırmak için:

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## <a name="construct-and-send-the-search-request"></a>Oluşturun ve arama isteği gönder

1. Yeni bir **çıkışlardan form verisi** kullanarak nesne `FormData()`ve, görüntü yolu kullanarak, ekleme `fs.createReadStream()`:
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

2. İsteği kitaplığı, görüntüyü yüklemek için kullanın ve çağrı `requestCallback()` yanıt'ı yazdırmak için. İstek üstbilgisi için abonelik anahtarınızı eklediğinizden emin olun:

    ```javascript
    form.getLength(function(err, length){
      if (err) {
        return requestCallback(err);
      }
      var r = request.post(baseUri, requestCallback);
      r._form = form; 
      r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
    });
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir görsel arama tek sayfa web uygulaması derleme](../tutorial-bing-visual-search-single-page-app.md)
