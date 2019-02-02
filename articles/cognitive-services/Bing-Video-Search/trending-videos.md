---
title: Bing Video arama API'si kullanarak popüler videolar için Web'de arama
titlesuffix: Azure Cognitive Services
description: Bing Video arama API'si, Web'de popüler videolar için arama yapmak için kullanmayı öğrenin.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: scottwhi
ms.openlocfilehash: 55f29a31a9574c7c32e6708565e961950520593f
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2019
ms.locfileid: "55566117"
---
# <a name="get-trending-videos-with-the-bing-video-search-api"></a>Bing Video arama API'si ile popüler videolar Al 

Bing Video arama API'si, web üzerinde ve farklı kategorilerdeki günümüzün popüler videoları bulmanıza olanak sağlar. 

## <a name="get-request"></a>ALMA isteği

Bing Video arama API'si günümüzün popüler videolar almak için aşağıdaki GET isteği gönder:  
  
```cURL
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending?mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="market-support"></a>Pazar desteği

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

## <a name="example-json-response"></a>Örnek JSON yanıtı  

Aşağıdaki örnek, kategori ve alt kategori tarafından listelenen popüler videolar içeren bir API yanıtı gösterir. Yanıt, ayrıca en popüler popüler videolar ve bir veya daha fazla kategorilerden gelebilir, başlık videoları içerir.  

```json
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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
[Video Öngörüler elde edin](video-insights.md)