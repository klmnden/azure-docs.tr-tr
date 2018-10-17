---
title: "Öğretici: Bir görüntüdeki yüzleri algılama ve çerçeveleme - Yüz Tanıma API'si, Python"
titleSuffix: Azure Cognitive Services
description: Bir görüntüdeki insan yüzlerini algılamak için Python SDK’sı ile Yüz Tanıma API’sinin nasıl kullanılacağını öğrenin.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 6cc3ac25d2196c0275b445503b79b9ac06a791d3
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46127746"
---
# <a name="tutorial-detect-and-frame-faces-with-the-face-api-and-python"></a>Öğretici: Yüz Tanıma API’si ve Python ile yüzleri algılama ve çerçeveleme 

Bu öğreticide, bir görüntüdeki insan yüzlerini algılamak için Python SDK’sı aracılığıyla Yüz Tanıma API’sinin nasıl çağrılacağını öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Öğreticiyi kullanmak için aşağıdakileri yapmanız gerekir:

- Python 2.7+ veya Python 3.5+ yükleyin.
- Pip yükleyin.
- Aşağıdaki adımları izleyerek Yüz Tanıma API’si için Python SDK’sını yükleyin:

```bash
pip install cognitive_face
```

- Microsoft Bilişsel Hizmetler için bir [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/) alın. Bu öğreticide, birincil veya ikincil anahtarınızı kullanabilirsiniz. (Herhangi bir Yüz Tanıma API’sini kullanmak için geçerli bir abonelik anahtarınız olması gerektiğini unutmayın.)

## <a name="detect-a-face-in-an-image"></a>Bir Görüntüdeki Yüzü algılama

```python
import cognitive_face as CF

KEY = '<Subscription Key>'  # Replace with a valid subscription key (keeping the quotes in place).
CF.Key.set(KEY)

BASE_URL = 'https://westus.api.cognitive.microsoft.com/face/v1.0/'  # Replace with your regional Base URL
CF.BaseUrl.set(BASE_URL)

# You can use this example JPG or replace the URL below with your own URL to a JPEG image.
img_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
faces = CF.face.detect(img_url)
print(faces)
```

Aşağıda örnek bir sonuç verilmiştir. Bu, algılanan yüzlerin `list` öğesidir. Listedeki her bir öğe `dict` örneği olup burada `faceId`, algılanan yüzün benzersiz kimliğidir ve `faceRectangle` ise, algılanan yüzün konumunu açıklar. 24 saat içinde yüz kimliğinin süresi dolar.

```python
[{u'faceId': u'68a0f8cf-9dba-4a25-afb3-f9cdf57cca51', u'faceRectangle': {u'width': 89, u'top': 66, u'height': 89, u'left': 446}}]
```

## <a name="draw-rectangles-around-the-faces"></a>Yüzlerin çevresine dikdörtgen çizme

Önceki komuttan aldığınız json koordinatlarını kullanarak, her bir yüzü görsel olarak temsil etmek için görüntüye dikdörtgenler çizebilirsiniz. `PIL` görüntüleme modülünü kullanmak için `pip install Pillow` işlemini yapmanız gerekir.  Dosyanın en üstüne aşağıdakileri ekleyin:

```python
import requests
from io import BytesIO
from PIL import Image, ImageDraw
```

Ardından `print(faces)` sonrasında betiğinize aşağıdakileri ekleyin:

```python
#Convert width height to a point in a rectangle
def getRectangle(faceDictionary):
    rect = faceDictionary['faceRectangle']
    left = rect['left']
    top = rect['top']
    bottom = left + rect['height']
    right = top + rect['width']
    return ((left, top), (bottom, right))

#Download the image from the url
response = requests.get(img_url)
img = Image.open(BytesIO(response.content))

#For each face returned use the face rectangle and draw a red box.
draw = ImageDraw.Draw(img)
for face in faces:
    draw.rectangle(getRectangle(face), outline='red')

#Display the image in the users default image browser.
img.show()
```

## <a name="further-exploration"></a>Daha Fazla Keşif

Yüz Tanıma API’sini daha fazla keşfetmenize yardımcı olması için bu öğreticide bir GUI örneği verilmiştir. Bunu çalıştırmak için önce [wxPython](https://wxpython.org/pages/downloads/) öğesini, sonra da aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Microsoft/Cognitive-Face-Python.git
cd Cognitive-Face-Python
python sample
```

## <a name="summary"></a>Özet

Bu öğreticide, Python SDK’sı çağrısı aracılığıyla Yüz Tanıma API’sini kullanmaya ilişkin temel işlemi öğrendiniz. API ayrıntıları hakkında daha fazla bilgi için lütfen Nasıl Yapılır ve [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)’na bakın.

## <a name="related-topics"></a>İlgili Konular

- [CSharp’ta Yüz Tanıma API’sini Kullanmaya Başlama](FaceAPIinCSharpTutorial.md)
- [Android için Java’da Yüz Tanıma API’sini Kullanmaya Başlama](FaceAPIinJavaForAndroidTutorial.md)
