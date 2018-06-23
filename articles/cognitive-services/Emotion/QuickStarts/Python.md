---
title: Duygu tanıma API'si Python hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get duygu tanıma API'si Bilişsel hizmetler Python ile kullanmaya başlayın.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/05/2018
ms.author: anroth
ms.openlocfilehash: ff1f6b2ddc872d0ee63d9885b04b1f007bc86e33
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351629"
---
# <a name="emotion-api-python-quickstart"></a>Duygu tanıma API'si Python hızlı başlangıç

> [!IMPORTANT]
> Video API Önizleme 30 Ekim 2017 sona erdi. Yeni deneyin [Video dizin oluşturucu API önizlemesi](https://azure.microsoft.com/services/cognitive-services/video-indexer/) kolayca videoların öngörüleri ayıklamak ve konuşulan sözcüklerin, yüzler, karakterler ve duygular algılayarak arama sonuçları gibi içerik bulma deneyimlerini geliştirmek üzere. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Bu kılavuz bilgiler sağlar ve hızlı bir şekilde yardımcı olmak için kod örnekleri, kullanımına başlamanıza [duygu tanıma API'si tanıması yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) görüntünün bir veya daha fazla kişiler tarafından ifade duygular tanımak için Python ile. 

Bu örneği Jupyter not defteri çalıştırmak [MyBinder](https://mybinder.org) başlatılırken bağlayıcı tıklayarak göstergeye: [ ![bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=EmotionAPI.ipynb)


## <a name="prerequisite"></a>Önkoşul
Ücretsiz abonelik anahtarınızı alın [burada](https://azure.microsoft.com/try/cognitive-services/)

## <a name="running-the-walkthrough"></a>İzlenecek yol çalıştırma
Bu kılavuzda ile devam etmek için değiştirmek `subscription_key` daha önce aldığınız API anahtarı ile.


```python
subscription_key = None
assert subscription_key
```

Ardından, hizmet URL'si API anahtarını kullanılan bölge karşılık geldiğinden emin olun. Bir deneme anahtarı kullanıyorsanız, herhangi bir değişiklik yapmanız gerekmez.


```python
emotion_recognition_url = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
```

Bu kılavuzda diskte depolanan görüntüleri kullanır. Genel olarak erişilebilir bir URL kullanılabilir olan görüntüleri de kullanabilirsiniz. Daha fazla bilgi için bkz: [REST API belgeleri](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa).

Görüntü verilerini istek gövdesi bir parçası olarak geçirilir beri ayarlamak gerektiğini unutmayın `Content-Type` başlığına `application/octet-stream`. Bir görüntü URL aracılığıyla geçirilirse, üstbilgi kümesine unutmayın:
```python
header = {'Ocp-Apim-Subscription-Key': subscription_key }
```
URL'sini içeren bir sözlük oluşturun:
```python
data = {'url': image_url}
```
ve için geçirin `requests` kitaplığını kullanarak:
```python
requests.post(emotion_recognition_url, headers=headers, json=image_data)
```

İlk birkaç örnek görüntülerden indirin [duygu tanıma API'si](https://azure.microsoft.com/services/cognitive-services/emotion/) site.


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



Döndürülen JSON nesnesi ile birlikte algılanan duygular tanındı yazıtipleri sınırlayıcı kutularını içerir. Her duygu daha yüksek bir puan daha düşük bir puan bir duygu daha göstergesi olduğu 0 ile 1 arasında bir puan ilişkilendirilir. 

Aşağıdaki satırları kod görüntü kullanarak yüzeyleri üzerinde algılanan duygular `matplotlib` kitaplığı. Görünmesine için yalnızca ilk üç duygular gösterilmektedir.


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

`annotate_image` Sonraki gösterilen işlevi, belirtilen kendi yol dosya sisteminde bir görüntü dosyası üstünde duygular kaplamak için kullanılabilir. Daha önce gösterilen duygu API'de arama kodunu dayanır.


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

Son olarak, `annotate_image` işlevi aşağıdaki satırda gösterildiği gibi bir görüntü dosyasının çağrılamaz:


```python
annotate_image("images/emotion_2.jpg")
```
