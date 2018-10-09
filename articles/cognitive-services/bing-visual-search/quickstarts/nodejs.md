---
title: 'Hızlı Başlangıç: Görsel arama sorgusu oluşturma, Node.js - Bing Görsel Arama'
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'sine görüntü yükleme ve görüntü hakkında içgörü alma.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: b13738c5bfd8fc75224bf934ae8be56e7c2edd69
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225506"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-javascript"></a>Hızlı Başlangıç: JavaScript ile ilk Bing Görsel Arama sorgunuz

Bing Görsel Arama API'si, verdiğiniz bir görüntü hakkında bilgi döndürür. Görüntüyü, bir URL veya bir içgörü belirteci kullanarak veya karşıya resim yükleyerek verebilirsiniz. Bu seçenekler hakkında bilgi için bkz. [Bing Görsel Arama API'si nedir?](../overview.md) Bu makale karşıya görüntü yüklemeyi göstermektedir. Karşıya resim yüklemek, mobil bir cihazla tanınmış bir yerin resmini çekip bu yer hakkında bilgi almak istediğiniz bir durumda kullanışlı olabilir. Örneğin içgörüler bu yer hakkındaki önemsiz küçük ayrıntıları içerebilir. 

Aşağıda yerel bir görüntüyü karşıya yükleyeceğiniz zaman POST'un gövdesine dahil etmeniz gereken form verileri gösterilmektedir. Form verileri Content-Disposition üstbilgisini içermelidir. `name` parametresi "image" olarak, `filename` parametresi ise herhangi bir dize olarak ayarlanmalıdır. Formun içeriği görüntünün ikili verisidir. Karşıya yükleyebileceğiniz görüntünün en büyük boyutu 1 MB'dir. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Bu makale, bir Bing Görsel Arama API'si isteği gönderen ve JSON arama sonuçlarını görüntüleyen basit bir konsol uygulamasını içermektedir. Bu uygulama JavaScript ile yazılmış olmakla birlikte API HTTP istekleri gönderebilen ve JSON ayrıştırabilen her programlama diliyle uyumlu bir RESTful Web hizmetidir. 

## <a name="prerequisites"></a>Ön koşullar

Bu kodu çalıştırmak için [Node.js 6](https://nodejs.org/en/download/) gerekir.

Bu hızlı başlangıçta bir [ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonelik anahtarı veya ücretli abonelik anahtarı kullanabilirsiniz.

## <a name="running-the-application"></a>Uygulamayı çalıştırma

Aşağıda Node.js'te FormData kullanılarak ileti gönderme işlemi gösterilmektedir.

Bu uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. Projeniz için bir klasör oluşturun (veya sık kullandığınız IDE'yi veya düzenleyiciyi kullanın).
2. Bir komut isteminden veya terminalden az önce oluşturduğunuz klasöre gidin.
3. İstek modüllerini yükleyin:  
  ```  
  npm install request  
  ```  
3. Form verisi modüllerini yükleyin:  
  ```  
  npm install form-data  
  ```  
4. GetVisualInsights.js adlı bir dosya oluşturun ve içine aşağıdaki kodu ekleyin.
5. `subscriptionKey` değerini abonelik anahtarınızla değiştirin.
6. `imagePath` değerini karşıya yüklenecek görüntünün yolu ile değiştirin.
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

[Bir içgörü belirteci kullanarak bir görüntü ile ilgili içgörüler elde edin](../use-insights-token.md)  
[Bing Görsel Arama görüntü yükleme öğreticisi](../tutorial-visual-search-image-upload.md)
[Bing Görsel Arama tek sayfalı uygulama öğreticisi](../tutorial-bing-visual-search-single-page-app.md)  
[Bing Görsel Arama'ya genel bakış](../overview.md)  
[Deneyin](https://aka.ms/bingvisualsearchtryforfree)  
[Ücretsiz deneme erişim anahtarı alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Görsel Arama API'si başvurusu](https://aka.ms/bingvisualsearchreferencedoc)
