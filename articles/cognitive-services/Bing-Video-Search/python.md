---
title: Bilişsel hizmetler Azure, Bing Video arama API için Python hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get Bing Video arama API Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki kullanmaya başlayın.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: ce4356f05e69540bc3bc3241e2ec1751ff7a7276
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352342"
---
# <a name="quickstart-for-bing-video-search-api-with-python"></a>Bing Video arama API'si ile Python için hızlı başlangıç

Bu kılavuzda Microsoft Bilişsel hizmetler Azure üzerinde bir parçası Bing Video arama API kullanmayı gösterir. Başvurmak [API Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) API'leri hakkında teknik ayrıntılar için.

Bu örneği Jupyter not defteri çalıştırmak [MyBinder](https://mybinder.org) başlatılırken bağlayıcı tıklayarak göstergeye: 

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingVideoSearchAPI.ipynb)


## <a name="prerequisites"></a>Önkoşullar
Bilmeniz gereken bir [Bilişsel Hizmetleri API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Bing arama API'leri**. [Ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) Bu Hızlı Başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirmek ya da Ücretli abonelik anahtarı Azure panonuza kullanabilir sağlanan erişim anahtarı gerekir.

## <a name="running-the-walkthrough"></a>İzlenecek yol çalıştırma

Öncelikle, ayarlamış `subscription_key` API anahtarınıza Bing API'si hizmeti için.


```python
subscription_key = None
assert subscription_key
```

Ardından, doğrulayın `search_url` doğru uç noktadır. Bu yazma sırasında tek bir uç nokta Bing arama API'leri için kullanılır. Yetkilendirme hatalarla karşılaşırsanız, bu değer Azure panonuza Bing arama uç karşı denetleyin.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/videos/search"
```

Ayarlama `search_term` kediler videolar için aramak için


```python
search_term = "kittens"
```

Aşağıdaki kullanımları engellemek `requests` Bing arama API'leri duyurmak ve sonuçları bir JSON nesnesi olarak dönmek için Python kitaplığında. Biz API anahtarında geçirmek gözlemlemek `headers` sözlük ve arama terimi aracılığıyla `params` sözlük. Arama sonuçlarını filtrelemek için kullanılabilir seçenekleri tam listesini görmek için bkz [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) belgeleri.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "count":5, "pricing": "free", "videoLength":"short"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

`search_results` Nesnesini içeren zengin meta verileri ile birlikte ilgili videolar. Görünüme videoları birini kullanın, `embedHtml` özelliği ve ekleyip içine bir `IFrame`.


```python
from IPython.display import HTML
HTML(search_results["value"][0]["embedHtml"].replace("autoplay=1","autoplay=0"))
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Disk belleği videolar](paging-videos.md)
> [Resizing ve küçük resim görüntüleri kırpma](resize-and-crop-thumbnails.md)

## <a name="see-also"></a>Ayrıca bkz. 

 [Web videolar için arama](search-the-web.md) [deneyin](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/)
