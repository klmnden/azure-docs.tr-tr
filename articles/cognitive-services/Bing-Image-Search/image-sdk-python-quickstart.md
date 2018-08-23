---
title: "Hızlı Başlangıç: İstek ve Python SDK'sını kullanarak görüntüleri Filtrele"
description: Bu hızlı başlangıçta, istek ve Python kullanarak Bing resim arama tarafından döndürülen görüntüleri Filtrele.
titleSuffix: Azure Image Search SDK Python quickstart
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/14/2018
ms.author: v-gedod
ms.openlocfilehash: 4729f103bb9b50d4ff039907db8eb677f3dc290a
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "41987486"
---
 # <a name="quickstart-request-and-filter-images-using-the-sdk-and-python"></a>Hızlı Başlangıç: Python ve SDK'sı kullanarak istek ve filtre görüntüleri

Bing görüntü arama SDK'sı web sorgular ve ayrıştırma sonuçları için REST API işlevlerini içerir. 

[Kaynak kodu Python Bing resim arama SDK örnekleri için](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/image_search_samples.py) Git hub'da kullanılabilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları
Zaten sahip değilseniz, Python'ı yükleyin. SDK, 3.3, 3.4, 3.5 ve 3.6 gibi Python 2.7 ile uyumludur.

Python geliştirme için genel öneri kullanmaktır bir [sanal ortam](https://docs.python.org/3/tutorial/venv.html). Yükleme ve sanal ortamıyla başlatma [venv Modülü](https://pypi.python.org/pypi/virtualenv). Python 2.7 için virtualenv yüklemeniz gerekir.
```
python -m venv mytestenv
```
Bing görüntü arama SDK bağımlılıklarını yükleyin:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-imagesearch
```
## <a name="image-search-client"></a>Görüntü arama istemci
Alma bir [Bilişsel hizmetler erişim anahtarını](https://azure.microsoft.com/try/cognitive-services/) altında *arama*. İçeri aktarmaları ekleyin:
```
from azure.cognitiveservices.search.imagesearch import ImageSearchAPI
from azure.cognitiveservices.search.imagesearch.models import ImageType, ImageAspect, ImageInsightModule
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Bir örneğini oluşturmak `CognitiveServicesCredentials`ve istemci örneği:
```
client = ImageSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Animasyonlu GIF'ler ve geniş bir boyut için filtrelenmiş sorgu (Yosemite) görüntülerinde arayın. Sonuç sayısı doğrulayın ve yazdırma insightsToken, küçük resim URL'si ve web ilk sonucunun URL'si.
```
image_results = client.images.search(
        query="Yosemite",
        image_type=ImageType.animated_gif, # Could be the str "AnimatedGif"
        aspect=ImageAspect.wide # Could be the str "Wide"
    )
    print("\r\nSearch images for \"Yosemite\" results that are animated gifs and wide aspect")

    if image_results.value:
        first_image_result = image_results.value[0]
        print("Image result count: {}".format(len(image_results.value)))
        print("First image insights token: {}".format(first_image_result.image_insights_token))
        print("First image thumbnail url: {}".format(first_image_result.thumbnail_url))
        print("First image web search url: {}".format(first_image_result.web_search_url))
    else:
        print("Couldn't find image results!")

```
Animasyonlu GIF'ler ve geniş bir boyut için filtrelenmiş resimler (Yosemite) için arama yapın.  Sonuç sayısı doğrulayın.  Çıkış yazdırma `insightsToken`, `thumbnail url` ve `web url` ilk sonuç.
```
image_results = client.images.search(
    query="Yosemite",
    image_type=ImageType.animated_gif, # Could be the str "AnimatedGif"
    aspect=ImageAspect.wide # Could be the str "Wide"
)
print("\r\nSearch images for \"Yosemite\" results that are animated gifs and wide aspect")

if image_results.value:
    first_image_result = image_results.value[0]
    print("Image result count: {}".format(len(image_results.value)))
    print("First image insights token: {}".format(first_image_result.image_insights_token))
    print("First image thumbnail url: {}".format(first_image_result.thumbnail_url))
    print("First image web search url: {}".format(first_image_result.web_search_url))
else:
    print("Couldn't find image results!")

```

Popüler sonuçları alın:
```
trending_result = client.images.trending()
print("\r\nSearch trending images")

# Categorires
if trending_result.categories:
    first_category = trending_result.categories[0]
    print("Category count: {}".format(len(trending_result.categories)))
    print("First category title: {}".format(first_category.title))
    if first_category.tiles:
        first_tile = first_category.tiles[0]
        print("Subcategory tile count: {}".format(len(first_category.tiles)))
        print("First tile text: {}".format(first_tile.query.text))
        print("First tile url: {}".format(first_tile.query.web_search_url))
    else:
        print("Couldn't find subcategory tiles!")
    else:
        print("Couldn't find categories!")

```

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Python SDK'sı örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)


