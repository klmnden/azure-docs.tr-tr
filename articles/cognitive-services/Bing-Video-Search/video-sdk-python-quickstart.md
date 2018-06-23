---
title: Video arama SDK Python hızlı başlangıç | Microsoft Docs
description: Video arama SDK konsol uygulaması kurulumu.
titleSuffix: Azure Video Search SDK Python quickstart
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 02/15/2018
ms.author: v-gedod
ms.openlocfilehash: 1c4769a6ca3391fa595cc078651beff330bbfd60
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355282"
---
# <a name="video-search-sdk-python-quickstart"></a>Video arama SDK Python hızlı başlangıç

Bing görüntü arama SDK web sorguları ve ayrıştırma sonuçları için REST API işlevselliğini içerir.

[Kaynak kodu Python Bing Video arama SDK örnekleri için](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/video_search_samples.py) Git hub'da kullanılabilir.


## <a name="application-dependencies"></a>Uygulama bağımlılıkları
Zaten sahip değilseniz, Python yükleyin. SDK 3.3, 3.4, 3.5 ve 3.6 gibi Python 2.7 ile uyumludur.

Python geliştirme için genel öneri kullanmaktır bir [sanal ortam](https://docs.python.org/3/tutorial/venv.html). Yükleme ve sanal ortamıyla başlatma [venv Modülü](https://pypi.python.org/pypi/virtualenv). Python 2.7 için virtualenv yükleyin.
```
python -m venv mytestenv
```
Bing videolar arama SDK bağımlılıkları yükler:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-videosearch
```
## <a name="video-search-client"></a>Video arama istemci
Alma bir [Bilişsel hizmetler erişim tuşu](https://azure.microsoft.com/try/cognitive-services/) altında *arama*. İçeri aktarmaları ekleyin:
```
subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Bir örneğini oluşturmak `CognitiveServicesCredentials`ve istemci örneği:
```
client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Videoları (SwiftKey) için arama yapın ve ardından sonuçları sayısını doğrulayın. Out yazdırma `ID`, `name` ve `URL` ilk video sonuç.
```
client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))

try:
    video_result = client.videos.search(query="SwiftKey")
    print("Search videos for query \"SwiftKey\"")

    if video_result.value:
        first_video_result = video_result.value[0]
        print("Video result count: {}".format(len(video_result.value)))
        print("First video id: {}".format(first_video_result.video_id))
        print("First video name: {}".format(first_video_result.name))
        print("First video url: {}".format(first_video_result.content_url))
    else:
        print("Didn't see any video result data..")

except Exception as err:
    print("Encountered exception. {}".format(err))

```
(Bellevue toplamı) için kısa ücretsiz, videolar ve 1080 p çözüm arayın. Sonuç sayısı doğrulayın ve yazdırmanız `ID`, `name` ve `URL` ilk video sonuç.
```
def video_search_with_filtering(subscription_key):

    client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        video_result = client.videos.search(
            query="Bellevue Trailer",
            pricing=VideoPricing.free,  # Can use the str "free" too
            length=VideoLength.short,   # Can use the str "short" too
            resolution=VideoResolution.hd1080p  # Can use the str "hd1080p" too
        )
        print("Search videos for query \"Bellevue Trailer\" that is free, short and 1080p resolution")

        if video_result.value:
            first_video_result = video_result.value[0]
            print("Video result count: {}".format(len(video_result.value)))
            print("First video id: {}".format(first_video_result.video_id))
            print("First video name: {}".format(first_video_result.name))
            print("First video url: {}".format(first_video_result.content_url))
        else:
            print("Didn't see any video result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```

Oluşturan eğilim sonuçları alın. Başlık kutucukları ve kategorilere doğrulayın:
```
def video_trending(subscription_key):

    client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        trending_result = client.videos.trending()
        print("Search trending video")

        # Banner tiles
        if trending_result.banner_tiles:
            first_banner_tile = trending_result.banner_tiles[0]
            print("Banner tile count: {}".format(len(trending_result.banner_tiles)))
            print("First banner tile text: {}".format(first_banner_tile.query.text))
            print("First banner tile url: {}".format(first_banner_tile.query.web_search_url))
        else:
            print("Couldn't find banner tiles!")

        # Categorires
        if trending_result.categories:
            first_category = trending_result.categories[0]
            print("Category count: {}".format(len(trending_result.categories)))
            print("First category title: {}".format(first_category.title))
            if first_category.subcategories:
                first_subcategory = first_category.subcategories[0]
                print("Subcategory count: {}".format(len(first_category.subcategories)))
                print("First subcategory title: {}".format(first_subcategory.title))
                if first_subcategory.tiles:
                    first_tile = first_subcategory.tiles[0]
                    print("Subcategory tile count: {}".format(len(first_subcategory.tiles)))
                    print("First tile text: {}".format(first_tile.query.text))
                    print("First tile url: {}".format(first_tile.query.web_search_url))
                else:
                    print("Couldn't find subcategory tiles!")
            else:
                print("Couldn't find subcategories!")
        else:
            print("Couldn't find categories!")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
Videoları (Bellevue toplamı) için arama ve ilk video ayrıntılı bilgi için arama yapın.
```
def video_detail(subscription_key):

    client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        video_result = client.videos.search(query="Bellevue Trailer")
        first_video_result = video_result.value[0]

        video_details = client.videos.details(
            query="Bellevue Trailer",
            id=first_video_result.video_id,
            modules=[VideoInsightModule.all]  # Can use ["all"] too
        )
        print("Search detail for video id={}, name={}".format(
            first_video_result.video_id,
            first_video_result.name
        ))

        if video_details.video_result:
            print("Expected Video id: {}".format(video_details.video_result.video_id))
            print("Expected Video name: {}".format(video_details.video_result.name))
            print("Expected Video url: {}".format(video_details.video_result.content_url))
        else:
            print("Couldn't find expected video")

        if video_details.related_videos.value:
            first_related_video = video_details.related_videos.value[0]
            print("Related video count: {}".format(len(video_details.related_videos.value)))
            print("First related video id: {}".format(first_related_video.video_id))
            print("First related video name: {}".format(first_related_video.name))
            print("First related video content url: {}".format(first_related_video.content_url))
        else:
            print("Couldn't find any related video!")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Python SDK'sı örneği](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

