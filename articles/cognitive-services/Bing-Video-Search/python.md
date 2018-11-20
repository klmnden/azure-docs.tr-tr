---
title: 'Hızlı Başlangıç: Bing Video Arama, Python'
titlesuffix: Azure Cognitive Services
description: Bing Video Arama API'sini kısa sürede kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri alın.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: quickstart
ms.date: 9/21/2017
ms.author: aahi
ms.openlocfilehash: ccc27481289ffc686e3e480685ba421c762e3718
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52161098"
---
# <a name="quickstart-bing-video-search-api-with-python"></a>Hızlı Başlangıç: Python ile Bing Video Arama API'si

Bu yazıda Azure'da Microsoft Bilişsel Hizmetleri'nin parçası olan Bing Video Arama API'sini kullanma adımları gösterilmektedir. API'lerle ilgili teknik ayrıntılar için [API başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference)'na bakın.

Bu örneği başlatma Bağlayıcı rozetine tıklayarak [Bağlayıcım](https://mybinder.org)’da bir Jupyter not defteri olarak çalıştırabilirsiniz: 

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingVideoSearchAPI.ipynb)


## <a name="prerequisites"></a>Önkoşullar
**Bing Arama API'leri**'nde bir [Bilişsel Hizmetler API hesabınız](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) bu hızlı başlangıç için yeterlidir. Ücretsiz denemenizi etkinleştirdiğinizde verilen erişim anahtarınız olması veya Azure panonuzdan ücretli bir abonelik anahtarı kullanmanız gerekir.

## <a name="running-the-walkthrough"></a>Adımları çalıştırma

Önce `subscription_key` değerini Bing API hizmeti için API anahtarınıza ayarlayın.


```python
subscription_key = None
assert subscription_key
```

Ardından `search_url` uç noktasını doğrulayın. Bu yazının yazıldığı tarihte Bing arama API’leri için tek bir uç nokta kullanılıyordu. Yetkilendirme hatalarıyla karşılaşırsanız bu değeri Azure panonuzdaki Bing arama uç noktasından tekrar kontrol edin.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/videos/search"
```

Kedi yavrusu videoları aramak için `search_term` değerini ayarlayın


```python
search_term = "kittens"
```

Aşağıdaki blok, Bing Arama API'lerini çağırmak ve sonuçları bir JSON nesnesi olarak döndürmek için Python'da `requests` kitaplığını kullanmaktadır. API anahtarını `headers` sözlüğü ile geçtiğimize ve terimi `params` sözlüğü ile aradığımıza dikkat edin. Arama sonuçlarını filtrelemek için kullanılabilecek seçeneklerin tam listesi için [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) belgelerine başvurun.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "count":5, "pricing": "free", "videoLength":"short"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

`search_results` nesnesi terimle ilgili videoların yanı sıra zengin meta veriler içerir. Videolardan birini görüntülemek için videonun `embedHtml` özelliğini kullanın ve videoyu `IFrame` içine ekleyin.


```python
from IPython.display import HTML
HTML(search_results["value"][0]["embedHtml"].replace("autoplay=1","autoplay=0"))
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Video çağırma](paging-videos.md)
> [Küçük resimleri yeniden boyutlandırma ve kırpma](resize-and-crop-thumbnails.md)

## <a name="see-also"></a>Ayrıca bkz. 

 [Web'de video arama](search-the-web.md) [Deneyin](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/)
