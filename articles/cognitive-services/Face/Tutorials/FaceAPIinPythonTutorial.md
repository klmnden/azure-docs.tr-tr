---
title: "Hızlı Başlangıç: Algılama ve Python SDK'sı ile bir görüntüdeki yüzleri çerçeve"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, algılamak ve uzak bir görüntüdeki yüzleri çerçeve için yüz tanıma API'sini kullanan basit bir Python betiğini oluşturur.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: sbowles
ms.openlocfilehash: e4b762d6f36f8682162160be6f42b8691e4b2ca3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60815250"
---
# <a name="quickstart-create-a-python-script-to-detect-and-frame-faces-in-an-image"></a>Hızlı Başlangıç: Algılamak için bir Python betiği oluşturmak ve çerçevenin bir resimdeki yüz

Bu hızlı başlangıçta, uzak bir görüntüde İnsan yüzlerini algılamak için Python SDK'sı aracılığıyla Azure yüz tanıma API'sini kullanan basit bir Python betiğini oluşturur. Uygulama, seçilen görüntü görüntüler ve algılanan her yüz etrafında bir çerçeve çizer.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

- Yüz tanıma API'si abonelik anahtarı. Ücretsiz deneme aboneliği anahtarından alabilirsiniz [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yüz tanıma API'si hizmete abone ve anahtarınızı alın.
- [Python 2.7+ veya 3.5+](https://www.python.org/downloads/)
- [pip](https://pip.pypa.io/en/stable/installing/) aracı

## <a name="get-the-face-sdk"></a>Yüz tanıma SDK'sı Al

Yüz tanıma Python SDK'sı için komut istemini açın ve aşağıdaki komutu çalıştırarak yükleyin:

```shell
pip install cognitive_face
```

## <a name="detect-faces-in-an-image"></a>Görüntüdeki yüzleri algılayın

Adlı yeni bir Python betiği oluşturmak _FaceQuickstart.py_ ve aşağıdaki kodu ekleyin. Yüz algılama temel işlevlerini budur. Değiştirmeniz gerekecektir `<Subscription Key>` anahtarınızı değerine sahip. Değerini değiştirmeniz gerekebilir `BASE_URL` doğru bölge tanımlayıcısı için anahtarınızı kullanmak için (bkz [yüz tanıma API'si belgeleri](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) tüm bölge uç noktalar listesi). Ücretsiz deneme aboneliği anahtarları oluşturulur **westus** bölge. İsteğe bağlı olarak, `img_url` kullanmak istediğiniz herhangi bir görüntü URL'si.

Betik çağırarak yüzleri algılar **cognitive_face.face.detect** sarmalar yöntemi [Algıla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) REST API'si ve yüz listesini döndürür.

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

### <a name="try-the-app"></a>Uygulamayı deneyin

Komutuyla uygulamayı çalıştırın `python FaceQuickstart.py`. Konsol penceresinde, aşağıdaki gibi bir metin yanıtı almanız gerekir:

```shell
[{'faceId': '26d8face-9714-4f3e-bfa1-f19a7a7aa240', 'faceRectangle': {'top': 124, 'left': 459, 'width': 227, 'height': 227}}]
```

Algılanan yüzeylere listesidir. Listedeki her bir öğe bir **dict** örneğinde `faceId` algılanan yüz için benzersiz bir kimliği ve `faceRectangle` algılanan yüz konumu açıklar. 

> [!NOTE]
> Yüz tanıma kimlikleri 24 saat sonra süresi dolacak; uzun süreli saklamak istiyorsanız açıkça yüz verileri depolamak gerekir.

## <a name="draw-face-rectangles"></a>Yüz tanıma dikdörtgenler çizme

Önceki komutta aldığınız koordinatlar kullanarak, her yüzü görsel olarak sunmak üzere görüntünün dikdörtgen çizebilirsiniz. Pillow yüklemeniz gerekir (`pip install pillow`) Pillow görüntü modülü kullanmak için. Üst kısmındaki *FaceQuickstart.py*, aşağıdaki kodu ekleyin:

```python
import requests
from io import BytesIO
from PIL import Image, ImageDraw
```

Sonra kodunuzu sonunda, aşağıdaki kodu ekleyin. Bu durum, dikdörtgen koordinatları ayrıştırmak için basit bir işlev oluşturur ve orijinal görüntünün dikdörtgen çizmek için Pillow kullanır. Sonra görüntüyü varsayılan Resim Görüntüleyicisi'nde görüntüler.

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

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Varsayılan Image viewer seçmeniz istenebilir. Ardından, aşağıdaki gibi bir görüntü görmeniz gerekir. Ayrıca, konsol penceresinde yazdırılır dikdörtgen veri görmeniz gerekir.

![Yüz tanıma geçici olarak çizilen kırmızı bir dikdörtgen genç bir kadın](../images/face-rectangle-result.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yüz tanıma API'si Python SDK'ları kullanmak için temel işlemi hakkında bilgi edindiniz ve algılayın ve bir görüntüdeki yüzleri çerçeve için bir komut dosyası oluşturuldu. Ardından, daha karmaşık bir örnek Python SDK'ın kullanımını keşfedin. Github'da Bilişsel yüz tanıma Python örnek için proje klasörünüze kopyalayın ve yönergeleri _README.md_ dosya.

> [!div class="nextstepaction"]
> [Bilişsel yüz tanıma Python örneği](https://github.com/Microsoft/Cognitive-Face-Python)
