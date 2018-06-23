---
title: Oluşturan eğilim görüntüleri için web Ara | Microsoft Docs
description: Bing görüntüleri arama API oluşturan eğilim görüntüleri Web'de aramak için nasıl kullanılacağını gösterir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: EAB92D35-5C0B-4A0A-8F49-02DF7FAD44B4
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: b12524cd4c1896501820209b3a45746b8f38b210
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351473"
---
# <a name="get-trending-images"></a>Oluşturan eğilim görüntüleri alma  

Günümüzün oluşturan eğilim görüntüleri almak için aşağıdaki GET isteği gönderin:  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending?mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Eğilim görüntüleri API şu anda yalnızca aşağıdaki pazarda destekler:  

- en-US (İngilizce, Amerika Birleşik Devletleri)  
- tr-CA (İngilizce, Kanada)  
- tr-AU (İngilizce, Avustralya)  
- zh-CN (Çince, Çin)

Yanıtı içeren bir [TrendingImages](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#trendingimages) görüntüleri kategoriye göre listeler nesnesi. Kategorinin kullanmak `title` kullanıcı deneyiminizi görüntülerinde gruplandırmak için. Kategoriler günlük değişebilir.  
  
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
  
Her bölme görüntü ve ilgili görüntüleri almak için seçenekleri içerir. İlgili görüntüleri almak için sorgu kullanabilirsiniz `text` çağırmak için [görüntü arama API](./search-the-web.md) ve ilgili görüntüleri kendiniz görüntüleyin. Veya, URL'de kullanabilirsiniz `webSearchUrl` ilgili görüntüleri içeren Bing'ın görüntüleri arama sonuçları sayfasını, kullanıcıya gerçekleştirilecek. 

İlgili görüntüleri almak için görüntü arama API çağırırsanız, ayarlamak [kimliği](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#id) sorgu parametresi kimliği için `id` alan. Kimliğini belirtme yanıt (bunu ilk yanıt görüntüdür) görüntüsü ve ilişkili görüntüleri içeren sağlar. Ayrıca, [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#q) sorgu parametresi metne `query` nesnenin `text` alan.

Aşağıdaki örnek ilgili Bay Etikan görüntülerinde önceki eğilim görüntüleri API yanıt almak için resim kimliği kullanmayı gösterir.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=Smith&id=77FDE4A1C6529A23C7CF0EC073FAA64843E828F2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  
