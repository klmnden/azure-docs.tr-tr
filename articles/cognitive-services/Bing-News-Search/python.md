---
title: "Hızlı Başlangıç: Python ile bir haber arama ve Bing haber arama REST API'si işlemleri"
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Python kullanarak Bing haber arama REST API'si için bir istek göndermek için kullanın ve bir JSON yanıtı alırsınız.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 6/18/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: c598a9e879cf2f48b6b038f0688d7394075ef521
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206082"
---
# <a name="quickstart-perform-a-news-search-using-python-and-the-bing-news-search-rest-api"></a>Hızlı Başlangıç: Python ve Bing haber arama REST API'si kullanarak bir haber arama yapın

Bu hızlı başlangıçta, bir JSON yanıtı alıp Bing haber arama API'si, ilk çağrı yapmak için kullanın. Bu basit bir JavaScript uygulaması, API için bir arama sorgusu gönderir ve sonuçları işler. Bu uygulama Python'da yazılmıştır, ancak bir RESTful Web API'si, uyumlu, çoğu programlama dilinden hizmet.

Bu kod örneği bir Jupyter not defteri çalıştırma [MyBinder](https://mybinder.org) bağlayıcı başlatma sırasında tıklayarak rozet: 

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

Bu örnek için kaynak kodu de kullanılabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingNewsSearchv7.py).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

Ayrıca bkz: [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE veya düzenleyici yeni bir Python dosyası oluşturun ve istek modülü içeri aktarın. Abonelik anahtarınız, uç noktayı ve bir arama terimi için değişkenler oluşturun. Uç noktanız Azure panosunda bulabilirsiniz.

```python
import requests

subscription_key = "your subscription key"
search_term = "Microsoft"
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

### <a name="create-parameters-for-the-request"></a>İstek için parametreleri oluşturma

1. Yeni bir sözlük için abonelik anahtarınızı ekleme kullanarak `"Ocp-Apim-Subscription-Key"` anahtar. Arama parametrelerinizi için de aynısını yapın.

    ```python
    headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
    params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
    ```

## <a name="send-a-request-and-get-a-response"></a>Bir istek gönderir ve bir yanıt alın

1. Bing görsel arama, abonelik anahtarınız ve son adımda oluşturduğunuz sözlük nesnelerine kullanarak API'yi çağırmak için istekleri kitaplığını kullanın.

    ```python
    response = requests.get(search_url, headers=headers, params=params)
    response.raise_for_status()
    search_results = response.json()
    ```

2. `search_results` API yanıtından JSON nesnesi olarak içerir. Yanıtta bulunan makaleleri açıklamalarını erişin.
    
    ```python
    descriptions = [article["description"] for article in search_results["value"]]
    ```

## <a name="displaying-the-results"></a>Sonuçları görüntüleme

Bu açıklamalardan arama anahtar sözcüklerinin **kalın** yazıldığı bir tablo oluşturulabilir.

```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc) for desc in descriptions])
HTML("<table>"+rows+"</table>")
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](tutorial-bing-news-search-single-page-app.md)
