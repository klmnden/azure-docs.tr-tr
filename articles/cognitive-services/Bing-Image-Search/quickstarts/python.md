---
title: Çağrı ve yanıt - Azure Bilişsel Hizmetleri, Bing görüntü arama API'sı için Python hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get Bing görüntü arama API Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki kullanmaya başlayın.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: 3b5d6a961ce4bcde8aaf73f1fbd30689a6c2c2d1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352348"
---
# <a name="call-and-response-your-first-bing-image-search-query-in-python"></a>Çağrı ve yanıt: Python, ilk Bing görüntü arama sorgusu

Bing görüntü arama API geri ilgili görüntüleri listesini almak ve bir kullanıcı arama sorgusu için Bing göndermenize izin vererek Bing.com/Images için benzer bir deneyim sağlar.

Bu anlatımda Bing görüntü arama API'yi çağıran ve sonuçta elde edilen JSON nesnesi sonrası işlem basit bir örnek gösterilir. Daha fazla bilgi için bkz: [Bing görüntü arama belgelerine](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).

Bu örneği Jupyter not defteri çalıştırmak [MyBinder](https://mybinder.org) başlatılırken bağlayıcı tıklayarak göstergeye: 

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)

## <a name="prerequisites"></a>Önkoşullar

Bilmeniz gereken bir [Bilişsel Hizmetleri API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Bing arama API'leri**. [Ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) Bu Hızlı Başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirmek ya da Ücretli abonelik anahtarı Azure panonuza kullanabilir sağlanan erişim anahtarı gerekir.

## <a name="running-the-walkthrough"></a>İzlenecek yol çalıştırma
İzlenecek yol ile devam etmek için ayarlama `subscription_key` API anahtarınıza Bing API'si hizmeti için.


```python
subscription_key = None
assert subscription_key
```

Ardından, doğrulayın `search_url` doğru uç noktadır. Bu yazma sırasında tek bir uç nokta Bing arama API'leri için kullanılır. Yetkilendirme hatalarla karşılaşırsanız, bu değer Azure panonuza Bing arama uç karşı denetleyin.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/images/search"
```

Ayarlama `search_term` Yavru köpekler görüntülerde aramak için.


```python
search_term = "puppies"
```

Aşağıdaki kullanımları engellemek `requests` Bing arama API'leri duyurmak ve sonuçları bir JSON nesnesi olarak dönmek için Python kitaplığında. Biz API anahtarında geçirmek gözlemlemek `headers` sözlük ve arama terimi aracılığıyla `params` sözlük. Arama sonuçlarını filtrelemek için kullanılabilir seçenekleri tam listesini görmek için bkz [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) belgeleri.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "license": "public", "imageType": "photo"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

`search_results` Nesnesini içeren ilgili öğeler gibi zengin meta verilerin yanı sıra fiili görüntüler. Örneğin, aşağıdaki kod satırını ilk 16 sonuçları için küçük resim URL'leri ayıklayabilirsiniz.


```python
thumbnail_urls = [img["thumbnailUrl"] for img in search_results["value"][:16]]
```

Ardından, biz kullanabilirsiniz `PIL` küçük resmini indirmek için kitaplık ve `matplotlib` bir $4 \times 4$ kılavuz işlenecek kitaplığı.


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
> [Bing görüntü arama tek sayfa uygulaması Öğreticisi](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Bing görüntü arama genel bakış](../overview.md)  
[Deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
[Ücretsiz deneme erişim anahtarı alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
[Bing görüntü arama API Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
