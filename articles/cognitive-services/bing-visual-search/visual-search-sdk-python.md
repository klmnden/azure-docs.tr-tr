---
title: "Hızlı Başlangıç: Bing Görsel Arama SDK'sı, Python"
titleSuffix: Azure Cognitive Services
description: Python Görsel Arama SDK'sı konsol uygulamasını kurma.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 06/11/2018
ms.author: v-gedod
ms.openlocfilehash: 935241d5574a59edfa256b74db3a0aeab2f7bf91
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49458114"
---
# <a name="quickstart-bing-visual-search-sdk-python"></a>Hızlı Başlangıç: Bing Görsel Arama SDK'sı Python

Bing Görsel Arama SDK'sı, web istekleri ve sonuçları ayrıştırma için REST API işlevlerini kullanır.
[Python Bing Görsel Arama SDK'sı örnekleri için kaynak kodu](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/visual_search_samples.py) Git Hub'dan edinilebilir.

Kod senaryoları aşağıdaki başlıklar altında belgelenmiştir:
* [Görsel Arama İstemcisi](#client)
* [Tam konsol uygulaması](#complete-console)
* [cropArea ile görüntü ikili gönderisi](#binary-crop)
* [KnowledgeRequest parametresi](#knowledge-req)
* [Etiketler, eylemler ve actionType](#tags-actions)

## <a name="application-dependencies"></a>Uygulama bağımlılıkları
* SDK çağrılarının kimliğini doğrulamak için bir bilişsel hizmetler API anahtarı gerekir. [Ücretsiz deneme anahtarına](https://azure.microsoft.com/try/cognitive-services/?api=search-api-v7) kaydolun. Deneme anahtarı yedi gün boyunca saniyede 1 çağrı için kullanılabilir. Üretim aşaması senaryosu için [erişim anahtarı satın alın](https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7). Ayrıca bkz: [fiyatlandırma bilgileri](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/visual/).
* Henüz yüklemediyseniz Python'ı yükleyin. SDK, Python 2.7, 3.3, 3.4, 3.5 ve 3.6 ile uyumludur.
* Python geliştirmesi için genel öneri [sanal bir ortam](https://docs.python.org/3/tutorial/venv.html) kullanmaktır. Sana ortamı [venv modülü](https://pypi.python.org/pypi/virtualenv) ile yükleyin ve başlatın. Python 2.7 için virtualenv dosyasını yükleyin.
```
python -m venv mytestenv
```
Bing Görsel Arama SDK'sı bağımlılık dosyalarını yükleyin:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-visualsearch
```

<a name="client"></a> 
## <a name="visual-search-client"></a>Görsel Arama istemcisi
`VisualSearchAPI` istemcisinin bir örneğini oluşturmak için aşağıdaki kitaplıkları içeri aktarın:
```
import http.client, urllib.parse
import json
import os.path
from azure.cognitiveservices.search.visualsearch import VisualSearchAPI
from azure.cognitiveservices.search.visualsearch.models import (
    VisualSearchRequest,
    CropArea,
    ImageInfo,
    Filters,
    KnowledgeRequest,
)
```
subscriptionKey dizesi değerini geçerli abonelik anahtarınız ile değiştirin.
```
subscription_key = 'YOUR-VISUAL-SEARCH-ACCESS-KEY'
```
Ardından, istemciyi örneklendirin:
```
var client = new WebSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
```
Görüntü arama ve sonuçları ayrıştırma için istemciyi kullanın:
```
PATH = 'C:\\Users\\USER\\azure-cognitive-samples\\mytestenv\\TestImages\\'
image_path = os.path.join(PATH, "image.jpg")

with open(image_path, "rb") as image_fd:

    # You need to pass the serialized form of the model
    knowledge_request = json.dumps(VisualSearchRequest().serialize())

    print("\r\nSearch visual search request with binary of dog image")
    result = client.images.visual_search(image=image_fd, knowledge_request=knowledge_request)
        
    if not result:
        print("No visual search result data.")

        # Visual Search results
    if result.image.image_insights_token:
        print("Uploaded image insights token: {}".format(result.image.image_insights_token))
    else:
        print("Couldn't find image insights token!")

    # List of tags
    if result.tags:
        first_tag = result.tags[0]
        print("Visual search tag count: {}".format(len(result.tags)))

        # List of actions in first tag
        if first_tag.actions:
            first_tag_action = first_tag.actions[0]
            print("First tag action count: {}".format(len(first_tag.actions)))
            print("First tag action type: {}".format(first_tag_action.action_type))
        else:
            print("Couldn't find tag actions!")
    else:
        print("Couldn't find image tags!")

```

<a name="complete-console"></a> 
## <a name="complete-console-application"></a>Tam konsol uygulaması

Aşağıdaki konsol uygulaması, önceden tanımlanmış sorguyu yürütür ve sonuçları ayrıştırır:
```
import http.client, urllib.parse
import json
import os.path

# Replace the subscriptionKey string value with your valid subscription key.
subscription_key = 'YOUR-VISUAL-SEARCH-ACCESS-KEY'

PATH = 'C:\\Users\\v-gedod\\azure-cognitive-samples\\mytestenv\\TestImages\\'

from azure.cognitiveservices.search.visualsearch import VisualSearchAPI
from azure.cognitiveservices.search.visualsearch.models import (
    VisualSearchRequest,
    CropArea,
    ImageInfo,
    Filters,
    KnowledgeRequest,)

from msrest.authentication import CognitiveServicesCredentials

client = VisualSearchAPI(CognitiveServicesCredentials(subscription_key))

image_path = os.path.join(PATH, "image.jpg")

with open(image_path, "rb") as image_fd:

    # You need to pass the serialized form of the model
    knowledge_request = json.dumps(VisualSearchRequest().serialize())

    print("\r\nSearch visual search request with binary of dog image")
    result = client.images.visual_search(image=image_fd, knowledge_request=knowledge_request)
        
    if not result:
        print("No visual search result data.")

        # Visual Search results
    if result.image.image_insights_token:
        print("Uploaded image insights token: {}".format(result.image.image_insights_token))
    else:
        print("Couldn't find image insights token!")

    # List of tags
    if result.tags:
        first_tag = result.tags[0]
        print("Visual search tag count: {}".format(len(result.tags)))

        # List of actions in first tag
        if first_tag.actions:
            first_tag_action = first_tag.actions[0]
            print("First tag action count: {}".format(len(first_tag.actions)))
            print("First tag action type: {}".format(first_tag_action.action_type))
        else:
            print("Couldn't find tag actions!")
    else:
        print("Couldn't find image tags!")


# Uncomment these methods to include code under the following headings of this documentation.
#search_image_binary_with_crop_area(client, subscription_key, PATH)
#search_url_with_filters(client, subscription_key)
#search_insights_token_with_crop_area(client, subscription_key)

```

Bing arama örnekleri SDK'nın çeşitli özelliklerini gösterir.  Aşağıdaki işlevleri önceden tanımlanmış `VisualSrchSDK` sınıfına ekleyin.

<a name="binary-crop"></a>
## <a name="image-binary-post-with-croparea"></a>cropArea ile görüntü ikili gönderisi

Aşağıdaki kod, gönderi isteğinin gövdesinde cropArea nesnesinin yanı sıra görüntünün ikili verilerini de gönderir.  Ardından imageInsightsToken değerini, etiket sayısını, eylem sayısını ve ilk actionType değerini yazdırır.

```
def search_image_binary_with_crop_area(client, sub_key, file_path):

    #client = VisualSearchAPI(CognitiveServicesCredentials(sub_key))

    image_path = os.path.join(file_path, "image.jpg")
    with open(image_path, "rb") as image_fd:

        crop_area = CropArea(top=0.1,bottom=0.5,left=0.1,right=0.9)
        knowledge_request = VisualSearchRequest(image_info=ImageInfo(crop_area=crop_area))

        # You need to pass the serialized form of the model
        knowledge_request = json.dumps(knowledge_request.serialize())

        print("\r\nSearch visual search request with binary of dog image")
        result = client.images.visual_search(image=image_fd, knowledge_request=knowledge_request)

        if not result:
            print("No visual search result data.")
            return

        # Visual Search results
        if result.image.image_insights_token:
            print("Uploaded image insights token: {}".format(result.image.image_insights_token))
        else:
            print("Couldn't find image insights token!")

        # List of tags
        if result.tags:
            first_tag = result.tags[0]
            print("Visual search tag count: {}".format(len(result.tags)))

            # List of actions in first tag
            if first_tag.actions:
                first_tag_action = first_tag.actions[0]
                print("First tag action count: {}".format(len(first_tag.actions)))
                print("First tag action type: {}".format(first_tag_action.action_type))
            else:
                print("Couldn't find tag actions!")
        else:
            print("Couldn't find image tags!")


```
<a name="knowledge-req"></a>
## <a name="knowledgerequest-parameter"></a>KnowledgeRequest parametresi

Aşağıdaki kod, `knowledgeRequest` parametresinde bir \"site:pinterest.com\" filtresi ile birlikte bir resim URL'si gönderir. Ardından `imageInsightsToken` değerini, etiket sayısını, eylem sayısını ve ilk actionType değerini yazdırır.
```
def search_url_with_filters(client_in, sub_key):

    client = client_in

    image_url = "https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80"
    filters = Filters(site="pinterest.com")

    knowledge_request = VisualSearchRequest(
        image_info=ImageInfo(url=image_url),
        knowledge_request=KnowledgeRequest(filters=filters)
    )

    # You need to pass the serialized form of the model
    knowledge_request = json.dumps(knowledge_request.serialize())

    print("\r\nSearch visual search request with url of dog image")
    result = client.images.visual_search(knowledge_request=knowledge_request)

    if not result:
        print("No visual search result data.")
        return

    # Visual Search results
    if result.image.image_insights_token:
        print("Uploaded image insights token: {}".format(result.image.image_insights_token))
    else:
        print("Couldn't find image insights token!")

    # List of tags
    if result.tags:
        first_tag = result.tags[0]
        print("Visual search tag count: {}".format(len(result.tags)))

        # List of actions in first tag
        if first_tag.actions:
            first_tag_action = first_tag.actions[0]
            print("First tag action count: {}".format(len(first_tag.actions)))
            print("First tag action type: {}".format(first_tag_action.action_type))
        else:
            print("Couldn't find tag actions!")
    else:
        print("Couldn't find image tags!")

```
<a name="tags-actions"></a>
## <a name="tags-actions-and-actiontype"></a>Etiketler, eylemler ve actionType

Aşağıdaki kod, knowledgeRequest parametresinde cropArea nesnesi ile birlikte bir görüntü içgörüleri belirteci gönderir. Ardından imageInsightsToken değerini, etiket sayısını, eylem sayısını ve ilk actionType değerini yazdırır.

```
    client = client_in

    image_insights_token = "bcid_CA6BDBEA28D57D52E0B9D4B254F1DF0D*ccid_6J+8V1zi*thid_R.CA6BDBEA28D57D52E0B9D4B254F1DF0D"
    crop_area = CropArea(top=0.1,bottom=0.5,left=0.1,right=0.9)

    knowledge_request = VisualSearchRequest(
        image_info=ImageInfo(
            image_insights_token=image_insights_token,
            crop_area=crop_area
        ),
    )

    # You need to pass the serialized form of the model
    knowledge_request = json.dumps(knowledge_request.serialize())

    print("\r\nSearch visual search request with URL of dog image")
    result = client.images.visual_search(knowledge_request=knowledge_request)

    if not result:
        print("No visual search result data.")
        return

    # Visual Search results
    if result.image.image_insights_token:
        print("Uploaded image insights token: {}".format(result.image.image_insights_token))
    else:
        print("Couldn't find image insights token!")

    # List of tags
    if result.tags:
        first_tag = result.tags[0]
        print("Visual search tag count: {}".format(len(result.tags)))

        # List of actions in first tag
        if first_tag.actions:
            first_tag_action = first_tag.actions[0]
            print("First tag action count: {}".format(len(first_tag.actions)))
            print("First tag action type: {}".format(first_tag_action.action_type))
        else:
            print("Couldn't find tag actions!")
    else:
        print("Couldn't find image tags!")
```

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel Hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7).