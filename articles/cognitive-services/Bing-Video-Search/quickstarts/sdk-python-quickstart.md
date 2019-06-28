---
title: "Hızlı Başlangıç: Python için Bing Video arama SDK'sını kullanarak video Ara"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Python için Bing Video arama SDK'sını kullanarak video arama istekleri göndermek için kullanın
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 06/26/2019
ms.author: aahi
ms.openlocfilehash: 5fb6b2b349712a04b79564f9b748538a5cc32d73
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446993"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-python"></a>Hızlı Başlangıç: Python için bir video arama Bing Video arama SDK ile gerçekleştirme

Haberler için Bing Video arama SDK ile Python için aramaya başlamak için bu Hızlı Başlangıç'ı kullanın. Bing Video arama çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/video_search_samples.py) ilave ek açıklamaların ve özelliklerine sahip.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="prerequisites"></a>Önkoşullar

- [Python](https://www.python.org/) 2.x veya 3.x
- Python için Bing Video arama SDK'sı

Bir python kullanmanız önerilir [sanal ortam](https://docs.python.org/3/tutorial/venv.html). Yükleme ve sanal bir ortamda başlatmak [venv Modülü](https://pypi.python.org/pypi/virtualenv). Python 2.7 ile virtualenv yükle:

```console
python -m venv mytestenv
```

Bing Video arama SDK'sı ile yükleyin:

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-videosearch
```

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE veya düzenleyici yeni bir Python dosyası oluşturun ve aşağıdaki import deyimlerini girin. 

    ```python
    from azure.cognitiveservices.search.videosearch import VideoSearchAPI
    from azure.cognitiveservices.search.videosearch.models import VideoPricing, VideoLength, VideoResolution, VideoInsightModule
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Abonelik anahtarınız için bir değişken oluşturun. 

    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    ```

## <a name="create-the-search-client"></a>Arama İstemcisi oluşturma

`CognitiveServicesCredentials` nesnesinin bir örneğini oluşturun ve istemcinin bir örneğini başlatın:

```python
client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))
```

## <a name="send-a-search-request-and-get-a-response"></a>Bir arama talebi gönderin ve bir yanıt alın

1. Kullanım `client.videos.search()` arama sorgunuzla Bing Video arama API'si için bir istek gönderir ve bir yanıt alın.

    ```python
    video_result = client.videos.search(query="SwiftKey")
    ```

2. Yanıt arama sonuçları içeriyorsa, ilk alın ve kimliği, adı ve url yazdırın.

    ```python
    if video_result.value:
        first_video_result = video_result.value[0]
        print("Video result count: {}".format(len(video_result.value)))
        print("First video id: {}".format(first_video_result.video_id))
        print("First video name: {}".format(first_video_result.name))
        print("First video url: {}".format(first_video_result.content_url))
    else:
        print("Didn't see any video result data..")
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı bir web uygulaması oluşturma](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

- [Bing Video arama API'si nedir?](../overview.md)
- [Bilişsel Hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
