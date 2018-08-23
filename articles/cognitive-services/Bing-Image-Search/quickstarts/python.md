---
title: "Hızlı Başlangıç: Python, Bing resim arama API'si için REST API kullanarak gönderme arama sorguları"
description: Bu hızlı başlangıçta Python kullanarak ilgili görüntülerin listesini almak için Bing arama API'si arama sorguları gönderin.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: bc527ba39b580935f113f56aa63f7bdd283ba304
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "41987991"
---
# <a name="quickstart-send-search-queries-using-the-rest-api-and-python"></a>Hızlı Başlangıç: Python ve REST API kullanarak gönderme arama sorguları

Bing resim arama API'si, Bing için bir kullanıcı arama sorgusu gönderin ve ilgili görüntülerin listesini dönmek vererek Bing.com/Images için benzer bir deneyim sağlar.

Bu yönerge, Bing resim arama API'si çağırmak ve sonuçta elde edilen JSON nesnesi sonrası işlem basit bir örnek gösterir. Daha fazla bilgi için [Bing resim arama belgeleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).

Bu örnek bir Jupyter not defteri çalıştırma [MyBinder](https://mybinder.org) bağlayıcı başlatma sırasında tıklayarak rozet: 

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="running-the-walkthrough"></a>İzlenecek yol çalıştırma
Adım adım kılavuza devam etmek için ayarlama `subscription_key` için Bing API'si hizmeti için API anahtarınızı.


```python
subscription_key = None
assert subscription_key
```

Ardından, doğrulayın `search_url` doğru uç noktadır. Bu yazma sırasında Bing arama API'leri için tek bir uç nokta kullanılır. Yetkilendirme hatalarla karşılaşırsanız, bu değer Azure Panonuzda Bing arama uç nokta karşı denetleyin.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/images/search"
```

Ayarlama `search_term` Yavru köpekler görüntülerde aramak için.


```python
search_term = "puppies"
```

Aşağıdaki blokları kullandığı `requests` Bing arama API'lerine duyurmak ve sonuçları bir JSON nesnesi olarak döndürmek için Python kitaplıkta. Biz API anahtarı geçmesini gözlemleyin `headers` sözlük ve bir arama terimi aracılığıyla `params` sözlüğü. Arama sonuçlarını filtrelemek için kullanılabilir seçeneklerin tam listesini görmek için başvurmak [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) belgeleri.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "license": "public", "imageType": "photo"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

`search_results` Nesne zengin meta veriler gibi ilgili öğeleri yanı sıra gerçek görüntüleri içerir. Örneğin, aşağıdaki kod satırını ilk 16 sonuçları için küçük resim URL'LERİNİ ayıklayabilirsiniz.


```python
thumbnail_urls = [img["thumbnailUrl"] for img in search_results["value"][:16]]
```

Ardından, kullanabiliriz `PIL` masaüstünün indirmek için kitaplık ve `matplotlib` 4 ABD Doları \times 4$ Kılavuzu'nun işlenecek kitaplığı.


```python
%matplotlib inline
import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

f, axes = plt.subplots(4, 4)
for i in range(4):
    for j in range(4):
        image_data = requests.get(thumbnail_urls[i+4*j])
        image_data.raise_for_status()
        image = Image.open(BytesIO(image_data.content))        
        axes[i][j].imshow(image)
        axes[i][j].axis("off")
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing resim arama tek sayfalı uygulama Öğreticisi](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Bing resim arama genel bakış](../overview.md)  
[Deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
[Ücretsiz deneme erişim anahtarını alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
[Bing resim arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
