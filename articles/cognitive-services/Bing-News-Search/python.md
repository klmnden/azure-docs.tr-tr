---
title: 'Hızlı başlangıç: Bing Haber Arama API’si, Python'
titlesuffix: Azure Cognitive Services
description: Bing Haber Arama API'sini kısa sürede kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri alın.
services: cognitive-services
author: v-jerkin
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: 583b304a742d9abfd799442c9aa2999ad6783a34
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803554"
---
# <a name="quickstart-for-bing-news-search-api-with-python"></a>Hızlı başlangıç: Python ile Bing Haber Arama API’si
Bu kılavuz Bing Haber Arama API'si çağrısı oluşturma ve döndürülen JSON nesnesinin işlenmesini anlatan basit bir örnek göstermektedir. Daha fazla bilgi için bkz. [Bing Haber Arama belgeleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference).  

Bu örneği başlatma Bağlayıcı rozetine tıklayarak [Bağlayıcım](https://mybinder.org)'da bir Jupyter not defteri olarak çalıştırabilirsiniz: 

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

## <a name="prerequisites"></a>Ön koşullar

**Bing Arama API'leri**'nde bir [Bilişsel Hizmetler API hesabınız](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) bu hızlı başlangıç için yeterlidir. Ücretsiz denemenizi etkinleştirdiğinizde verilen erişim anahtarınız olması veya Azure panonuzdan ücretli bir abonelik anahtarı kullanmanız gerekir.

## <a name="running-the-walkthrough"></a>Adımları çalıştırma
Önce `subscription_key` değerini Bing API hizmeti için API anahtarınıza ayarlayın.


```python
subscription_key = None
assert subscription_key
```

Ardından `search_url` uç noktasını doğrulayın. Bu yazının yazıldığı tarihte Bing arama API'leri için tek bir uç nokta kullanılıyordu. Yetkilendirme hatalarıyla karşılaşırsanız bu değeri Azure panonuzdaki Bing arama uç noktasından tekrar kontrol edin.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

`search_term` değerini Microsoft ile ilgili haberleri arayacak şekilde ayarlayın.


```python
search_term = "Microsoft"
```

Aşağıdaki blok, Bing Arama API'lerini çağırmak ve sonuçları bir JSON nesnesi olarak döndürmek için Python'da `requests` kitaplığını kullanmaktadır. API anahtarını `headers` sözlüğü ile geçtiğimize ve terimi `params` sözlüğü ile aradığımıza dikkat edin. Arama sonuçlarını filtrelemek için kullanılabilecek seçeneklerin tam listesi için [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference) belgelerine başvurun.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

`search_results` nesnesi terimle ilgili yeni haberlerin yanı sıra zengin meta veriler içerir. Örneğin aşağıdaki kod satırı, haberlerin açıklamalarını ayıklar.


```python
descriptions = [article["description"] for article in search_results["value"]]
```

Bu açıklamalardan arama anahtar sözcüklerinin **kalın** yazıldığı bir tablo oluşturulabilir.


```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc) for desc in descriptions])
HTML("<table>"+rows+"</table>")
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Haberleri sayfalara bölme](paging-news.md)
> [Metni vurgulamak için süsleme işaretçilerini kullanma](hit-highlighting.md)

## <a name="see-also"></a>Ayrıca bkz. 

 [Web'de haber arama](search-the-web.md)  
 [Deneyin](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/)
