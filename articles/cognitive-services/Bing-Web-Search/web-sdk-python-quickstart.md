---
title: Web ara SDK Python hızlı başlangıç | Microsoft Docs
description: Web ara SDK konsol uygulaması kurulumu.
titleSuffix: Azure Cognitive Services Web search SDK Python quickstart
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 02/14/2018
ms.author: v-gedod
ms.openlocfilehash: 2a5fed58be863b882b827dbed73862bc690bab1e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355295"
---
# <a name="web-search-sdk-python-quickstart"></a>Web ara SDK Python hızlı başlangıç

Bing Web arama SDK'sı web sorguları ve ayrıştırma sonuçları için REST API işlevselliğini içerir. 

[Kaynak kodu Python Bing Web arama SDK örnekleri için](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/web_search_samples.py) Git hub'da kullanılabilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları
Zaten sahip değilseniz, Python yükleyin. SDK 3.3, 3.4, 3.5 ve 3.6 gibi Python 2.7 ile uyumludur.

Python geliştirme için genel öneri kullanmaktır bir [sanal ortam](https://docs.python.org/3/tutorial/venv.html). Yükleme ve sanal ortamıyla başlatma [venv Modülü](https://pypi.python.org/pypi/virtualenv). Python 2.7 için virtualenv yüklemeniz gerekir.
```
python -m venv mytestenv
```
Bing Web arama SDK bağımlılıkları yükler:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-websearch
```
## <a name="web-search-client"></a>Web ara istemcisi
Alma bir [Bilişsel hizmetler erişim tuşu](https://azure.microsoft.com/try/cognitive-services/) altında *arama*. İçeri aktarmaları ekleyin ve bir örneğini oluşturmak `CognitiveServicesCredentials`:
```
from azure.cognitiveservices.search.websearch import WebSearchAPI
from azure.cognitiveservices.search.websearch.models import SafeSearch
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Ardından, istemci örneği:
```
client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Arama sonuçları için ve ilk Web sayfası sonucu yazdırma:
```
web_data = client.web.search(query="Yosemite")
print("\r\nSearched for Query# \" Yosemite \"")

# WebPages
if web_data.web_pages.value:

    print("\r\nWebpage Results#{}".format(len(web_data.web_pages.value)))

    first_web_page = web_data.web_pages.value[0]
    print("First web page name: {} ".format(first_web_page.name))
    print("First web page URL: {} ".format(first_web_page.url))

else:
    print("Didn't see any Web data..")
```
Resimleri, Haberler ve videolar dahil olmak üzere diğer sonuç türleri Yazdır:
```
# Images
if web_data.images.value:

    print("\r\nImage Results#{}".format(len(web_data.images.value)))

    first_image = web_data.images.value[0]
    print("First Image name: {} ".format(first_image.name))
    print("First Image URL: {} ".format(first_image.url))

else:
    print("Didn't see any Image..")
        
# News
if web_data.news.value:

    print("\r\nNews Results#{}".format(len(web_data.news.value)))

    first_news = web_data.news.value[0]
    print("First News name: {} ".format(first_news.name))
    print("First News URL: {} ".format(first_news.url))

else:
    print("Didn't see any News..")
            
# Videos
if web_data.videos.value:

    print("\r\nVideos Results#{}".format(len(web_data.videos.value)))

    first_video = web_data.videos.value[0]
    print("First Videos name: {} ".format(first_video.name))
    print("First Videos URL: {} ".format(first_video.url))

else:
    print("Didn't see any Videos..")

```
Arama (Seattle en iyi Restoran) için sonuç sayısı doğrulayın ve yazdırmanız `name` ve `URL` ilk sonuç.
```
def web_results_with_count_and_offset(subscription_key):

    client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        web_data = client.web.search(query="Best restaurants in Seattle", offset=10, count=20)
        print("\r\nSearched for Query# \" Best restaurants in Seattle \"")

        if web_data.web_pages.value:

            print("Webpage Results#{}".format(len(web_data.web_pages.value)))

            first_web_page = web_data.web_pages.value[0]
            print("First web page name: {} ".format(first_web_page.name))
            print("First web page URL: {} ".format(first_web_page.url))

        else:
            print("Didn't see any Web data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))```

```
"Xbox" için arama ile `response_filter` atanan `News`.  Haber sonuçları ayrıntılarını yazdırın.
```
def web_search_with_response_filter(subscription_key):

    client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        web_data = client.web.search(query="xbox", response_filter=["News"])
        print("\r\nSearched for Query# \" xbox \" with response filters \"News\"")

        # News attribute since I filtered "News"
        if web_data.news.value:

            print("Webpage Results#{}".format(len(web_data.news.value)))

            first_web_page = web_data.news.value[0]
            print("First web page name: {} ".format(first_web_page.name))
            print("First web page URL: {} ".format(first_web_page.url))

        else:
            print("Didn't see any Web data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
Arama sorgusu "Niagra döner ile", kullanarak `answerCount` ve `promote` parametreleri. Sonuçları ayrıntılarını yazdırın.
```
def web_search_with_answer_count_promote_and_safe_search(subscription_key):

    client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        web_data = client.web.search(
            query="Niagara Falls",
            answer_count=2,
            promote=["videos"],
            safe_search=SafeSearch.strict  # or directly "Strict"
        )
        print("\r\nSearched for Query# \" Niagara Falls\"")

        if web_data.web_pages.value:

            print("Webpage Results#{}".format(len(web_data.web_pages.value)))

            first_web_page = web_data.web_pages.value[0]
            print("First web page name: {} ".format(first_web_page.name))
            print("First web page URL: {} ".format(first_web_page.url))

        else:
            print("Didn't see any Web data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Python SDK'sı örneği](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)


