---
title: "Hızlı Başlangıç: Resimler - Bing resim arama REST API'si ve Python için arama yapın"
titleSuffix: Azure Cognitive Services
description: JSON yanıtlar almasına ve bu hızlı başlangıçta Python kullanarak Bing resim arama REST API'si için görüntü arama istekleri göndermek için kullanın.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: quickstart
ms.date: 8/20/2018
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: a93a044279cccd883de5f946bb236cad4b088ae2
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53261986"
---
# <a name="quickstart-search-for-images-using-the-bing-image-search-rest-api-and-python"></a>Hızlı Başlangıç: Bing resim arama REST API'si ve Python kullanarak resimler için arama yapın

İlk Bing Resim Arama API’si çağrınızı yapmak ve bir JSON yanıtı almak için bu hızlı başlangıcı kullanın. Bu basit Python uygulaması, API’ye bir arama sorgusu gönderir ve ham sonuçları görüntüler.

Bu uygulama Python ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir.

Bu örneği başlatma Bağlayıcı rozetine tıklayarak [Bağlayıcım](https://mybinder.org)’da bir Jupyter not defteri olarak çalıştırabilirsiniz:

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)


Ek olarak bu örneğin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingImageSearchv7.py)’da mevcuttur.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

Ayrıca bkz: [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="running-the-quickstart"></a>Hızlı başlangıcı çalıştırma

Başlamak için `subscription_key` öğesini, Bing API hizmeti için geçerli bir abonelik anahtarına ayarlayın.

```python
subscription_key = None
assert subscription_key
```

Ardından `search_url` uç noktasının doğru olduğundan emin olun. Bu yazının yazıldığı tarihte Bing arama API’leri için tek bir uç nokta kullanılıyordu. Yetkilendirme hatalarıyla karşılaşırsanız bu değeri Azure panonuzdaki Bing arama uç noktasından tekrar kontrol edin.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/images/search"
```

Yavru köpek görüntüleri aramak için `search_term` öğesini ayarlayın.


```python
search_term = "puppies"
```

Aşağıdaki blok, Bing Arama API’lerini çağırmak ve sonuçları bir JSON nesnesi olarak döndürmek için Python’da `requests` kitaplığını kullanmaktadır. API anahtarını `headers` sözlüğü ile geçtiğimize ve terimi `params` sözlüğü ile aradığımıza dikkat edin. Arama sonuçlarını filtrelemek için kullanılabilecek seçeneklerin tam listesi için [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) belgelerine başvurun.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "license": "public", "imageType": "photo"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

`search_results` nesnesi, ilgili öğeler gibi zengin meta verilerle birlikte gerçek görüntüleri içerir. Örneğin, aşağıdaki kod satırı, ilk 16 sonuç için küçük resim URL’lerini ayıklayabilir.


```python
thumbnail_urls = [img["thumbnailUrl"] for img in search_results["value"][:16]]
```

Daha sonra küçük resim görüntülerini indirmek için `PIL` kitaplığını ve bunları $4 \times 4$ kılavuzunda işlemek için `matplotlib` kitaplığını kullanın.


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

Bing Resim Arama API'sinden yanıtlar JSON olarak döndürülür. Bu örnek yanıt, tek bir sonuç göstermek için kısaltıldı.

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
> [Bing Resim Arama tek sayfalı uygulama öğreticisi](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Bing Resim Arama nedir?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Çevrimiçi etkileşimli bir tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Ücretsiz bir Bilişsel Hizmetler erişim anahtarı alın](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Azure Bilişsel Hizmetler Belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing Resim Arama API’si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
