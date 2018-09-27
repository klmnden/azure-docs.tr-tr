---
title: Popüler videolar - Bing Video arama Web'de arayın
titlesuffix: Azure Cognitive Services
description: Bing Video arama API'si Web'de popüler videolar için arama için nasıl kullanılacağını gösterir.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 8a6ccc9ea8cf9468d7638360c9db8131bc6dc5be
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47222055"
---
# <a name="get-trending-videos"></a>Popüler videoları alma  

Günümüzün popüler videolar almak için aşağıdaki GET isteği gönder:  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending?mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

Popüler videolar aşağıdaki pazarlar destekler.  
 
-   tr-AU (İngilizce, Avustralya)  
-   tr-CA (İngilizce, Kanada)  
-   en-GB (İngilizce, Büyük Britanya)  
-   tr-ID (İngilizce, Endonezya)  
-   tr-IE (İngilizce, İrlanda)  
-   tr-giriş (İngilizce, Hindistan)  
-   tr-NZ (İngilizce, Yeni Zelanda)  
-   tr-PH (İngilizce, Filipinler)  
-   tr-SG (İngilizce, Singapur)  
-   en-US (İngilizce, Amerika Birleşik Devletleri)  
-   tr WW (İngilizce, Worldwide toplama kod)  
-   tr-ZA (İngilizce, Güney Afrika)  
-   zh-CN (Çince, Çin)

  
Aşağıdaki örnek, popüler videolar içeren bir yanıt gösterir.  

```  
{  
    "_type" : "TrendingVideos",  
    "bannerTiles" : [
        {  
            "query" : {  
                "text" : "Hello - Smith",  
                "displayText" : "Hello - Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
            },  
            "image" : {  
                "description" : "Image from: contosowallpapers.com",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=RsA%2fdPlTmx4zS...",  
                "headLine" : "\"Hello\" is a song by...",  
                "contentUrl" : "http:\/\/www.contosowallpapers.com\/wp-content..."  
            }  
        },  
        . . .  
    ],  
    "categories" : [
        {  
            "title" : "Music videos",  
            "subcategories" : [
                {  
                    "tiles" : [
                        {  
                            "query" : {  
                                "text" : "Song Title - Artist Name",  
                                "displayText" : "Song Title - Artist Name",  
                                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
                            },  
                            "image" : {  
                                "description" : "Image from: contoso.com",  
                                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=...",  
                                "contentUrl" : "http:\/\/images6.contoso.com\/image..."  
                            }  
                        },  
                        . . .  
                    ],
                    "title" : "Top "  
                },
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        {
            "title" : "Viral videos",
            "subcategories" : [
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        . . .  
    ]  
}  
  
```  
Yanıt, kategori ve alt kategori tarafından videoların bir listesini içerir. Örneğin, müzik, videolar kategorisi kategori listesi yer alan ve alt kategorilerinin üst biriydi kullanıcı deneyiminizi bir müzik videoları üst kategori oluşturabilirsiniz. Ardından kullanabileceğinizi `thumbnailUrl`, `displayText`, ve `webSearchUrl` her kategorisi (örneğin, üst müzik videoları) altında tıklanabilir bir kutucuğu oluşturmak için alanları. Kullanıcı kutucuğa tıkladığında, burada video oynatılır Bing'in video tarayıcıya yönlendirilirsiniz.

Yanıt, ayrıca en popüler popüler videolar olan başlığı videoları içerir. Başlık videoları, bir veya daha fazla kategori gelebilir.  
  
