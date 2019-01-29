---
title: "Hızlı Başlangıç: Bing görsel arama REST API'si ve Node.js kullanarak görüntü Öngörüler elde edin"
titleSuffix: Azure Cognitive Services
description: Bing görsel arama API'sine bir görüntüyü karşıya yükleme ve ilgili Öngörüler alma hakkında bilgi edinin.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 23c3e167f2fd1544a42c98946f4ae95d3451dafe
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180628"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>Hızlı Başlangıç: Bing görsel arama REST API'si ve Node.js kullanarak görüntü Öngörüler elde edin

Bu hızlı başlangıçta, ilk Bing görsel arama API'sine çağrı yapmak ve arama sonuçlarını görüntülemek için kullanın. Bu basit bir JavaScript uygulama API için bir görüntüyü karşıya yükler ve bu konuda döndürülen bilgileri görüntüler. Bu uygulamanın, JavaScript'te yazılmış olsa da çoğu programlama dilleri ile uyumlu bir RESTful Web hizmeti API'dir.

Form verileri, yerel bir görüntüyü karşıya yüklenirken içerik düzeni üstbilgisini içermesi gerekir. `name` parametresi, "image" olarak, `filename` parametresi ise herhangi bir dize olarak ayarlanmalıdır. Formun içerikleri, görüntünün ikili verisidir. Karşıya yükleyebileceğiniz maksimum görüntü boyutu 1 MB’tır.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/download/)
* JavaScript için istek Modülü
    * Bu modül kullanarak yükleyebilirsiniz. `npm install request`
* Form verileri Modülü
    * Bu modül kullanarak yükleyebilirsiniz. `npm install form-data`


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]


## <a name="initialize-the-application"></a>Uygulamayı Başlat

1. Sık kullandığınız IDE veya düzenleyici yeni bir JavaScript dosyası oluşturun ve aşağıdaki gereksinimleri ayarlayın:

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. API uç noktanız abonelik anahtarı ve görüntü yolu için değişkenler oluşturun.

    ```javascript
    var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. Çağrılan bir işlev oluşturma `requestCallback()` API'den yanıt'ı yazdırmak için.

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## <a name="construct-and-send-the-search-request"></a>Oluşturun ve arama isteği gönder

1. Kullanarak yeni bir form verilerini oluşturmak `FormData()`ve, görüntü yolu kullanarak, ekleme `fs.createReadStream()`.
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

2. Görüntü yüklemek için istek kitaplığını kullanacak çağırma `requestCallback()` yanıt'ı yazdırmak için. İstek üstbilgisi için abonelik anahtarınızı eklediğinizden emin olun. 

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
> [Bir özel arama web uygulaması derleme](../tutorial-bing-visual-search-single-page-app.md)
