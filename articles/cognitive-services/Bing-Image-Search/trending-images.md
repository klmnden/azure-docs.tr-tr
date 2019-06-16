---
title: Bing resim arama API'si ile popüler resimlerden Al
titleSuffix: Azure Cognitive Services
description: Günümüzde Bing resim arama API'si ile web yanından popüler resimler için arama yapın.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: EAB92D35-5C0B-4A0A-8F49-02DF7FAD44B4
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: 2936b94d7ba791b1a4e5a9b95aca3ca3ecdb5904
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66383428"
---
# <a name="get-trending-images-from-the-web"></a>Popüler resimler Web'den Al

Günümüzün popüler resimlerden almak için aşağıdaki GET isteği gönder:  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending?mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Popüler resimler API'si şu anda aşağıdaki borsa destekler:  

- en-US (İngilizce, Amerika Birleşik Devletleri)  
- tr-CA (İngilizce, Kanada)  
- tr-AU (İngilizce, Avustralya)  
- zh-CN (Çince, Çin)

Yanıt içeren bir [TrendingImages](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#trendingimages) kategoriye göre görüntülerini listeleyen bir nesne. Kategori kullanın `title` , kullanıcı deneyiminde görüntülerini gruplandırmak için. Kategoriler her gün değişebilir.  

```json
{
    "_type" : "TrendingImages",  
    "categories" : [{  
        "title" : "Popular people searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Smith",  
                "displayText" : "Mr. Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=smith&FORM=..."
            },  
            "image" : {  
                "id" : "C3C60AE779A054D5CD80D3CACF0F3",  
                "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OIP.M2532...",  
                "contentUrl" : "http:\/\/www.contoso.com.au\/assets\/Uploads\/smith-SH01.jpg",  
                "thumbnail" : {  
                    "width" : 288,  
                    "height" : 300  
                }  
            }  
        },  
        . . .  
        ]  
    },  
    . . .  
    {  
        "title" : "Popular Halloween searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Halloween costumes for adults",  
                "displayText" : "Halloween costumes for adults",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Halloween+costumes..."
            },  
            "image" : {  
                "id" : "0F3395F2983003F89DCEE711B55D7FA53E4",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP.Me429c...",  
                "contentUrl" : "http:\/\/images.domain.com\/products\/8179\/1-1\/adult-squirrel...",  
                "thumbnail" : {  
                    "width" : 336,  
                    "height" : 480  
                }  
            }  
        }]  
    }]  
}  
```  

Her kutucuk, görüntü ve ilgili görüntüleri almak için seçenekleri içerir. İlgili görüntüleri almak için sorguyu kullanabilirsiniz `text` çağrılacak [resim arama API'si](./search-the-web.md) ve ilgili kendiniz görüntüler. Veya URL'de kullandığınız `webSearchUrl` ilgili görüntüleri içeren Bing'in görüntüleri arama sonuçları sayfası, kullanıcıya yapılacak.

İlgili görüntüleri almak için resim arama API'si çağrısı verilirse [kimliği](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#id) sorgu parametresi kimlik `id` alan. Kimliğini belirterek yanıt görüntünün (yanıt ilk görüntüde olduğu) ve ilgili resimlerini içeren sağlar. Ayrıca, [q](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference) sorgu parametresi metinde `query` nesnenin `text` alan.

Aşağıdaki örnek, ilgili Bay Smith görüntülerde önceki popüler resimler API yanıt almak için resim kimliği kullanmayı gösterir.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=Smith&id=77FDE4A1C6529A23C7CF0EC073FAA64843E828F2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  
