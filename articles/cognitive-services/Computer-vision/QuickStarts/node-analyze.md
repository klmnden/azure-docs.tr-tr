---
title: Görüntü İşleme API'si Node.js hızlı başlangıç görüntü analizi | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Bu hızlı başlangıçta, Bilişsel Hizmetler’de Node.js ile Görüntü İşleme kullanarak bir görüntü analiz edeceksiniz.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: dab6547e08b1b01a9090a817d728c86359c680f2
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772613"
---
# <a name="quickstart-analyze-a-remote-image---rest-nodejs"></a>Hızlı Başlangıç: Uzak görüntü analiz etme - REST, Node.js

Bu hızlı başlangıçta, Görüntü İşleme kullanarak görsel özellikleri ayıklamak için bir görüntü analiz edeceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Görüntü İşleme’yi kullanmak için, bir abonelik anahtarınızın olması gerekir; bkz. [Abonelik Anahtarlarını Alma](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="analyze-image-request"></a>Görüntü Analizi isteği

[Görüntü Analizi yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) ile, görüntü içeriğini temel alarak görsel özellikleri ayıklayabilirsiniz. Bir görüntüyü karşıya yükleyebilir veya bir görüntü URL’si belirleyebilir ve aşağıdaki özelliklerden hangilerinin döndürüleceğini seçebilirsiniz:

* Görüntü içeriğiyle ilgili etiketlerin ayrıntılı listesi.
* Görüntü içeriğinin tam bir cümlelik açıklaması.
* Görüntünün içerdiği yüzlerin koordinatları, cinsiyeti ve yaşı.
* ImageType (küçük resim veya çizgi çizim).
* Baskın renk, vurgu rengi veya bir görüntünün siyah beyaz olup olmadığı.
* Bu [sınıflandırmada](../Category-Taxonomy.md) tanımlanan kategori.
* Görüntü, yetişkinlere yönelik veya müstehcen cinsel içerik içeriyor mu?

Örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Aşağıdaki kodu bir düzenleyicinin içine kopyalayın.
1. `<Subscription Key>` değerini geçerli abonelik anahtarınızla değiştirin.
1. Gerekirse `uriBase` değerini abonelik anahtarlarınızı aldığınız konumla değiştirin.
1. İsteğe bağlı olarak, `imageUrl` öğesini analiz etmek istediğiniz görüntü olarak değiştirin.
1. İsteğe bağlı olarak, yanıt dilini değiştirin (`'language': 'en'`).
1. Dosyayı `.js` uzantısıyla kaydedin.
1. Node.js komut istemini açın ve dosyayı çalıştırın, örneğin: `node myfile.js`.

Bu örnek, npm [istek](https://www.npmjs.com/package/request) paketini kullanır.

```nodejs
'use strict';

const request = require('request');

// Replace <Subscription Key> with your valid subscription key.
const subscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to get your
// subscription keys. For example, if you got your subscription keys from
// westus, replace "westcentralus" in the URL below with "westus".
const uriBase =
    'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze';

const imageUrl =
    'http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg';

// Request parameters.
const params = {
    'visualFeatures': 'Categories,Description,Color',
    'details': '',
    'language': 'en'
};

const options = {
    uri: uriBase,
    qs: params,
    body: '{"url": ' + '"' + imageUrl + '"}',
    headers: {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key' : subscriptionKey
    }
};

request.post(options, (error, response, body) => {
  if (error) {
    console.log('Error: ', error);
    return;
  }
  let jsonResponse = JSON.stringify(JSON.parse(body), null, '  ');
  console.log('JSON Response\n');
  console.log(jsonResponse);
});
```

## <a name="analyze-image-response"></a>Görüntü Analizi yanıtı

JSON’da başarılı bir yanıt döndürülür, örneğin:

```json
{
  "categories": [
    {
      "name": "outdoor_water",
      "score": 0.9921875,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "nature",
      "water",
      "waterfall",
      "outdoor",
      "rock",
      "mountain",
      "rocky",
      "grass",
      "hill",
      "covered",
      "hillside",
      "standing",
      "side",
      "group",
      "walking",
      "white",
      "man",
      "large",
      "snow",
      "grazing",
      "forest",
      "slope",
      "herd",
      "river",
      "giraffe",
      "field"
    ],
    "captions": [
      {
        "text": "a large waterfall over a rocky cliff",
        "confidence": 0.916458423253597
      }
    ]
  },
  "color": {
    "dominantColorForeground": "Grey",
    "dominantColorBackground": "Green",
    "dominantColors": [
      "Grey",
      "Green"
    ],
    "accentColor": "4D5E2F",
    "isBwImg": false
  },
  "requestId": "81b4e400-e3c1-41f1-9020-e6871ad9f0ed",
  "metadata": {
    "height": 959,
    "width": 1280,
    "format": "Jpeg"
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Görüntü analiz etmek, ünlüleri ve önemli yerleri algılamak, küçük resim oluşturmak ve yazdırılan ve el yazısı metinleri ayıklamak için kullanılan Görüntü İşleme API'lerini keşfedin. Görüntü İşleme API'lerini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) konusuna göz atın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API'lerini keşfedin](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
