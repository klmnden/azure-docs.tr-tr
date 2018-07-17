---
title: Bing görsel arama API'si için JavaScript hızlı başlangıç | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Bing görsel arama API'sine bir görüntüyü karşıya yükleme ve görüntü ile ilgili Öngörüler geri alma işlemi gösterilmektedir.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 60b1dc9b8ea9eda258e9776b8967df38c97d964e
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071712"
---
# <a name="your-first-bing-visual-search-query-in-javascript"></a>İlk Bing görsel arama sorgunuzda JavaScript

Bing görsel arama API'sine sağlayan bir görüntü ile ilgili bilgi döndürür. Belirteç, ya da bir görüntü yükleyerek bir ınsights görüntünün URL'sini kullanarak görüntü sağlayabilir. Bu seçenekler hakkında daha fazla bilgi için bkz: [Bing görsel arama API'si nedir?](../overview.md) Bu makalede, bir resim karşıya gösterilmektedir. Görüntüyü karşıya yükleme, iyi bilinen bir yer işareti resmini Al ve geri hakkında bilgi alın mobil senaryolarda yararlı olabilir. Örneğin, öngörüleri, yer işareti hakkında bilgi dahil olabilir. 

Yerel bir görüntüyü karşıya yükleme, aşağıdaki form verilerini POST gövdesinde içermelidir gösterir. Form verileri içerik düzeni üstbilgisini içermelidir. Kendi `name` parametresi ayarlanması gerekir "Görüntü" ve `filename` parametreyi bir dizeye ayarlayın. Form içeriğini ikili görüntünün olur. Karşıya yükleyebilirsiniz en yüksek görüntü boyutu 1 MB'dir. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Bu makale, Bing görsel arama API'sine bir istek gönderir ve JSON arama sonuçlarını görüntüleyen basit bir konsol uygulaması içerir. Bu uygulamanın, JavaScript'te yazılmış olsa da, HTTP istekleri ve JSON Ayrıştır programlama dili ile uyumlu bir RESTful Web hizmeti API'dir. 

## <a name="prerequisites"></a>Önkoşullar

Gereksinim duyduğunuz [Node.js 6](https://nodejs.org/en/download/) bu kodu çalıştırmak için.

Bu hızlı başlangıçta, kullanabileceğiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonelik anahtarı veya Ücretli abonelik anahtarı.

## <a name="running-the-application"></a>Uygulamayı çalıştırma

Node.js'de çıkışlardan form verisi kullanarak ileti gönderme işlemini gösterir.

Bu uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. Projeniz için bir klasör oluşturun (veya sık kullandığınız IDE veya düzenleyici kullanın).
2. Bir komut istemi veya terminal, az önce oluşturduğunuz klasöre gidin.
3. İstek modüllerini yükleyin:  
  ```  
  npm install request  
  ```  
3. Form verileri modüllerini yükleyin:  
  ```  
  npm install form-data  
  ```  
4. GetVisualInsights.js adlı bir dosya oluşturun ve aşağıdaki kodu ekleyin.
5. Değiştirin `subscriptionKey` abonelik anahtarınız ile değeri.
6. Değiştirin `imagePath` görüntünün karşıya yükleme yolunu içeren değer.
7. Programı çalıştırın.  
  ```
  node GetVisualInsights.js
  ```

```javascript
var request = require('request');
var FormData = require('form-data');
var fs = require('fs');

var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
var subscriptionKey = '<yoursubscriptionkeygoeshere>';
var imagePath = "<pathtoyourimagegoeshere>";

var form = new FormData();
form.append("image", fs.createReadStream(imagePath));

form.getLength(function(err, length){
  if (err) {
    return requestCallback(err);
  }

  var r = request.post(baseUri, requestCallback);
  r._form = form; 
  r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
});

function requestCallback(err, res, body) {
    console.log(JSON.stringify(JSON.parse(body), null, '  '))
}
```


## <a name="next-steps"></a>Sonraki adımlar

[Insights belirteci kullanarak bir görüntü ile ilgili Öngörüler elde edin](../use-insights-token.md)  
[Bing görsel arama görüntüsünü karşıya yükleme Öğreticisi](../tutorial-visual-search-image-upload.md)
[Bing görsel arama tek sayfalı uygulama Öğreticisi](../tutorial-bing-visual-search-single-page-app.md)  
[Bing görsel arama genel bakış](../overview.md)  
[Deneyin](https://aka.ms/bingvisualsearchtryforfree)  
[Ücretsiz deneme erişim anahtarını alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing görsel arama API'si başvurusu](https://aka.ms/bingvisualsearchreferencedoc)
