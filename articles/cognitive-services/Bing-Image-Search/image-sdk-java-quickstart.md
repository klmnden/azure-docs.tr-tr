---
title: "Hızlı Başlangıç: Java ve Bing resim arama SDK'sını kullanarak görüntüleri arayın"
description: Bu hızlı başlangıçta, arama ve Bing resim arama SDK'sı ve Java kullanarak web üzerinde görüntüleri bulmak için kullanın.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: 12bd6f9a9a0b43b4571a7e0311ffbea54c7b9054
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574070"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-java"></a>Hızlı Başlangıç: Java ve Bing resim arama SDK'sı ile görüntüleri arayın

Bing görüntü arama API'si için bir sarmalayıcı olan ve aynı özellikleri içeren SDK'yı kullanarak ilk görüntü arama yapmak için bu Hızlı Başlangıç'ı kullanın. Bu basit bir Java uygulama bir resim arama sorgusu gönderir, JSON yanıtı ayrıştırır ve döndürülen ilk görüntünün URL'sini görüntüler.

Bu örnek için kaynak kodu kullanılabilir [github'da](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingImageSearch/Quickstart) ek hata işleme ve ek açıklamalar. 

## <a name="prerequisites"></a>Önkoşullar 

En son sürümünü [Java Development Kit](http://www.oracle.com/technetwork/java/javase/downloads/index.html) (JDK)

Bing görüntü arama SDK bağımlılıklarını, Maven, Gradle veya başka bir bağımlılık yönetim sistemi kullanarak yükleyin. Maven POM dosyası aşağıdaki bildirimi gerektirir:

```xml
 <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.cognitiveservices</groupId>
      <artifactId>azure-cognitiveservices-imagesearch</artifactId>
      <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
 </dependencies> 
```

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Oluşturma ve uygulama başlatma

1. Sık kullandığınız IDE veya düzenleyici yeni bir Java projesi oluşturun ve aşağıdaki içeri aktarmaları için sınıf implimentation ekleyin:

    ```java
    import com.microsoft.azure.cognitiveservices.search.imagesearch.BingImageSearchAPI;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.BingImageSearchManager;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.models.ImageObject;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.models.ImagesModel;
    ```

2. Ana yönteminiz için abonelik anahtarınızı değişkenleri oluşturma ve arama terimi. Ardından Bing resim arama istemci örneği oluşturun.

    ```java
    final String subscriptionKey = "COPY_YOUR_KEY_HERE";
    String searchTerm = "canadian rockies";
    //Image search client
    BingImageSearchAPI client = BingImageSearchManager.authenticate(subscriptionKey);
    ```

## <a name="send-a-search-request-to-the-bing-image-search-api"></a>Bing resim arama API'si için bir arama isteği gönder

1. Kullanarak `bingImages().search()`, arama sorgusu içeren HTTP isteği gönder. Yanıt olarak kaydetmek bir `ImagesModel`.
    ```java
    ImagesModel imageResults = client.bingImages().search()
                .withQuery(searchTerm)
                .withMarket("en-us")
                .execute();
    ```

## <a name="parse-and-view-the-result"></a>Ayrıştırma ve sonucu görüntülemek

Yanıtta döndürülen resim sonuçları ayrıştırılamıyor.
Yanıt arama sonuçları içeriyorsa, ilk sonucu depolar ve bir küçük resim gibi ayrıntılarını yazdırmak URL, toplam sayısını özgün URL'yi döndürülen görüntüler.  

```java
if (imageResults != null && imageResults.value().size() > 0) {
    // Image results
    ImageObject firstImageResult = imageResults.value().get(0);

    System.out.println(String.format("Total number of images found: %d", imageResults.value().size()));
    System.out.println(String.format("First image thumbnail url: %s", firstImageResult.thumbnailUrl()));
    System.out.println(String.format("First image content url: %s", firstImageResult.contentUrl()));
}
else {
        System.out.println("Couldn't find image results!");
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing resim arama tek sayfalı uygulama Öğreticisi](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing resim arama nedir?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Çevrimiçi bir etkileşimli Tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Ücretsiz bir Bilişsel hizmetler erişim anahtarını alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api) 
* [Azure Bilişsel hizmetler SDK için Java örnekleri](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
* [Azure Bilişsel hizmetler belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing resim arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
