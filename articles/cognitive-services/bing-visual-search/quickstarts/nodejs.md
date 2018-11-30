---
title: 'Hızlı Başlangıç: Görsel arama sorgusu oluşturma, Node.js - Bing Görsel Arama'
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'sine görüntü yükleme ve görüntü hakkında içgörü alma.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 553d068d70f7e722f3c8e4de3978f3583b941963
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52442545"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-javascript"></a>Hızlı Başlangıç: JavaScript ile ilk Bing Görsel Arama sorgunuz

Bing Görsel Arama API'si, verdiğiniz bir görüntü hakkında bilgi döndürür. Bir URL veya bir içgörü belirteci kullanarak ya da karşıya resim yükleyerek görüntüyü verebilirsiniz. Bu seçenekler hakkında bilgi için bkz. [Bing Görsel Arama API’si nedir?](../overview.md) Bu makalede, karşıya görüntü yükleme gösterilmektedir. Karşıya resim yüklemek, mobil bir cihazla tanınmış bir yerin resmini çekip bu yer hakkında bilgi almak istediğiniz bir durumda kullanışlı olabilir. Örneğin, içgörüler bu yer hakkındaki önemsiz küçük ayrıntıları içerebilir. 

Aşağıda, yerel bir görüntüyü karşıya yükleyeceğiniz zaman POST’un gövdesine dahil etmeniz gereken form verileri gösterilmektedir. Form verileri, Content-Disposition üst bilgisini içermelidir. `name` parametresi, "image" olarak, `filename` parametresi ise herhangi bir dize olarak ayarlanmalıdır. Formun içerikleri, görüntünün ikili verisidir. Karşıya yükleyebileceğiniz maksimum görüntü boyutu 1 MB’tır. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Bu makale, bir Bing Görsel Arama API'si isteği gönderen ve JSON arama sonuçlarını görüntüleyen basit bir konsol uygulamasını içermektedir. Bu uygulama JavaScript ile yazılmış olmakla birlikte API HTTP istekleri gönderebilen ve JSON ayrıştırabilen her programlama diliyle uyumlu bir RESTful Web hizmetidir. 

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıçta bir abonelik S9 fiyat katmanı gösterildiği gibi başlatmanız gerekecek [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/search-api/). 

Azure portalında bir abonelik başlatmak için:
1. 'BingSearchV7' ifadesini içeren Azure portalının üst metin kutusuna girin `Search resources, services, and docs`.  
2. Aşağı açılan listesinde Marketi bölümünde seçin `Bing Search v7`.
3. Girin `Name` yeni kaynak için.
4. Seçin `Pay-As-You-Go` abonelik.
5. Seçin `S9` fiyatlandırma katmanı.
6. Tıklayın `Enable` abonelik başlatmak için.

Bu kodu çalıştırmak için [Node.js 6](https://nodejs.org/en/download/) gerekir.

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
6. `imagePath` değerini, karşıya yüklenecek görüntünün yoluyla değiştirin.
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

[İçgörü belirteci kullanarak bir görüntü ile ilgili içgörüler elde edin](../use-insights-token.md)  
[Bing Görsel Arama görüntü karşıya yükleme öğreticisi](../tutorial-visual-search-image-upload.md)
[Bing Görsel Arama tek sayfalı uygulama öğreticisi](../tutorial-bing-visual-search-single-page-app.md)  
[Bing Görsel Arama’ya genel bakış](../overview.md)  
[Deneyin](https://aka.ms/bingvisualsearchtryforfree)  
[Ücretsiz deneme erişim anahtarı alın](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Görsel Arama API’si başvurusu](https://aka.ms/bingvisualsearchreferencedoc)
