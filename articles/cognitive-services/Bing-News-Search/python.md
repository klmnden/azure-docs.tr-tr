---
title: Bilişsel hizmetler Azure, Bing Haberler arama API için Python hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get Bing Haberler arama API Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki kullanmaya başlayın.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: 0fde478b650513aa1527c1d47f5b453ba094506c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352354"
---
# <a name="quickstart-for-bing-news-search-api-with-python"></a>Bing Haberler arama API'si ile Python için hızlı başlangıç
Bu anlatımda Bing Haberler arama API'yi çağıran ve sonuçta elde edilen JSON nesnesi sonrası işlem basit bir örnek gösterilir. Daha fazla bilgi için bkz: [Bing yeni arama belgelerine](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference).  

Bu örneği Jupyter not defteri çalıştırmak [MyBinder](https://mybinder.org) başlatılırken bağlayıcı tıklayarak göstergeye: 

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

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
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

Ayarlama `search_term` Microsoft hakkındaki haber makalelerini aramak için.


```python
search_term = "Microsoft"
```

Aşağıdaki kullanımları engellemek `requests` Bing arama API'leri duyurmak ve sonuçları bir JSON nesnesi olarak dönmek için Python kitaplığında. Biz API anahtarında geçirmek gözlemlemek `headers` sözlük ve arama terimi aracılığıyla `params` sözlük. Arama sonuçlarını filtrelemek için kullanılabilir seçenekleri tam listesini görmek için bkz [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference) belgeleri.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

`search_results` Nesne zengin meta verileri ile birlikte ilgili yeni makaleler içerir. Örneğin, aşağıdaki kod satırını makaleleri açıklamalarını ayıklar.


```python
descriptions = [article["description"] for article in search_results["value"]]
```

Bu açıklamalar sonra tablo olarak vurgulanan arama anahtar sözcüğü ile işlenip **kalın**.


```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc) for desc in descriptions])
HTML("<table>"+rows+"</table>")
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Disk belleği haber](paging-news.md)
> [metin vurgulamak için decoration işaretlerini kullanma](hit-highlighting.md)

## <a name="see-also"></a>Ayrıca bkz. 

 [Web haber için arama](search-the-web.md)  
 [Deneyin](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/)
