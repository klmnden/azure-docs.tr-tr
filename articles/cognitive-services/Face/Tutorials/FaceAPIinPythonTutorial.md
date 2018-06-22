---
title: Yüz API Python Eğitmen | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Bilişsel hizmetler görüntüdeki İnsan yüzeyleri algılamak için Python SDK'sı ile yüz API kullanmayı öğrenin.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 408f9759262d6ae2193df3193ee1c306d384afe6
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36308797"
---
# <a name="getting-started-with-face-api-in-python-tutorial"></a>Python öğreticide API yüzeyi ile çalışmaya başlama

Bu öğreticide, bir resim İnsan yüzeyleri algılamak için Python SDK'sı üzerinden yüz API çağırma öğreneceksiniz.

## <a name="prerequisites"></a> Önkoşullar

Öğretici kullanmak için aşağıdakileri yapmanız gerekir:

- Python 2.7 + veya Python 3.5 + yükleyin.
- PIP yükleyin.
- Python SDK'sı için yüz API gibi yükleyin:

```bash
pip install cognitive_face
```

- Elde bir [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/) Microsoft Bilişsel hizmetler için. Bu öğreticide, birincil veya ikincil anahtarınızı kullanabilirsiniz. (Herhangi bir yazıtipi API'yi kullanmak için geçerli bir abonelik anahtarı olması gerektiğini unutmayın.)

## <a name="sdk-example"></a> Görüntüyü bir yazıtipi Algıla

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

Aşağıda bir örnek sonucudur. Bu bir `list` algılanan yazıtipleri. Her öğe listede bir `dict` örneğinde `faceId` algılanan yüz için benzersiz bir kimliği ve `faceRectangle` algılanan yüz konumunu açıklar. Bir nominal kimliği 24 saat içinde süresi dolar.

```python
[{u'faceId': u'68a0f8cf-9dba-4a25-afb3-f9cdf57cca51', u'faceRectangle': {u'width': 89, u'top': 66, u'height': 89, u'left': 446}}]
```

## <a name="draw-rectangles-around-the-faces"></a>Yazıtipleri dikdörtgenler çizme

Önceki komutu alınan json koordinatları kullanarak, her yüz görsel olarak sunmak üzere görüntüde dikdörtgenler çizme. Etmeniz `pip install Pillow` kullanmak için `PIL` görüntüleme modülü.  Dosyanın üst kısmında, aşağıdakileri ekleyin:

```python
import requests
from io import BytesIO
from PIL import Image, ImageDraw
```

Ardından, sonra `print(faces)`, komut dosyanıza şunları içerir:

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

## <a name='further'></a> Daha fazla araştırması

Daha fazla yüz API keşfetmenize yardımcı olmak için Bu öğretici bir GUI örneğini sağlar. Çalıştırmak için ilk yükleme [wxPython](https://wxpython.org/) ardından aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Microsoft/Cognitive-Face-Python.git
cd Cognitive-Face-Python
python sample
```

## <a name="summary"></a> Özet

Bu öğreticide, Python SDK'sı çağrılırken aracılığıyla yüz API kullanma için temel işlem öğrendiniz. Nasıl yapılır konuları için API ayrıntıları hakkında daha fazla bilgi için lütfen bakın ve [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="related"></a> İlgili Konular

- [CSharp karşılaştıkları API'si ile çalışmaya başlama](FaceAPIinCSharpTutorial.md)
- [Android için yüz Java API ile Başlarken](FaceAPIinJavaForAndroidTutorial.md)
