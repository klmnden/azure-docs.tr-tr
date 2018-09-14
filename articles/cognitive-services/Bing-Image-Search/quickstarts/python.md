---
title: "Hızlı Başlangıç: Python ve Bing resim arama API'si arama sorgular göndererek"
description: Bu hızlı başlangıçta Python kullanarak görüntülerin listesini almak için Bing arama API'si arama sorguları gönderin.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 8/20/2018
ms.author: aahi
ms.openlocfilehash: 42b9af2d6c7223dc3f5d269dd2003f1c99d8c7da
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45578336"
---
# <a name="quickstart-send-search-queries-using-the-rest-api-and-python"></a>Hızlı Başlangıç: Python ve REST API kullanarak gönderme arama sorguları

Bu hızlı başlangıçta, bir JSON yanıtı alırsınız ve Bing resim arama API'si, ilk çağrı yapmak için kullanın. Bu basit bir Python uygulaması, API için bir arama sorgusu gönderir ve ham sonuçları görüntüler.

Bu uygulama Python'da yazılmıştır, ancak çoğu programlama dilleri ile uyumlu bir RESTful Web hizmeti API'dir.

Bu örnek bir Jupyter not defteri çalıştırma [MyBinder](https://mybinder.org) bağlayıcı başlatma sırasında tıklayarak rozet: 

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)


Ayrıca, bu örnek için kaynak kodu kullanılabilir [github'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingImageSearchv7.py).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="running-the-quickstart"></a>Hızlı Başlangıç'ı çalıştırma

Başlamak için ayarlanmış `subscription_key` için Bing API'si hizmeti için geçerli bir abonelik anahtarı.

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

Ardından `PIL` masaüstünün indirmek için kitaplık ve `matplotlib` 4 ABD Doları \times 4$ Kılavuzu'nun işlenecek kitaplığı.


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
plt.show()
```

## <a name="sample-json-response"></a>Örnek JSON yanıtı

Bing resim arama API'si alınan yanıtları JSON olarak döndürülür. Bu örnek yanıt, tek bir sonuç göstermek için kısaltıldı.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing resim arama tek sayfalı uygulama Öğreticisi](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing resim arama nedir?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Çevrimiçi bir etkileşimli Tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Ücretsiz bir Bilişsel hizmetler erişim anahtarını alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Azure Bilişsel hizmetler belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing resim arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)