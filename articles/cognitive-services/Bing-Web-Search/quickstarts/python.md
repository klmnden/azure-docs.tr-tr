---
title: 'Hızlı Başlangıç: Bing Web Araması API’sini çağırmak için Python kullanma'
description: Bu hızlı başlangıçta, Python kullanarak ilk Bing Web Araması API'si çağrınızı yapmayı ve bir JSON yanıtı almayı öğreneceksiniz.
services: cognitive-services
author: erhopf
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 8/16/2018
ms.author: erhopf
ms.openlocfilehash: cd53a323a07617284e82004a6b3feed57b6e15e2
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888616"
---
# <a name="quickstart-use-python-to-call-the-bing-web-search-api"></a>Hızlı Başlangıç: Bing Web Araması API’sini çağırmak için Python kullanma  

10 dakikadan daha kısa bir sürede ilk Bing Web Araması API'si çağrınızı yapmak ve bir JSON yanıtı almak için bu hızlı başlangıcı kullanın.  

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

Bu örnek [MyBinder](https://mybinder.org) üzerinde bir Jupyter notebook olarak çalıştırılır. Bağlayıcıyı başlat rozetine tıklayın:

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)

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

## <a name="sample-code-on-github"></a>GitHub'da örnek kod

Bu kodu yerel ortamda çalıştırmak isterseniz [GitHub'da örneğin tamamına ulaşabilirsiniz](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingWebSearchv7.js).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Web araması tek sayfalı uygulama öğreticisi](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
