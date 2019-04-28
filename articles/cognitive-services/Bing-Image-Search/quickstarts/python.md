---
title: "Hızlı Başlangıç: Resimler - Bing resim arama REST API'si ve Python için arama yapın"
titleSuffix: Azure Cognitive Services
description: JSON yanıtlar almasına ve bu hızlı başlangıçta Python kullanarak Bing resim arama REST API'si için görüntü arama istekleri göndermek için kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 02/06/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 0fa60f8dc7a1bb0f72080e91adb1149c1c4c082d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60913811"
---
# <a name="quickstart-search-for-images-using-the-bing-image-search-rest-api-and-python"></a>Hızlı Başlangıç: Bing resim arama REST API'si ve Python kullanarak resimler için arama yapın

Bu hızlı başlangıçta, Bing resim arama API'si için arama istekleri gönderirken başlatmak için kullanın. Python uygulaması bu API için bir arama sorgusu gönderir ve ilk görüntünün URL'sini sonuçları görüntüler. Bu uygulama Python'da yazılmıştır, ancak çoğu programlama dilleri ile uyumlu bir RESTful web hizmeti API'dir.

Bu örneği başlatma Bağlayıcı rozetine tıklayarak [Bağlayıcım](https://mybinder.org)’da bir Jupyter not defteri olarak çalıştırabilirsiniz:

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)


Bu örneğin kaynak kodu, ek hata işleme ve açıklama notları ile [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingImageSearchv7.py)’da bulunabilir.


## <a name="prerequisites"></a>Önkoşullar

* [Python 2.x veya 3.x](https://www.python.org/)
* [Python kitaplığı (PIL) görüntüleme](https://pillow.readthedocs.io/en/stable/index.html)
* [matplotlib](https://matplotlib.org/) 

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE veya düzenleyici yeni bir Python dosyası oluşturun ve aşağıdaki modülleri içeri aktarın. Abonelik anahtarınız için bir değişken oluşturun, uç nokta arayın ve arama terimi.

    ```python
    import requests
    import matplotlib.pyplot as plt
    from PIL import Image
    from io import BytesIO
    
    subscription_key = "your-subscription-key"
    search_url = "https://api.cognitive.microsoft.com/bing/v7.0/images/search"
    search_term = "puppies"
    ```

2. Abonelik anahtarınızı ekleme `Ocp-Apim-Subscription-Key` bir sözlük oluşturma ve anahtar değeri olarak ekleyerek başlığı. 

    ```python
    headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
    ```

## <a name="create-and-send-a-search-request"></a>Oluşturma ve arama isteği gönderme

1. Arama için bir sözlük isteğin parametreleri oluşturun. Arama teriminizi ekleme `q` parametresi. "Genel" için kullanmak `license` genel etki alanındaki görüntülerini aramak için parametre. Kullanmak için "Fotoğraf" `imageType` fotoğraflar için aranacak.

    ```python
    params  = {"q": search_term, "license": "public", "imageType": "photo"}
    ```

2. Kullanım `requests` Bing görüntü arama API'sini çağırmak için kitaplığı. İsteği üst bilgisi ve parametreleri ekleyin ve yanıt olarak bir JSON nesnesi döndürür. 

    ```python
    response = requests.get(search_url, headers=headers, params=params)
    response.raise_for_status()
    search_results = response.json()
    ```

## <a name="view-the-response"></a>Yanıtı görüntüleyin

1. Dört sütun ve matplotlib kitaplığını kullanarak dört satır yeni şekil oluşturun. 

2. Şekil ait satırları ve sütunları yineleyebilir ve PIL kitaplığın kullanın `Image.open()` her alan için bir görüntüyü küçük resim eklemek için yöntemi. 

    ```python
    f, axes = plt.subplots(4, 4)
    for i in range(4):
        for j in range(4):
            image_data = requests.get(thumbnail_urls[i+4*j])
            image_data.raise_for_status()
            image = Image.open(BytesIO(image_data.content))        
            axes[i][j].imshow(image)
            axes[i][j].axis("off")
    ```

3. Kullanım `plt.show()` şekil çizme ve görüntüler.

## <a name="example-json-response"></a>Örnek JSON yanıtı

Bing Resim Arama API'sinden yanıtlar JSON olarak döndürülür. Bu örnek yanıt, tek bir sonuç göstermek için kısaltıldı.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Resim Arama tek sayfalı uygulama öğreticisi](../tutorial-bing-image-search-single-page-app.md)

* [Bing resim arama API'si nedir?](../overview.md)  
* [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/) Bing arama API'leri. 
* [Ücretsiz bir Bilişsel Hizmetler erişim anahtarı alın](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Azure Bilişsel Hizmetler Belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing Resim Arama API’si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
