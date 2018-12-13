---
title: "Hızlı Başlangıç: Python - Bing haber arama REST API'si ile bir haber arama yapın"
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Python kullanarak Bing haber arama REST API'si için bir istek göndermek için kullanın ve bir JSON yanıtı alırsınız.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 9/21/2017
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 8ce8353df9a6f8354c56d9c9115645c0b7f2136a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53251666"
---
# <a name="quickstart-perform-a-news-search-using-python-and-the-bing-news-search-rest-api"></a>Hızlı Başlangıç: Python ve Bing haber arama REST API'si kullanarak bir haber arama yapın

Bu kılavuz Bing Haber Arama API'si çağrısı oluşturma ve döndürülen JSON nesnesinin işlenmesini anlatan basit bir örnek göstermektedir. Daha fazla bilgi için bkz. [Bing Haber Arama belgeleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference).  

Bu örneği başlatma Bağlayıcı rozetine tıklayarak [Bağlayıcım](https://mybinder.org)’da bir Jupyter not defteri olarak çalıştırabilirsiniz: 

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

## <a name="prerequisites"></a>Önkoşullar

**Bing Arama API'leri**'nde bir [Bilişsel Hizmetler API hesabınız](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) bu hızlı başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirdiğinizde sağlanan erişim anahtarı gerekir.  Ayrıca bkz: [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="running-the-walkthrough"></a>Adımları çalıştırma
Önce `subscription_key` değerini Bing API hizmeti için API anahtarınıza ayarlayın.


```python
subscription_key = None
assert subscription_key
```

Ardından `search_url` uç noktasını doğrulayın. Bu yazının yazıldığı tarihte Bing arama API’leri için tek bir uç nokta kullanılıyordu. Yetkilendirme hatalarıyla karşılaşırsanız bu değeri Azure panonuzdaki Bing arama uç noktasından tekrar kontrol edin.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

`search_term` değerini Microsoft ile ilgili haberleri arayacak şekilde ayarlayın.


```python
search_term = "Microsoft"
```

Aşağıdaki blok, Bing Arama API’lerini çağırmak ve sonuçları bir JSON nesnesi olarak döndürmek için Python’da `requests` kitaplığını kullanmaktadır. API anahtarını `headers` sözlüğü ile geçtiğimize ve terimi `params` sözlüğü ile aradığımıza dikkat edin. Arama sonuçlarını filtrelemek için kullanılabilecek seçeneklerin tam listesi için [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference) belgelerine başvurun.


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
