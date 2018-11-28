---
title: "Hızlı başlangıç: Bir görüntüdeki yüzlerin duygularını tanıma - Duygu Tanıma API'si, Python"
description: Python ile Duygu Tanıma API'sini kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri edinin.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: quickstart
ms.date: 02/05/2018
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: f0bcc88b60e0a9b93856aa32a10b9c0ad898ce95
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50240707"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Hızlı başlangıç: Bir görüntüdeki yüzlerin duygularını tanımak için bir uygulama oluşturma.

> [!IMPORTANT]
> Duygu Tanıma API'si 15 Şubat 2019 tarihinde kullanım dışı bırakılacaktır. Duygu tanıma özelliği [Yüz Tanıma API'sinin](https://docs.microsoft.com/azure/cognitive-services/face/) bir parçası olarak genel kullanıma sunulmuştur. 

Bu kılavuz bir görüntüdeki bir veya daha fazla kişinin duygularını tanımak için Python ile [Duygu Tanıma API'si Recognize metodunu](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri bulunmaktadır.

Bu örneği başlatma Bağlayıcı rozetine tıklayarak [Bağlayıcım](https://mybinder.org)'da bir Jupyter not defteri olarak çalıştırabilirsiniz: [![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=EmotionAPI.ipynb)


## <a name="prerequisite"></a>Önkoşul
[Buradan](https://azure.microsoft.com/try/cognitive-services/) ücretsiz Abonelik Anahtarınızı alın

## <a name="running-the-walkthrough"></a>Adımları çalıştırma
Bu kılavuza devam etmek için `subscription_key` yerine önceden aldığınız API anahtarını girin.


```python
subscription_key = None
assert subscription_key
```

Ardından hizmet URL'si bölgesinin API anahtarını ayarlarken kullandığınızla aynı olduğundan emin olun. Deneme anahtarı kullanıyorsanız değişiklik yapmanıza gerek yoktur.


```python
emotion_recognition_url = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
```

Bu kılavuzda diskte bulunan görüntüler kullanılmaktadır. URL ile erişilebilen genel görüntüleri de kullanabilirsiniz. Daha fazla bilgi için bkz: [REST API belgeleri](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa).

Görüntü verileri istek gövdesinde geçirildiği için `Content-Type` üst bilgisinin `application/octet-stream` olarak ayarlanması gerektiğine dikkat edin. Görüntüyü URL ile geçiriyorsanız üst bilgisini şu şekilde ayarlamanız gerekir:
```python
header = {'Ocp-Apim-Subscription-Key': subscription_key }
```
URL'yi içeren bir sözlük oluşturun:
```python
data = {'url': image_url}
```
ve bunu şu şekilde `requests` kitaplığına geçirin:
```python
requests.post(emotion_recognition_url, headers=headers, json=image_data)
```

İlk olarak [Duygu Tanıma API'si](https://azure.microsoft.com/services/cognitive-services/emotion/) sitesinden birkaç örnek görüntü indirin.


```bash
%%bash
mkdir -p images
curl -Ls https://aka.ms/csnb-emotion-1 -o images/emotion_1.jpg
curl -Ls https://aka.ms/csnb-emotion-2 -o images/emotion_2.jpg
```


```python
image_path = "images/emotion_1.jpg"
image_data = open(image_path, "rb").read()
```


```python
import requests
headers  = {'Ocp-Apim-Subscription-Key': subscription_key, "Content-Type": "application/octet-stream" }
response = requests.post(emotion_recognition_url, headers=headers, data=image_data)
response.raise_for_status()
analysis = response.json()
analysis
```




    [{'faceRectangle': {'height': 162, 'left': 130, 'top': 141, 'width': 162},
      'scores': {'anger': 9.29041e-06,
       'contempt': 0.000118981574,
       'disgust': 3.15619363e-05,
       'fear': 0.000589638,
       'happiness': 0.06630674,
       'neutral': 0.00555004273,
       'sadness': 7.44669524e-06,
       'surprise': 0.9273863}}]



Döndürülen JSON nesnesinde tanınan yüzlerin sınırlayıcı kutularıyla birlikte algılanan duygular yer alır. Her duyguya 0 ile 1 arasında bir puan verilir ve yüksek puanlar, düşük puanlara göre daha yoğun bir duyguyu belirtir.

Aşağıdaki kod satırları `matplotlib` kitaplığını kullanarak görüntüdeki yüzlerin duygularını alır. Karmaşıklığı azaltmak için yalnızca ilk üç duygu gösterilmektedir.


```python
%matplotlib inline
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle
from io import BytesIO
from PIL import Image

plt.figure(figsize=(5,5))

image  = Image.open(BytesIO(image_data))
ax     = plt.imshow(image, alpha=0.6)

for face in analysis:
    fr = face["faceRectangle"]
    em = face["scores"]
    origin = (fr["left"], fr["top"])
    p = Rectangle(origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
    ax.axes.add_patch(p)
    ct = "\n".join(["{0:<10s}{1:>.4f}".format(k,v) for k, v in sorted(list(em.items()),key=lambda r: r[1], reverse=True)][:3])
    plt.text(origin[0], origin[1], ct, fontsize=20)    
_ = plt.axis("off")
```

Bir sonraki adımda gösterilen `annotate_image` işlevi, duyguları dosya sistemindeki yolu verilen bir görüntü dosyasının üzerine yerleştirebilir. Daha önce gösterilen Duygu Tanıma API'si çağırma kodunu temel alır.


```python
def annotate_image(image_path):    
    image_data = open(image_path, "rb").read()
    headers  = {'Ocp-Apim-Subscription-Key': subscription_key, "Content-Type": "application/octet-stream" }
    response = requests.post(emotion_recognition_url, headers=headers, data=image_data)
    response.raise_for_status()
    analysis = response.json()

    plt.figure()

    image  = Image.open(image_path)
    ax     = plt.imshow(image, alpha=0.6)

    for face in analysis:
        fr = face["faceRectangle"]
        em = face["scores"]
        origin = (fr["left"], fr["top"])
        p = Rectangle(origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
        ax.axes.add_patch(p)
        ct = "\n".join(["{0:<10s}{1:>.4f}".format(k,v) for k, v in sorted(list(em.items()),key=lambda r: r[1], reverse=True)][:3])
        plt.text(origin[0], origin[1], ct, fontsize=20, va="bottom")    
    _ = plt.axis("off")
```

Son olarak `annotate_image` işlevi aşağıdaki satırda gösterilen şekilde görüntü dosyasına çağrılabilir:


```python
annotate_image("images/emotion_2.jpg")
```
