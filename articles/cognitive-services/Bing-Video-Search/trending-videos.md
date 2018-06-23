---
title: Web oluşturan eğilim videolar için arama | Microsoft Docs
description: Bing Video arama API oluşturan eğilim videolar Web'de aramak için nasıl kullanılacağını gösterir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 897A28A3-0980-484E-814F-FFE1D5C885E6
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 8db7fcf77042631260b4b165bd3d44053827f3ce
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351418"
---
# <a name="get-trending-videos"></a>Oluşturan eğilim videolar Al  

Günümüzün oluşturan eğilim videolar almak için aşağıdaki GET isteği gönderin:  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending?mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

Aşağıdaki pazarda oluşturan eğilim videolar destekler.  
 
-   tr-AU (İngilizce, Avustralya)  
-   tr-CA (İngilizce, Kanada)  
-   tr-GB (İngilizce, Türkiye)  
-   tr-ID (İngilizce, Endonezya)  
-   tr-IE (İngilizce, İrlanda)  
-   tr-IN (İngilizce, Hindistan)  
-   tr-NZ (İngilizce, Yeni Zelanda)  
-   tr-PH (İngilizce, Filipin)  
-   tr-SG (İngilizce, Singapur)  
-   en-US (İngilizce, Amerika Birleşik Devletleri)  
-   tr WW (İngilizce, Worldwide toplama kodu)  
-   tr-ZA (İngilizce, Güney Afrika)  
-   zh-CN (Çince, Çin)

  
Aşağıdaki örnek oluşturan eğilim videolar içeren bir yanıtı gösterir.  

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
Yanıt, kategori ve alt kategori tarafından videolar listesini içerir. Örneğin, bir müzik videoları kategorisi kategori listesi içeriyordu ve onun alt kategorileri üst birinde kullanıcı deneyiminizi üst müzik videoları kategorisi oluşturabilirsiniz. Daha sonra kullanabilirsiniz `thumbnailUrl`, `displayText`, ve `webSearchUrl` her kategori (örneğin, üst müzik videoları) altında tıklanabilir bir bölme oluşturmak için alanları. Kullanıcı döşeme tıkladığında, burada video oynanan Bing'ın video tarayıcıya yönlendirilirsiniz.

Yanıt en popüler oluşturan eğilim videolar olan başlık videolar de içerir. Başlık videoları, bir veya daha fazla kategorileri gelebilir.  
  
