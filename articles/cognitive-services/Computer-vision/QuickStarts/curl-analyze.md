---
title: 'Hızlı Başlangıç: Bir uzak image - REST, cURL analiz edin'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, cURL ile Görüntü İşleme API’si kullanarak bir uzak görüntüyü analiz edeceksiniz.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 2be6625fa2c21c3ece767f270746171c3928a774
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604446"
---
# <a name="quickstart-analyze-a-remote-image-using-the-computer-vision-rest-api-and-curl"></a>Hızlı Başlangıç: CURL ve görüntü işleme REST API'si kullanarak uzak bir resmi çözümleme

Bu hızlı başlangıçta, görüntü işleme'nın REST API kullanarak görsel özellikleri ayıklamak için uzaktan depolanan bir resmi analiz edin. [Görüntü Analizi](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) yöntemi ile, görüntü içeriğini temel alarak görsel özellikleri ayıklayabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- [cURL](https://curl.haxx.se/windows)’niz olmalıdır.
- Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Ücretsiz bir deneme anahtarından alabilirsiniz [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) görüntü işleme için abone ve anahtarınızı alın.

## <a name="create-and-run-the-sample-command"></a>Örnek komutu oluşturma ve çalıştırma

Örneği oluşturup çalıştırmak için aşağıdaki adımları uygulayın:

1. Aşağıdaki komutu bir metin düzenleyicisine kopyalayın.
1. Gerektiğinde komutta aşağıdaki değişiklikleri yapın:
    1. `<subscriptionKey>` değerini abonelik anahtarınızla değiştirin.
    1. Gerekirse istek URL’sini (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze`), abonelik anahtarlarınızı aldığınız Azure bölgesindeki [Görüntü Analizi](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) yönteminin uç nokta URL’si ile değiştirin.
    1. İsteğe bağlı olarak farklı bir desteklenen dil kullanmak için istek URL’sinin dil parametresini (`language=en`) değiştirin.
    1. İsteğe bağlı olarak, istek gövdesindeki görüntü URL’sini (`http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg\`), analiz edilecek farklı bir görüntünün URL’si ile değiştirin.
1. Bir komut istemi penceresi açın.
1. Metin düzenleyicisindeki komutu komut istemi penceresine yapıştırın ve komutu çalıştırın.

```bash
curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks&language=en" -d "{\"url\":\"http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg\"}"
```

## <a name="examine-the-response"></a>Yanıtı inceleme

Başarılı bir yanıt JSON biçiminde döndürülür. Örnek uygulama, aşağıdaki örneğe benzer şekilde başarılı bir yanıtı ayrıştırıp komut istemi penceresinde görüntüler:

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
  "requestId": "b6e33879-abb2-43a0-a96e-02cb5ae0b795",
  "metadata": {
    "height": 959,
    "width": 1280,
    "format": "Jpeg"
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Görüntü analiz etmek, ünlüleri ve yer işaretlerini algılamak, küçük resim oluşturmak ve yazdırılan ve el yazısı metinleri ayıklamak için kullanılan Görüntü İşleme API’sini keşfedin. Görüntü İşleme API'sini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) konusuna bakın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API’sini keşfetme](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
