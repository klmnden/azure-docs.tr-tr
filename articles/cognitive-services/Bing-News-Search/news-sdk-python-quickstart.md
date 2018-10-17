---
title: "Hızlı başlangıç: Bing Haber Arama SDK'sı, Python"
titleSuffix: Azure Cognitive Services
description: Bing Haber Arama SDK'sı konsol uygulaması kurulumu.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 02/14/2018
ms.author: v-gedod
ms.openlocfilehash: 8e4343b053835c0fc2219373ad60f96c7b80636a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803350"
---
# <a name="quickstart-bing-news-search-sdk-with-python"></a>Hızlı başlangıç: Python ile Bing Haber Arama SDK'sı

Haber Arama SDK'sı, web sorguları ve sonuçları ayrıştırmak için REST API işlevselliğini içerir. 

[Python Bing Haber Arama SDK'sı örnekleri için kaynak kodu](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/news_search_samples.py) Git Hub'dan edinilebilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları
Henüz yüklemediyseniz Python'ı yükleyin. SDK, Python 2.7, 3.3, 3.4, 3.5 ve 3.6 ile uyumludur.

Python geliştirmesi için genel öneri [sanal bir ortam](https://docs.python.org/3/tutorial/venv.html) kullanmaktır. Sana ortamı [venv modülü](https://pypi.python.org/pypi/virtualenv) ile yükleyin ve başlatın. Python 2.7 için virtualenv dosyasını yüklemeniz gerekir.
```
python -m venv mytestenv
```
Bing Haber Arama SDK'sı bağımlılıklarını yükleyin:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-newssearch
```
## <a name="news-search-client"></a>Haber Arama istemcisi
*Arama* altından bir [Bilişsel Hizmetler erişim anahtarı](https://azure.microsoft.com/try/cognitive-services/) alın. İçeri aktarmaları ekleyin:
```
from azure.cognitiveservices.search.newssearch import NewsSearchAPI
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
`CognitiveServicesCredentials` örneği oluşturun. İstemciyi başlatın:
```
client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Sonuçları arayın ve ilk web sayfası sonucunu yazdırın:
```
news_result = client.news.search(query="Quantum Computing", market="en-us", count=10)
print("Search news for query \"Quantum Computing\" with market and count")

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
`freshness` ve `sortBy` parametreleriyle "Artificial Intelligence" hakkındaki en güncel haberleri arayıp filtreleyin. Sonuç sayısını doğrulayın ve ilk haber öğesi sonucunun `totalEstimatedMatches`, `name`, `url`, `description`, `published time` ve `name of provider` verilerini yazdırın.
```
def news_search_with_filtering(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        news_result = client.news.search(
            query="Artificial Intelligence",
            market="en-us",
            freshness="Week",
            sort_by="Date"
        )
        print("\r\nSearch most recent news for query \"Artificial Intelligence\" with freshness and sortBy")

        if news_result.value:
            first_news_result = news_result.value[0]
            print("News result count: {}".format(len(news_result.value)))
            print("First news name: {}".format(first_news_result.name))
            print("First news url: {}".format(first_news_result.url))
            print("First news description: {}".format(first_news_result.description))
            print("First published time: {}".format(first_news_result.date_published))
            print("First news provider: {}".format(first_news_result.provider[0].name))
        else:
            print("Didn't see any news result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
Güvenli aramayı etkinleştirerek film, TV ve eğlence kategorilerinde arama yapın. Sonuç sayısını doğrulayın ve ilk haber öğesi sonucunun `category`, `name`, `url`, `description`, `published time` ve `name of provider` verilerini yazdırın.
```
def news_category(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        news_result = client.news.category(
            category="Entertainment_MovieAndTV",
            market="en-us",
            safe_search="strict"
        )
        print("\r\nSearch category news for movie and TV entertainment with safe search")

        if news_result.value:
            first_news_result = news_result.value[0]
            print("News result count: {}".format(len(news_result.value)))
            print("First news category: {}".format(first_news_result.category))
            print("First news name: {}".format(first_news_result.name))
            print("First news url: {}".format(first_news_result.url))
            print("First news description: {}".format(first_news_result.description))
            print("First published time: {}".format(first_news_result.date_published))
            print("First news provider: {}".format(first_news_result.provider[0].name))
        else:
            print("Didn't see any news result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))


```
Bing'de popüler haber başlıklarını arayın.  Sonuç sayısını doğrulayın ve ilk haber sonucunun `name`, `text of query`, `webSearchUrl`, `newsSearchUrl` ve `image Url` verilerini yazdırın.
```
def news_trending(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        trending_topics = client.news.trending(market="en-us")
        print("\r\nSearch news trending topics in Bing")

        if trending_topics.value:
            first_topic = trending_topics.value[0]
            print("News result count: {}".format(len(trending_topics.value)))
            print("First topic name: {}".format(first_topic.name))
            print("First topic query: {}".format(first_topic.query.text))
            print("First topic image url: {}".format(first_topic.image.url))
            print("First topic webSearchUrl: {}".format(first_topic.web_search_url))
            print("First topic newsSearchUrl: {}".format(first_topic.news_search_url))
        else:
            print("Didn't see any topics result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel Hizmetler Python SDK'sı örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)


