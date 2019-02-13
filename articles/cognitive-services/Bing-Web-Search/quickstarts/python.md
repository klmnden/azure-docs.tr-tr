---
title: "Hızlı Başlangıç: Python - Bing Web araması API'si ile bir arama yapın"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Python kullanarak Bing Web araması REST API'si için istekleri göndermek için kullanın ve bir JSON yanıtı alırsınız.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 02/12/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: e191821c8e2c6e189e8d7f8befcb11009f2ed7d8
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56172374"
---
# <a name="quickstart-use-python-to-call-the-bing-web-search-api"></a>Hızlı Başlangıç: Bing Web araması API'si çağırmak için Python kullanma  

Bu hızlı başlangıçta, Bing Web araması API'si, ilk çağrı yapmak ve JSON yanıtını almak için kullanın. Python uygulaması bu API için bir arama isteği gönderir ve yanıt görüntüler. Bu uygulama Python ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir.

Bu örnek [MyBinder](https://mybinder.org) üzerinde bir Jupyter notebook olarak çalıştırılır. Bağlayıcıyı başlat rozetine tıklayın:

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)

## <a name="prerequisites"></a>Önkoşullar

* [Python 2.x veya 3.x](https://www.python.org/)

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="define-variables"></a>Değişkenleri tanımlama

`subscription_key` değerini Azure hesabınızdan geçerli bir abonelik anahtarı ile değiştirin.

```python
subscription_key = "YOUR_ACCESS_KEY"
assert subscription_key
```

Bing Web Araması API’si uç noktasını tanımlayın. Yetkilendirme hatasıyla karşılaşırsanız bu değeri Azure panonuzdaki Bing arama uç noktasından tekrar kontrol edin.

```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

`search_term` için değeri değiştirerek arama sorgusunu değiştirebilirsiniz.

```python
search_term = "Azure Cognitive Services"
```

## <a name="make-a-request"></a>İstekte bulunma

Bu blokta `requests` kitaplığı kullanılarak Bing Web Araması API'si çağrılır ve sonuçlar JSON nesnesi olarak döndürülür. API anahtarı `headers` dizininde, arama terimi ile sorgu parametreleri de `params` dizininde iletilir. Tüm seçeneklerin ve parametrelerin listesi için [Bing Web Araması API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) belgelerine bakın.

```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations":True, "textFormat":"HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

## <a name="format-and-display-the-response"></a>Yanıtı biçimlendirme ve görüntüleme

`search_results` nesnesinde arama sonuçlarına ek olarak ilgili sorgular ve sayfalar gibi meta veriler bulunur. Bu kodda yanıtı biçimlendirmek ve tarayıcınızda görüntülemek için `IPython.display` kitaplığı kullanılır.

```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"],v["name"],v["snippet"]) \
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="sample-code-on-github"></a>GitHub üzerinde örnek kod

Bu kodu yerel ortamda çalıştırmak isterseniz [GitHub'da örneğin tamamına ulaşabilirsiniz](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingWebSearchv7.js).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Web araması tek sayfalı uygulama öğreticisi](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
