---
title: "Hızlı Başlangıç: Python SDK'sını kullanarak Bing özel arama uç noktanızı arayın | Microsoft Docs"
titleSuffix: Azure Cognitive Services
description: Özel arama sonuçları elde etmek için Bing özel arama için SDK'sı Python'ı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 03/05/2019
ms.author: aahi
ms.openlocfilehash: c4c5059bc57ea33357145f6b119456dc6c5bdb7b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60946650"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-the-python-sdk"></a>Hızlı Başlangıç: Python SDK'sını kullanarak Bing özel arama uç noktanızı arayın 

Bu hızlı başlangıçta, arama sonuçlarını Python SDK'sını kullanarak Bing özel arama örneğinizin talep başlamak için kullanın. Bing özel arama çoğu programlama dilleri ile uyumlu bir REST API olsa da Bing özel arama SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/custom_search_samples.py) ek hata işleme ve ek açıklamalar.

## <a name="prerequisites"></a>Önkoşullar

- Bing özel arama örneği için. Bkz: [hızlı başlangıç: İlk Bing özel arama örneğinizin oluşturma](quick-start.md) daha fazla bilgi için.
- Python [2.x veya 3.x](https://www.python.org/) 
- [Python için Bing özel arama SDK'sı](https://pypi.org/project/azure-cognitiveservices-search-customsearch/) 

## <a name="install-the-python-sdk"></a>Python SDK'sını yükleme

Bing özel arama SDK'sı şu komutla yükleyin.

```Console
python -m pip install azure-cognitiveservices-search-customsearch
```


## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

Tercih ettiğiniz düzenleyiciyi veya IDE içinde yeni bir Python dosyası oluşturun ve aşağıdaki içeri aktarmaları ekleyin.

```python
from azure.cognitiveservices.search.customsearch import CustomSearchClient
from msrest.authentication import CognitiveServicesCredentials
```

## <a name="create-a-search-client-and-send-a-request"></a>Bir arama istemcisi oluşturma ve bir istek gönderin

1. Abonelik anahtarınız için bir değişken oluşturun.

    ```python
    subscription_key = 'your-subscription-key'
    ```

2. Bir örneğini oluşturmak `CustomSearchClient`kullanarak bir `CognitiveServicesCredentials` abonelik anahtarını içeren nesne. 

    ```python
    client = CustomSearchClient(CognitiveServicesCredentials(subscription_key))
    ```

3. Arama isteği gönderme `client.custom_instance.search()`. Arama teriminizi ekleme `query` parametresi ve kümesi `custom_config` arama örneğinizin kullanmak için özel yapılandırma kimliğinizin. Kimliğinizi dan alabileceğiniz [Bing özel arama portalı](https://www.customsearch.ai/), tıklayarak **üretim** sekmesi.

    ```python
    web_data = client.custom_instance.search(query="xbox", custom_config="your-configuration-id")
    ```

## <a name="view-the-search-results"></a>Arama sonuçlarını görüntüleme

Herhangi bir web sayfası arama sonucu bulundu, ilk alın ve adı, URL ve toplam web sayfaları bulundu yazdırın.

```python
if web_data.web_pages.value:
    first_web_result = web_data.web_pages.value[0]
    print("Web Pages result count: {}".format(len(web_data.web_pages.value)))
    print("First Web Page name: {}".format(first_web_result.name))
    print("First Web Page url: {}".format(first_web_result.url))
else:
    print("Didn't see any web data..")
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir özel arama web uygulaması derleme](./tutorials/custom-search-web-page.md)
