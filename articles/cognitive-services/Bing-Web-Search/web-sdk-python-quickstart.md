---
title: "Hızlı Başlangıç: Python için Bing Web araması SDK'sını kullanma"
titleSuffix: Azure Cognitive Services
description: Bing Web Araması SDK'sı, Bing Web Araması özelliklerini Python uygulamanızla tümleştirmeyi kolaylaştırır. Bu hızlı başlangıçta istek göndermeyi, JSON yanıtı almayı, sonuçları filtrelemeyi ve ayrıştırmayı öğreneceksiniz.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 03/12/2019
ms.author: aahi
ms.openlocfilehash: 273922c8cf48c24ff3b1b55fa44b36b69e061057
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122549"
---
# <a name="quickstart-use-the-bing-web-search-sdk-for-python"></a>Hızlı Başlangıç: Python için Bing Web araması SDK'sını kullanma

Bing Web Araması SDK'sı, Bing Web Araması özelliklerini Python uygulamanızla tümleştirmeyi kolaylaştırır. Bu hızlı başlangıçta istek göndermeyi, JSON yanıtı almayı, sonuçları filtrelemeyi ve ayrıştırmayı öğreneceksiniz.

Kodu hemen görmek istiyor musunuz? GitHub'daki [Python için Bing Web Araması SDK'sı örneklerini](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) inceleyebilirsiniz.

[!INCLUDE [bing-web-search-quickstart-signup](../../../includes/bing-web-search-quickstart-signup.md)]

Ayrıca bkz: [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="prerequisites"></a>Önkoşullar
Bing Web Araması SDK'sı Python 2.7, 3.3, 3.4, 3.5 ve 3.6 ile uyumludur. Bu hızlı başlangıç için sanal ortam kullanmanızı öneririz.

* Python 2.7, 3.3, 3.4, 3.5 veya 3.6
* Python 2.7 için [virtualenv](https://docs.python.org/3/tutorial/venv.html)
* Python 3.x için [venv](https://pypi.python.org/pypi/virtualenv)

## <a name="create-and-configure-your-virtual-environment"></a>Sanal ortamınızı oluşturma ve yapılandırma

Sanal ortamınızı ayarlama ve yapılandırma talimatları Python 2.x ve Python 3.x sürümleri için değişiklik gösterecektir. Sanal ortamınızı oluşturmak ve başlatmak için aşağıdaki adımları izleyin.

### <a name="python-2x"></a>Python 2.x

Python 2.7 için `virtualenv` ile sanal ortam oluşturun:

```console
virtualenv mytestenv
```

Ortamınızı etkinleştirme:

```console
cd mytestenv
source bin/activate
```

Bing Web Araması SDK'sı bağımlılıklarını yükleyin:

```console
python -m pip install azure-cognitiveservices-search-websearch
```

### <a name="python-3x"></a>Python 3.x

Python 3.x için `venv` ile sanal ortam oluşturun:

```console
python -m venv mytestenv
```

Bing Web Araması SDK'sı bağımlılıklarını yükleyin:

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-websearch
```

## <a name="create-a-client-and-print-your-first-results"></a>İstemci oluşturma ve ilk sonuçlarınızı yazdırma

Sanal ortamınızı ayarladığınıza ve bağımlılıkları yüklediğinizde göre bir istemci oluşturabilirsiniz. İstemci, Bing Web Araması API'sinden gelen istekleri ve yanıtları işler.

Yanıtta web sayfaları, görüntüler, haberler veya videolar varsa hepsinin ilk sonuçları yazdırılır.

1. Favori IDE ortamınızda veya düzenleyicide yeni bir Python projesi oluşturun.

1. Şu örnek kodu projenize kopyalayın:  

    ```python
    # Import required modules.
    from azure.cognitiveservices.search.websearch import WebSearchAPI
    from azure.cognitiveservices.search.websearch.models import SafeSearch
    from msrest.authentication import CognitiveServicesCredentials

    # Replace with your subscription key.
    subscription_key = "YOUR_SUBSCRIPTION_KEY"

    # Instantiate the client.
    client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

    # Make a request. Replace Yosemite if you'd like.
    web_data = client.web.search(query="Yosemite")
    print("\r\nSearched for Query# \" Yosemite \"")

    '''
    Web pages
    If the search response contains web pages, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.web_pages, 'value'):

        print("\r\nWebpage Results#{}".format(len(web_data.web_pages.value)))

        first_web_page = web_data.web_pages.value[0]
        print("First web page name: {} ".format(first_web_page.name))
        print("First web page URL: {} ".format(first_web_page.url))

    else:
        print("Didn't find any web pages...")

    '''
    Images
    If the search response contains images, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.images, 'value'):

        print("\r\nImage Results#{}".format(len(web_data.images.value)))

        first_image = web_data.images.value[0]
        print("First Image name: {} ".format(first_image.name))
        print("First Image URL: {} ".format(first_image.url))

    else:
        print("Didn't find any images...")

    '''
    News
    If the search response contains news, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.news, 'value'):

        print("\r\nNews Results#{}".format(len(web_data.news.value)))

        first_news = web_data.news.value[0]
        print("First News name: {} ".format(first_news.name))
        print("First News URL: {} ".format(first_news.url))

    else:
        print("Didn't find any news...")

    '''
    If the search response contains videos, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.videos, 'value'):

        print("\r\nVideos Results#{}".format(len(web_data.videos.value)))

        first_video = web_data.videos.value[0]
        print("First Videos name: {} ".format(first_video.name))
        print("First Videos URL: {} ".format(first_video.url))

    else:
        print("Didn't find any videos...")
    ```

1. `subscription_key` değerini geçerli bir abonelik anahtarıyla değiştirin.

1. Programı çalıştırın. Örneğin: `python your_program.py`.

## <a name="define-functions-and-filter-results"></a>İşlevleri tanımlama ve sonuçları filtreleme

Bing Web araması API'si için ilk çağrınızı yaptığınız, bazı işlevler bakalım. Aşağıdaki bölümlerde, sorgular verilerin iyileştirilmesi ve sonuçları filtrelemek için SDK işlevselliğini vurgulayın. Her işlev, önceki bölümde oluşturduğunuz Python programa eklenebilir.

### <a name="limit-the-number-of-results-returned-by-bing"></a>Bing tarafından döndürülen sonuç sayısını sınırlama

Bu örnekte SDK'nın [`search` metodu](https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python) kullanılarak döndürülen sonuçları sınırlandırmak için `count` ve `offset` parametreleri kullanılmıştır. İlk sonucun `name` ve `url` değerleri yazdırılır.

1. Python projenize şu kodu ekleyin:

   ```python
    # Declare the function.
    def web_results_with_count_and_offset(subscription_key):
        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

        try:
            '''
            Set the query, offset, and count using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(query="Best restaurants in Seattle", offset=10, count=20)
            print("\r\nSearching for \"Best restaurants in Seattle\"")

            if web_data.web_pages.value:
                '''
                If web pages are available, print the # of responses, and the first and second
                web pages returned.
                '''
                print("Webpage Results#{}".format(len(web_data.web_pages.value)))

                first_web_page = web_data.web_pages.value[0]
                print("First web page name: {} ".format(first_web_page.name))
                print("First web page URL: {} ".format(first_web_page.url))

            else:
                print("Didn't find any web pages...")

        except Exception as err:
            print("Encountered exception. {}".format(err))
    ```

1. Programı çalıştırın.

### <a name="filter-for-news-and-freshness"></a>Haberler ve güncellik filtresi

Bu örnekte SDK'nın [`search` metodu](/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations) ile arama sonuçlarını filtrelemek için `response_filter` ve `freshness` parametreleri kullanılmıştır. Döndürülen arama sonuçları Bing'in son 24 saat içinde keşfettiği haberler ve sayfalarla sınırlıdır. İlk sonucun `name` ve `url` değerleri yazdırılır.

1. Python projenize şu kodu ekleyin:

    ```python
    # Declare the function.
    def web_search_with_response_filter(subscription_key):
        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))
        try:
            '''
            Set the query, response_filter, and freshness using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(query="xbox",
                response_filter=["News"],
                freshness="Day")
            print("\r\nSearching for \"xbox\" with the response filter set to \"News\" and freshness filter set to \"Day\".")

            '''
            If news articles are available, print the # of responses, and the first and second
            articles returned.
            '''
            if web_data.news.value:

                print("# of news results: {}".format(len(web_data.news.value)))

                first_web_page = web_data.news.value[0]
                print("First article name: {} ".format(first_web_page.name))
                print("First article URL: {} ".format(first_web_page.url))

                print("")

                second_web_page = web_data.news.value[1]
                print("\nSecond article name: {} ".format(second_web_page.name))
                print("Second article URL: {} ".format(second_web_page.url))

            else:
                print("Didn't find any news articles...")

        except Exception as err:
            print("Encountered exception. {}".format(err))

    # Call the function.
    web_search_with_response_filter(subscription_key)
    ```

1. Programı çalıştırın.

### <a name="use-safe-search-answer-count-and-the-promote-filter"></a>Güvenli arama, yanıt sayısı ve yükseltme filtresini kullanma

Bu örnekte SDK'nın [`search` metodu](https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python) ile arama sonuçlarını filtrelemek için `answer_count`, `promote` ve `safe_search` parametreleri kullanılmıştır. İlk sonucun `name` ve `url` değerleri görüntülenir.

1. Python projenize şu kodu ekleyin:

    ```python
    # Declare the function.
    def web_search_with_answer_count_promote_and_safe_search(subscription_key):

        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

        try:
            '''
            Set the query, answer_count, promote, and safe_search parameters using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(
                query="Niagara Falls",
                answer_count=2,
                promote=["videos"],
                safe_search=SafeSearch.strict  # or directly "Strict"
            )
            print("\r\nSearching for \"Niagara Falls\"")

            '''
            If results are available, print the # of responses, and the first result returned.
            '''
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

1. Programı çalıştırın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu projeyi tamamladıktan sonra abonelik anahtarınızı program kodundan kaldırmayı ve sanal ortamınızı devre dışı bırakmayı unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilişsel Hizmetler Python SDK'sı örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Python SDK'sı başvurusu](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/websearch)
