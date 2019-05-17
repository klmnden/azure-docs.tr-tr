---
title: "Hızlı Başlangıç: Bing varlık arama SDK'sı Python"
titlesuffix: Azure Cognitive Services
description: Bing Varlık Arama SDK'sı konsol uygulaması kurulumu.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 9a66df03ceb03885ba5f61b4adeb54cb1876338d
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65813606"
---
# <a name="quickstart-bing-entity-search-sdk-with-python"></a>Hızlı Başlangıç: Bing varlık arama ile Python SDK'sı

Bing varlık arama SDK'sı ile varlıkları için Python için aramaya başlamak için bu Hızlı Başlangıç'ı kullanın. Bing varlık arama çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/entity_search_samples.py).

## <a name="prerequisites"></a>Önkoşullar

* Python [2.x veya 3.x](https://www.python.org/)

* [İçin Python SDK'sı Bing varlık arama](https://pypi.org/project/azure-cognitiveservices-search-entitysearch/)

Bir python sanal ortamı kullanmanız önerilir. Yükleme ve venv modülüyle bir sanal ortam başlatılamıyor. Python 2.7 ile için virtualenv yükleyebilirsiniz:

```Console
python -m venv mytestenv
```

Bing varlık arama SDK'sı ile yükleyin:

```Console
cd mytestenv
python -m pip install azure-cognitiveservices-search-entitysearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE veya düzenleyici yeni bir Python dosyası oluşturun ve aşağıdaki import deyimlerini girin. 

    ```python
    from azure.cognitiveservices.search.entitysearch import EntitySearchAPI
    from azure.cognitiveservices.search.entitysearch.models import Place, ErrorResponseException
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Abonelik anahtarınız için bir değişken oluşturun ve istemci yeni bir örneğini `CognitiveServicesCredentials` olan nesne.
    
    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    client = EntitySearchAPI(CognitiveServicesCredentials(subscription_key))
    ```

## <a name="send-a-search-request-and-receive-a-response"></a>Arama isteği gönderme ve yanıt

1. Bing varlık arama ile arama isteği göndermek `client.entities.search()` ve bir arama sorgusu. 
    
    ```python
    entity_data = client.entities.search(query="Gibralter")
    ```

2. Varlıkları döndürülmedi, dönüştürme `entity_data.entities.value` bir listesi ve yazdırma ilk sonucu.
    ```python
    if entity_data.entities.value:
    
        main_entities = [entity for entity in entity_data.entities.value
                         if entity.entity_presentation_info.entity_scenario == "DominantEntity"]
    
        if main_entities:
            print(main_entities[0].description)
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../tutorial-bing-entities-search-single-page-app.md)

* [Bing varlık arama API'si nedir?](../overview.md )