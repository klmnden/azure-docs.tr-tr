---
title: Görüntü İşleme Python hızlı başlangıç yerel görüntü analiz etme | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Bu hızlı başlangıçta, Bilişsel Hizmetler’de Python ile Görüntü İşleme kullanarak bir yerel görüntü analiz edeceksiniz.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: a1f3fce5a547f143f7c4884c6642e78f53d160e9
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772577"
---
# <a name="quickstart-analyze-a-local-image---rest-python"></a>Hızlı Başlangıç: Yerel görüntü analiz etme - REST, Python

Bu hızlı başlangıçta, Görüntü İşleme kullanarak bir yerel görüntü analiz edeceksiniz. Uzak bir görüntüyü analiz etmek için, bkz. [Python ile uzak bir görüntüyü analiz etme](python-analyze.md).

[MyBinder](https://mybinder.org) üzerinde bir Jupyter not defteri kullanarak bu hızlı başlangıcı adım adım görüntülenecek şekilde çalıştırabilirsiniz. Bağlayıcıyı başlatmak için aşağıdaki düğmeyi seçin:

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="prerequisites"></a>Ön koşullar

Görüntü İşleme’yi kullanmak için, bir abonelik anahtarınızın olması gerekir; bkz. [Abonelik Anahtarlarını Alma](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="analyze-a-local-image"></a>Yerel resmi çözümleme

Bu örnek [Python ile uzak görüntüyü analiz etmeye](python-analyze.md) benzer ancak analiz edilecek görüntü diskten yerel olarak okunur. İki değişiklik gereklidir:

- İsteğe bir `{"Content-Type": "application/octet-stream"}` üst bilgisi ekleme.
- İstek gövdesine görüntü verisi (bayt dizisi) ekleme.

Örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Aşağıdaki kodu yeni bir Python betik dosyasına kopyalayın.
1. `<Subscription Key>` değerini geçerli abonelik anahtarınızla değiştirin.
1. Gerekirse `vision_base_url` değerini abonelik anahtarlarınızı aldığınız konumla değiştirin.
1. `image_path` değerini yerel görüntünün yoluyla değiştirin.
1. Betiği çalıştırın.

Aşağıdaki kod, Görüntü İşleme Görüntü Analizi API’sine çağrı yapmak için Python `requests` kitaplığını kullanır. Sonuçları JSON nesnesi olarak döndürür. API anahtarı `headers` sözlüğü aracılığıyla geçirilir. Tanınacak özelliklerin türleri `params` sözlüğü aracılığıyla geçirilir. İkili görüntü verileri `data` parametresiyle `requests.post` öğesine geçirilir.

## <a name="analyze-image-request"></a>Görüntü Analizi isteği

```python
import requests
# If you are using a Jupyter notebook, uncomment the following line.
#%matplotlib inline
import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

# Replace <Subscription Key> with your valid subscription key.
subscription_key = "<Subscription Key>"
assert subscription_key

# You must use the same region in your REST call as you used to get your
# subscription keys. For example, if you got your subscription keys from
# westus, replace "westcentralus" in the URI below with "westus".
#
# Free trial subscription keys are generated in the westcentralus region.
# If you use a free trial subscription key, you shouldn't need to change
# this region.
vision_base_url = "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/"

analyze_url = vision_base_url + "analyze"

# Set image_path to the local path of an image that you want to analyze.
image_path = "C:/Documents/ImageToAnalyze.jpg"

# Read the image into a byte array
image_data = open(image_path, "rb").read()
headers    = {'Ocp-Apim-Subscription-Key': subscription_key,
              'Content-Type': 'application/octet-stream'}
params     = {'visualFeatures': 'Categories,Description,Color'}
response = requests.post(
    analyze_url, headers=headers, params=params, data=image_data)
response.raise_for_status()

# The 'analysis' object contains various fields that describe the image. The most
# relevant caption for the image is obtained from the 'description' property.
analysis = response.json()
print(analysis)
image_caption = analysis["description"]["captions"][0]["text"].capitalize()

# Display the image and overlay it with the caption.
image = Image.open(BytesIO(image_data))
plt.imshow(image)
plt.axis("off")
_ = plt.title(image_caption, size="x-large", y=-0.1)
```

## <a name="analyze-image-response"></a>Görüntü Analizi yanıtı

Başarılı bir yanıt JSON biçiminde döndürülür, örneğin:

```json
{
  "categories": [
    {
      "name": "outdoor_",
      "score": 0.00390625,
      "detail": {
        "landmarks": []
      }
    },
    {
      "name": "outdoor_street",
      "score": 0.33984375,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "building",
      "outdoor",
      "street",
      "city",
      "people",
      "busy",
      "table",
      "walking",
      "traffic",
      "filled",
      "large",
      "many",
      "group",
      "night",
      "light",
      "crowded",
      "bunch",
      "standing",
      "man",
      "sign",
      "crowd",
      "umbrella",
      "riding",
      "tall",
      "woman",
      "bus"
    ],
    "captions": [
      {
        "text": "a group of people on a city street at night",
        "confidence": 0.9122243847383961
      }
    ]
  },
  "color": {
    "dominantColorForeground": "Brown",
    "dominantColorBackground": "Brown",
    "dominantColors": [
      "Brown"
    ],
    "accentColor": "B54316",
    "isBwImg": false
  },
  "requestId": "c11894eb-de3e-451b-9257-7c8b168073d1",
  "metadata": {
    "height": 600,
    "width": 450,
    "format": "Jpeg"
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Optik karakter tanıma (OCR) gerçekleştirmek için Görüntü İşleme kullanan bir Python uygulaması keşfedin. Akıllı kırpılmış küçük resimler oluşturun. Buna ek olarak, bir görüntüdeki yüzler gibi görsel özellikleri algılayın, kategorilere ayırın, etiketleyin ve açıklayın. Görüntü İşleme API'lerini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) konusuna göz atın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API'si Python Öğreticisi](../Tutorials/PythonTutorial.md)
