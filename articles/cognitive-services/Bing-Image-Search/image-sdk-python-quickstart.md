---
title: "Hızlı Başlangıç: Python ve Bing resim arama SDK'sını kullanarak görüntüleri arayın"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, arama ve Bing resim arama SDK'sını ve Python kullanarak web üzerinde görüntüleri bulmak için kullanın.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: 4a24f1e4e051b627034f1d4664e94e0f47c43014
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45578302"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-python"></a>Hızlı Başlangıç: Python ve Bing resim arama SDK'sı ile görüntüleri arayın

Bing görüntü arama API'si için bir sarmalayıcı olan ve aynı özellikleri içeren SDK'yı kullanarak ilk görüntü arama yapmak için bu Hızlı Başlangıç'ı kullanın. Bu basit bir Python uygulaması bir görüntü arama sorgusu gönderir, JSON yanıtı ayrıştırır ve döndürülen ilk görüntünün URL'sini görüntüler.

Bu örnek için kaynak kodu kullanılabilir [github'da](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/image-search-quickstart.py) ek hata işleme ve ek açıklamalar.

## <a name="prerequisites"></a>Önkoşullar 

* [Python 2.7 veya 3.4](https://www.python.org/) ve daha yüksek.

* [Azure görüntü arama SDK](https://pypi.org/project/azure-cognitiveservices-search-imagesearch/) Python için
    * Kullanarak yükleme `pip install azure-cognitiveservices-search-imagesearch`

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Oluşturma ve uygulama başlatma

1. En sevdiğiniz IDE veya düzenleyici ve aşağıdaki içeri aktarmaları yeni bir Python betiği oluşturun:

    ```python
    from azure.cognitiveservices.search.imagesearch import ImageSearchAPI
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Abonelik anahtarı ve arama terimi için değişkenler oluşturun.

    ```python
    subscription_key = "Enter your key here"
    search_term = "canadian rockies"
    ```

## <a name="create-the-image-search-client"></a>Görüntü arama istemcisi oluşturma

3. Bir örneğini oluşturmak `CognitiveServicesCredentials`ve istemci örneği oluşturmak için kullanın:

    ```python
    client = ImageSearchAPI(CognitiveServicesCredentials(subscription_key))
    ```
4. Bing resim arama API'si için bir arama sorgusu gönderin:
    ```python
    image_results = client.images.search(query=search_term)
    ```
## <a name="process-and-view-the-results"></a>İşlem ve sonuçları görüntüleme

Yanıtta döndürülen resim sonuçları ayrıştırılamıyor.


Yanıt arama sonuçları içeriyorsa, ilk sonucu depolar ve bir küçük resim gibi ayrıntılarını yazdırmak URL, toplam sayısını özgün URL'yi döndürülen görüntüler.  

```python
if image_results.value:
    first_image_result = image_results.value[0]
    print("Total number of images returned: {}".format(len(image_results.value)))
    print("First image thumbnail url: {}".format(first_image_result.thumbnail_url))
    print("First image content url: {}".format(first_image_result.content_url))
else:
    print("No image results returned!")
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing resim arama tek sayfalı uygulama Öğreticisi](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing resim arama nedir?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Çevrimiçi bir etkileşimli Tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Ücretsiz bir Bilişsel hizmetler erişim anahtarını alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)
* [Azure Bilişsel hizmetler SDK için Python örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)  
* [Azure Bilişsel hizmetler belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing resim arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)