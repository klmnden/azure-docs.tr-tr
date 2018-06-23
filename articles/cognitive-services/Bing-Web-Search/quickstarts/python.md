---
title: Çağrı ve yanıt - Azure Bilişsel hizmetler, Bing Web arama API için Python hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get Bing Web arama API Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki kullanmaya başlayın.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 9/18/2017
ms.author: v-jerkin
ms.openlocfilehash: 8d4df9db60c7a74a5b9e53d4622528c0054b4f19
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352438"
---
# <a name="call-and-response-your-first-bing-web-search-query-in-python"></a>Çağrı ve yanıt: Python, ilk Bing Web arama sorgusu

Bing Web arama API Bing belirler arama sonuçları kullanıcının sorgu ile ilgili döndürerek Bing.com/Search için benzer bir deneyim sağlar. Sonuçları, Web sayfaları, resim, video, haber ve varlıklar, ilgili arama sorguları, yazım düzeltmeleri, saat dilimleri, birim dönüştürme, çeviri ve hesaplamalar birlikte içerebilir. Tür sonuç elde ilgileri ve abone olduğunuz Bing arama API'leri katmanını temel alır.

Başvurmak [API Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) API'leri hakkında teknik ayrıntılar için.

Bu örneği Jupyter not defteri çalıştırmak [MyBinder](https://mybinder.org) başlatılırken bağlayıcı tıklayarak göstergeye: 

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)


## <a name="prerequisites"></a>Önkoşullar
Bilmeniz gereken bir [Bilişsel Hizmetleri API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Bing arama API'leri**. [Ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) Bu Hızlı Başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirmek ya da Ücretli abonelik anahtarı Azure panonuza kullanabilir sağlanan erişim anahtarı gerekir.

## <a name="running-the-walkthrough"></a>İzlenecek yol çalıştırma

Ayarlama `subscription_key` API anahtarınıza Bing API'si hizmeti için.


```python
subscription_key = None
assert subscription_key
```

Ardından, doğrulayın `search_url` doğru uç noktadır. Bu yazma sırasında tek bir uç nokta Bing arama API'leri için kullanılır. Yetkilendirme hatalarla karşılaşırsanız, bu değer Azure panonuza Bing arama uç karşı denetleyin.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

Ayarlama `search_term` Microsoft Bilişsel hizmetler için Bing sorgulanamıyor.


```python
search_term = "Microsoft Cognitive Services"
```

Aşağıdaki kullanımları engellemek `requests` Bing arama API'leri duyurmak ve sonuçları bir JSON nesnesi olarak dönmek için Python kitaplığında. Biz API anahtarında geçirmek gözlemlemek `headers` sözlük ve arama terimi aracılığıyla `params` sözlük. Arama sonuçlarını filtrelemek için kullanılabilir seçenekleri tam listesini görmek için bkz [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) belgeleri.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations":True, "textFormat":"HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

`search_results` Nesne ilgili sorgular ve sayfalar gibi zengin meta verilerin yanı sıra arama sonuçlarını içerir. Aşağıdaki kod satırlarını sorgu tarafından döndürülen üst sayfaları biçimlendirin.


```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"],v["name"],v["snippet"]) \
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Web arama tek sayfa uygulaması Öğreticisi](../tutorial-bing-web-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Bing Web araması genel bakış](../overview.md)  
[Deneyin](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/)  
[Ücretsiz deneme erişim anahtarı alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api)
[Bing Web arama API Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
