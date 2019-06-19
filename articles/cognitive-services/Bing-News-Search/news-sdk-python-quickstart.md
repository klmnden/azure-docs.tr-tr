---
title: "Hızlı Başlangıç: Python için Bing haber arama SDK'sını kullanarak bir haber arama yapın"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Python için Bing haber arama SDK'sını kullanarak haber aramak için kullanın ve işlem yanıt.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 06/18/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: e13c500359c185d36a2e89914a1c112ee2e82cbc
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206103"
---
# <a name="quickstart-perform-a-news-search-with-the-bing-news-search-sdk-for-python"></a>Hızlı Başlangıç: Python için Bing haber arama SDK'sı ile haber araması

Bing haber arama SDK'sı ile haberler için Python için aramaya başlamak için bu Hızlı Başlangıç'ı kullanın. Bing haber arama çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/news_search_samples.py).

## <a name="prerequisites"></a>Önkoşullar

* [Python](https://www.python.org/) 2.x veya 3.x

Kullanmak için önerilen bir [sanal ortam](https://docs.python.org/3/tutorial/venv.html) python geliştirme için. Yükleme ve sanal ortamıyla başlatmak [venv Modülü](https://pypi.python.org/pypi/virtualenv). Python 2.7 için bir virtualenv yüklemeniz gerekir. Sanal bir ortamda oluşturabilirsiniz:

```console
python -m venv mytestenv
```

Bu komutla birlikte Bing haber arama SDK bağımlılıklarını yükleyebilirsiniz:
    
```console
python -m pip install azure-cognitiveservices-search-newssearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE veya düzenleyici yeni bir Python dosyası oluşturun ve aşağıdaki kitaplıkları içeri aktarma. Abonelik anahtarınız ve arama için bir değişken oluşturun.

    ```python
    from azure.cognitiveservices.search.newssearch import NewsSearchAPI
    from msrest.authentication import CognitiveServicesCredentials
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    search_term = "Quantum Computing"
    ```

## <a name="initialize-the-client-and-send-a-request"></a>İstemcisini başlatın ve bir istek gönderin

1. `CognitiveServicesCredentials` örneği oluşturun. İstemciyi başlatın:
    
    ```python
    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))
    ```

2. Haber arama API'si için bir arama sorgusu gönderin, yanıtı depolayın.

    ```python
    news_result = client.news.search(query=search_term, market="en-us", count=10)
    ```

## <a name="parse-the-response"></a>Yanıt Ayrıştırma

Tüm arama sonuçlarını bulunursa, ilk Web sayfası sonucu yazdırın:

```python
if news_result.value:
    first_news_result = news_result.value[0]
    print("Total estimated matches value: {}".format(news_result.total_estimated_matches))
    print("News result count: {}".format(len(news_result.value)))
    print("First news name: {}".format(first_news_result.name))
    print("First news url: {}".format(first_news_result.url))
    print("First news description: {}".format(first_news_result.description))
    print("First published time: {}".format(first_news_result.date_published))
    print("First news provider: {}".format(first_news_result.provider[0].name))
else:
    print("Didn't see any news result data..")
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](tutorial-bing-news-search-single-page-app.md)
